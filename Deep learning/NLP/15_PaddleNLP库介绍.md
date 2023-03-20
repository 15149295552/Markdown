# PaddlePaddle2.x

## 一、综合案例

### 1. 波士顿房价预测

```python
# 加载飞桨、Numpy和相关类库
import paddle
from paddle.nn import Linear
import paddle.nn.functional as F
import numpy as np
import os
import random
import matplotlib.pyplot as plt

def load_data():
    # 从文件导入数据
    datafile = './housing.data'
    data = np.fromfile(datafile, sep=' ', dtype=np.float32)

    # 每条数据包括14项，其中前面13项是影响因素，第14项是相应的房屋价格中位数
    feature_names = ['CRIM', 'ZN', 'INDUS', 'CHAS', 'NOX', 'RM', 'AGE',
                     'DIS', 'RAD', 'TAX', 'PTRATIO', 'B', 'LSTAT','MEDV']
    feature_num = len(feature_names)

    # 将原始数据进行Reshape，变成[N, 14]这样的形状
    data = data.reshape([data.shape[0] // feature_num, feature_num])

    # 将原数据集拆分成训练集和测试集
    # 这里使用80%的数据做训练，20%的数据做测试
    # 测试集和训练集必须是没有交集的
    ratio = 0.8
    offset = int(data.shape[0] * ratio)
    training_data = data[:offset]

    # 计算train数据集的最大值，最小值，平均值
    maximums = training_data.max(axis=0)
    minimums = training_data.min(axis=0)
    avgs = training_data.sum(axis=0) / training_data.shape[0]

    # 记录数据的归一化参数，在预测时对数据做归一化
    global max_values
    global min_values
    global avg_values
    max_values = maximums
    min_values = minimums
    avg_values = avgs

    # 对数据进行归一化处理
    for i in range(feature_num):
        data[:, i] = (data[:, i] - avgs[i]) / (maximums[i] - minimums[i])

    # 训练集和测试集的划分比例
    training_data = data[:offset]
    test_data = data[offset:]

    return training_data, test_data


class Regressor(paddle.nn.Layer):

    # self代表类的实例自身
    def __init__(self):
        # 初始化父类中的一些参数
        super(Regressor, self).__init__()

        # 定义一层全连接层，输入维度是13，输出维度是1
        self.fc = Linear(in_features=13, out_features=1)

    # 网络的前向计算
    def forward(self, inputs):
        x = self.fc(inputs)
        return x


# 声明定义好的线性回归模型
model = Regressor()
# 开启模型训练模式
model.train()
# 加载数据
training_data, test_data = load_data()
# 定义优化算法，使用随机梯度下降SGD
# 学习率设置为0.01
opt = paddle.optimizer.SGD(learning_rate=0.01, parameters=model.parameters())

EPOCH_NUM = 80  # 设置外层循环次数
BATCH_SIZE = 10  # 设置batch大小

iters = []
costs = []

# 定义外层循环
i = 0
for epoch_id in range(EPOCH_NUM):

    np.random.shuffle(training_data) # 随机打乱样本

    # 将训练数据进行拆分，每个batch包含10条数据
    mini_batches = [training_data[k:k + BATCH_SIZE] for k in range(0, len(training_data), BATCH_SIZE)]

    # 定义内层循环
    for iter_id, mini_batch in enumerate(mini_batches):
        i += 1
        iters.append(i)
        # print("shape:", mini_batch.shape) # (10, 14)
        x = np.array(mini_batch[:, :-1])  # 获得当前批次训练数据
        y = np.array(mini_batch[:, -1:])  # 获得当前批次训练标签（真实房价）

        # 将numpy数据转为飞桨动态图tensor形式
        house_features = paddle.to_tensor(x)
        prices = paddle.to_tensor(y)

        # 前向计算
        predicts = model(house_features)

        # 计算损失
        loss = F.square_error_cost(predicts, label=prices)
        avg_loss = paddle.mean(loss)
        costs.append( avg_loss.numpy()[0])
        if iter_id % 20 == 0:
            print("epoch: {}, iter: {}, loss is: {}".format(epoch_id, iter_id, avg_loss.numpy()))

        avg_loss.backward() # 反向传播
        opt.step() # 最小化loss,更新参数
        opt.clear_grad() # 清除梯度

# 保存模型参数，文件名为LR_model.pdparams
paddle.save(model.state_dict(), 'LR_model.pdparams')
print("模型保存成功，模型参数保存在LR_model.pdparams中")


def load_one_example():
    # 从上边已加载的测试集中，随机选择一条作为测试数据
    idx = np.random.randint(0, test_data.shape[0])
    idx = -10
    one_data, label = test_data[idx, :-1], test_data[idx, -1]
    # 修改该条数据shape为[1,13]
    one_data = one_data.reshape([1, -1])

    return one_data, label


# 参数为保存模型参数的文件地址
model_dict = paddle.load('LR_model.pdparams')
model.load_dict(model_dict) #从state_dict设置参数和持久缓冲区
model.eval() # 设置为评估模式, 只影响某些模块，如'Dropout'和'BatchNorm'

# 参数为数据集的文件地址
one_data, label = load_one_example()
# 将数据转为动态图的variable格式
one_data = paddle.to_tensor(one_data)
predict = model(one_data)

# 对结果做反归一化处理
predict = predict * (max_values[-1] - min_values[-1]) + avg_values[-1]
# 对label数据做反归一化处理
label = label * (max_values[-1] - min_values[-1]) + avg_values[-1]

print("Inference result is {}, the corresponding label is {}".format(predict.numpy(), label))

# 绘图
plt.figure("Trainning")
plt.title("Trainning")
plt.plot(iters, costs, color="red", label="cost")
plt.legend()
plt.savefig("trainning.png")
plt.show()
```



# PaddleNLP开源库

## 一、基本介绍

### 支持多种内置数据集

见：https://paddlenlp.readthedocs.io/zh/latest/data_prepare/dataset_list.html

## 二、基本操作

### 1. collect

#### 1）Stack

Stack用于把输入样本叠加成一个批次，输入样本必须具有相同的长度

```python
from paddlenlp.data import Stack

a = [1, 2, 3, 4]
b = [3, 4, 5, 6]
c = [5, 6, 7, 8]
result = Stack()([a, b, c])
print(result)
```

执行结果：

```
[[1, 2, 3, 4],
 [3, 4, 5, 6],
 [5, 6, 7, 8]]
```

#### 2）Pad

Pad用于将某个元素填充成和最长元素相等的长度

```python
from paddlenlp.data import Pad

a = [1, 2, 3, 4]
b = [5, 6, 7]
c = [8, 9]
result = Pad(pad_val=0)([a, b, c])
print(result)
```

执行结果：

```
[[1, 2, 3, 4],
 [5, 6, 7, 0],
 [8, 9, 0, 0]]
```

#### 3）Tuple

将多个batchify函数包装在一起，输入功能将应用于相应的输入字段

```python
from paddlenlp.data import Stack, Pad, Tuple

data = [
        [[1, 2, 3, 4], [1]],
        [[5, 6, 7], [0]],
        [[8, 9], [1]],
       ]
batchify_fn = Tuple(Pad(pad_val=0), Stack()) # 第一个数据填充，第二个数据堆叠
ids, label = batchify_fn(data)
print(ids)
print(label)
```

执行结果：

```
ids:
[[1, 2, 3, 4],
[5, 6, 7, 0],
[8, 9, 0, 0]]
label: [[1], [0], [1]]
```

#### 4）Dict

将多个batchify函数包装在一起，输入功能将应用于相应的输入字段。根据不同字段名称应用响应的处理。

```python
from paddlenlp.data import Stack, Pad, Dict

data = [
        {'labels':[1], 'token_ids':[1, 2, 3, 4]},
        {'labels':[0], 'token_ids':[5, 6, 7]},
        {'labels':[1], 'token_ids':[8, 9]},
       ]
batchify_fn = Dict({'token_ids':Pad(pad_val=0), # 对token_ids元素填充
                    'labels':Stack()})#对labels元素堆叠
ids, label = batchify_fn(data)
print(ids)
print(label)
```

执行结果：

```
ids:
[[1, 2, 3, 4],
[5, 6, 7, 0],
[8, 9, 0, 0]]
label: [[1], [0], [1]]
```

### 2. Vocab

从词语转换为索引（顺序数字），或从索引转换为词语

```python
from paddlenlp.data import Vocab

# 数据集地址 https://bj.bcebos.com/paddlenlp/data/senta_word_dict.txt

vocab_file_path = './senta_word_dict.txt'

vocab = Vocab.load_vocabulary(  # Initialize the Vocab
    vocab_file_path,
    unk_token='[UNK]',
    pad_token='[PAD]')
tokens = vocab.to_tokens([0, 1, 2, 3])
print(tokens)  # ['[PAD]', '[UNK]', '一斤三', '意面屋']

tokens = vocab.to_indices(['[PAD]', '[UNK]', '一斤三', '意面屋'])
print(tokens)  # [0, 1, 2, 3]
```

执行结果：

```
['[PAD]', '[UNK]', '一斤三', '意面屋']
[0, 1, 2, 3]
```

### 3. 数据集读取

#### 1）读取内置数据集

```python
from paddlenlp.datasets import load_dataset
# 加载名称为msra_ner的内置数据集
train_ds, test_ds = load_dataset("msra_ner", splits=("train", "test"))
```

#### 2）加载本地数据集

```python
from paddlenlp.datasets import load_dataset
# 读取和squad格式相同的本地数据集
train_ds, dev_ds = load_dataset("squad", 
                                data_files=("my_train_file.json", 
                                            "my_dev_file.json"))
test_ds = load_dataset("squad", data_files="my_test_file.json")
```

#### 3）从本地文件创建数据集

【直接通过函数读取】

```python
from paddlenlp.datasets import load_dataset

def read(data_path):
    with open(data_path, 'r', encoding='utf-8') as f:        
        next(f) # 跳过列名
        for line in f:
            words, labels = line.strip('\n').split('\t')
            words = words.split('\002')
            labels = labels.split('\002')
            yield {'tokens': words, 'labels': labels}

# data_path为read()方法的参数
map_ds = load_dataset(read, data_path='train.txt',lazy=False)
iter_ds = load_dataset(read, data_path='train.txt',lazy=True)
```

【通过继承系统类读取】

```python
from paddle.io import Dataset
from paddlenlp.datasets import MapDataset

class MyDataset(Dataset):
    def __init__(self, path):

        def load_data_from_source(path):
            ...
            ...
            return data

        self.data = load_data_from_source(path)

    def __getitem__(self, idx):
        return self.data[idx]

    def __len__(self):
        return len(self.data)

ds = MyDataset(data_path)  # paddle.io.Dataset
new_ds = MapDataset(ds)    # paddlenlp.datasets.MapDataset
```

### 4. 数据处理

Dataset中通常为原始数据，需要经过一定的数据处理并进行采样组batch，而后通过paddle.io.DataLoader为训练或预测使用，PaddleNLP中为其中各环节提供了相应的功能支持。

#### 1）基于预训练模型的数据处理

```python
from paddlenlp.transformers import BertTokenizer
from paddlenlp.datasets import load_dataset

tokenizer = BertTokenizer.from_pretrained('bert-base-chinese')
train_ds = load_dataset('lcqmc', splits='train')

print(train_ds[0]) # {'query': '喜欢打篮球的男生喜欢什么样的女生', 'title': '爱打篮球的男生喜欢什么样的女生', 'label': 1}

def convert_example(example, tokenizer):
    tokenized_example = tokenizer(text=example['query'],
                                  text_pair=example['title'])
    # 加上label用于训练
    tokenized_example['label'] = [example['label']]
    return tokenized_example

from functools import partial

trans_func = partial(convert_example,
                     tokenizer=tokenizer)

train_ds.map(trans_func)
print(train_ds[0])
```

执行结果：

```python
{'input_ids': [101, 1599, 3614, 2802, 5074, 4413, 4638, 4511, 4495,1599, 3614, 784, 720, 3416, 4638, 1957, 4495, 102,4263, 2802, 5074, 4413, 4638, 4511, 4495, 1599, 3614,784, 720, 3416, 4638, 1957, 4495, 102],
'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
'label': [1]}
```

#### 2）基于非预训练模型的数据处理

```python
from paddlenlp.data import JiebaTokenizer, Vocab
from paddlenlp.datasets import load_dataset

train_ds = load_dataset('chnsenticorp', splits='train')

print(train_ds[0]) # {'text': '选择珠江花园的原因就是方便，有电动扶梯直接到达海边，周围餐馆、食廊、商场、超市、摊位一应俱全。酒店装修一般，但还算整洁。 泳池在大堂的屋顶，因此很小，不过女儿倒是喜欢。 包的早餐是西式的，还算丰富。服务吗，一般', 'label': 1}

# 从本地词典文件构建Vocab
vocab = Vocab.load_vocabulary('./senta_word_dict.txt', 
                              unk_token='[UNK]', 
                              pad_token='[PAD]')

# 使用Vocab初始化JiebaTokenizer
tokenizer = JiebaTokenizer(vocab)

def convert_example(example, tokenizer):
    input_ids = tokenizer.encode(example["text"])
    valid_length = [len(input_ids)]
    label = [example["label"]]
    return input_ids, valid_length, label

trans_fn = partial(convert_example, tokenizer=tokenizer)
train_ds.map(trans_fn)

print(train_ds[0])
```

执行结果：

```python
(
[417329, 128448, 140437, 173188, 118001, 213058, 595790, 1106339, 940533, 947744, 169206, 421258, 908089, 982848, 1106339, 35413, 1055821, 4782, 377145, 4782, 238721, 4782, 642263, 4782, 891683, 767091, 4783, 672971, 774154, 1250380, 1106339, 340363, 146708, 1081122, 4783, 1, 943329, 1008467, 319839, 173188, 909097, 1106339, 1010656, 261577, 1110707, 1106339, 770761, 597037, 1068649, 850865, 4783, 1, 993848, 173188, 689611, 1057229, 1239193, 173188, 1106339, 146708, 427691, 4783, 1, 724601, 179582, 1106339, 1250380],
[67],
[1])
```

## 三、综合项目

### 1）快递单实体抽取

【链接地址】https://aistudio.baidu.com/aistudio/projectdetail/3226505?contributionType=1

【数据集】训练集1800笔、测试集200笔经过标注的快递单数据，示例如下：

```
text_a	label
黑龙江省双鸭山市尖山区八马路与东平行路交叉口北40米韦业涛1860000xxxx	A1-BA1-IA1-IA1-IA2-BA2-IA2-IA2-IA3-BA3-IA3-IA4-BA4-IA4-IA4-IA4-IA4-IA4-IA4-IA4-IA4-IA4-IA4-IA4-IA4-IA4-IP-BP-IP-IT-BT-IT-IT-IT-IT-IT-IT-IT-IT-IT-I
......
```

实体标记规则：

| 实体类型 | 符号 | 示例           |
| -------- | ---- | -------------- |
| 姓名     | P    | 张三           |
| 电话     | T    | 13512345678    |
| 省       | A1   | 广东省         |
| 市       | A2   | 深圳市         |
| 区       | A3   | 南山区         |
| 详细地址 | A4   | XXXX路XXXX大厦 |

位置标记规则：

| 符号 | 位置           |
| ---- | -------------- |
| B    | 起始位置       |
| I    | 中间或结束位置 |
| O    | 无关字符       |

标注示例：

```
喻晓*			P-BP-IP-I
云南省			A1-BA1-IA1-I
楚雄彝族自治州	A2-BA2-IA2-IA2-IA2-IA2-IA2-I
南华县			A3-BA3-IA3-I
东街古城路37号	A4-BA4-IA4-IA4-IA4-IA4-IA4-IA4-I
1851338xxxx	T-BT-IT-IT-IT-IT-IT-IT-IT-IT-IT-I
```

【代码】（增加注释，未做改动）：

第一步：安装paddlenlp

```shell
!pip install paddlenlp==2.1.0
```

第二步：下载并解压数据集

```python
from paddle.utils.download import get_path_from_url
URL = "https://paddlenlp.bj.bcebos.com/paddlenlp/datasets/waybill.tar.gz"
get_path_from_url(URL, "./")
```

第三步：训练

```python
# 预训练ERNIE模型实现快递单检测

# 下载并解压数据集
from paddle.utils.download import get_path_from_url

URL = "https://paddlenlp.bj.bcebos.com/paddlenlp/datasets/waybill.tar.gz"
get_path_from_url(URL, "./")

##################### 加载数据集 #######################
from functools import partial

import paddle
from paddlenlp.datasets import MapDataset
from paddlenlp.data import Stack, Tuple, Pad
from paddlenlp.transformers import ErnieTokenizer, ErnieForTokenClassification
from paddlenlp.metrics import ChunkEvaluator
from utils import convert_example, evaluate, predict, load_dict


def load_dataset(datafiles):
    def read(data_path):
        with open(data_path, 'r', encoding='utf-8') as fp:
            next(fp)  # 跳过第一行
            for line in fp.readlines():
                words, labels = line.strip('\n').split('\t')
                words = words.split('\002')
                labels = labels.split('\002')
                yield words, labels

    # 根据datafiles决定读取单个文件还是所有文件
    if isinstance(datafiles, str):
        return MapDataset(list(read(datafiles)))
    elif isinstance(datafiles, list) or isinstance(datafiles, tuple):
        return [MapDataset(list(read(datafile))) for datafile in datafiles]


# Create dataset, tokenizer and dataloader.
# 返回MapDataset对象
train_ds, dev_ds, test_ds = load_dataset(datafiles=('./data/train.txt', './data/dev.txt', './data/test.txt'))

# 打印
# 每条数据包含一句文本和这个文本中每个汉字以及数字对应的label标签
# 之后，还需要对输入句子进行数据处理，如切词，映射词表id等
for i in range(5):
    print(train_ds[i])

# 数据读入
label_vocab = load_dict('./data/tag.dic')
# Tokenizer用于将原始输入文本转化成模型可以接受的输入数据形式。
# PaddleNLP对于各种预训练模型已经内置了相应的Tokenizer，指定想要使用的模型名字即可加载
tokenizer = ErnieTokenizer.from_pretrained('ernie-1.0')

# partial函数：为convert_example函数设置固定参数
trans_func = partial(convert_example, tokenizer=tokenizer, label_vocab=label_vocab)

# 通过调用trans_func对样本进行转换
# map方法：执行指定的函数，并用结果进行替换
train_ds.map(trans_func)
dev_ds.map(trans_func)
test_ds.map(trans_func)
print(train_ds[0])

# Pad类：将输入的数据样本按照指定axis填充成最大长度，pad_val参数为要填充的值
# Stack: 堆叠
ignore_label = -1
batchify_fn = lambda samples, fn=Tuple(  # 创建元组
    Pad(axis=0, pad_val=tokenizer.pad_token_id),  # input_ids
    Pad(axis=0, pad_val=tokenizer.pad_token_type_id),  # token_type_ids
    Stack(),  # seq_len (Stack将样本堆叠成一个批次)
    Pad(axis=0, pad_val=ignore_label)  # labels
): fn(samples)

# 对数据集划分批次
# DataLoader：返回一个迭代器，每次从数据集返回一个批次数据
train_loader = paddle.io.DataLoader(dataset=train_ds,  # 数据集
                                    batch_size=36,  # 批次大小
                                    return_list=True,  # 是否返回列表
                                    collate_fn=batchify_fn)  # 堆叠成小批次,该参数为0，则在第0维上堆叠
dev_loader = paddle.io.DataLoader(dataset=dev_ds,
                                  batch_size=36,
                                  return_list=True,
                                  collate_fn=batchify_fn)
test_loader = paddle.io.DataLoader(dataset=test_ds,
                                   batch_size=36,
                                   return_list=True,
                                   collate_fn=batchify_fn)

# 加载预训练模型
# Define the model netword and its loss
model = ErnieForTokenClassification.from_pretrained("ernie-1.0", num_classes=len(label_vocab))
# 设置Fine-Tune优化策略，模型配置
# ChunkEvaluator: 用于设置每个chunk检测精度、召回率、F1等指标
metric = ChunkEvaluator(label_list=label_vocab.keys(), suffix=True)
# ignore_index指定哪些target值忽略不计算loss，负数表示不忽略
loss_fn = paddle.nn.loss.CrossEntropyLoss(ignore_index=ignore_label)
# AdamW: 可以解决Adam优化器中L2正则化失败的问题
optimizer = paddle.optimizer.AdamW(learning_rate=2e-5, parameters=model.parameters())

# 模型训练与评估
step = 0
for epoch in range(10):
    for idx, (input_ids, token_type_ids, length, labels) in enumerate(train_loader):
        logits = model(input_ids, token_type_ids)  # 预测
        loss = paddle.mean(loss_fn(logits, labels))  # 指定loss
        loss.backward()  # 反向传播计算
        optimizer.step()  # 优化器更新参数
        optimizer.clear_grad()  # 清楚优化器梯度，否则梯度会累积
        step += 1
        print("epoch:%d - step:%d - loss: %f" % (epoch, step, loss))

    evaluate(model, metric, dev_loader)  # 评估

    paddle.save(model.state_dict(),  # 将所有待持久化参数写入字典中
                './ernie_result/model_%d.pdparams' % step)  # 保存路径
# model.save_pretrained('./checkpoint')
# tokenizer.save_pretrained('./checkpoint')

#################### 模型预测 ##################
preds = predict(model, test_loader, test_ds, label_vocab)  # 预测
file_path = "ernie_results.txt"
with open(file_path, "w", encoding="utf8") as fout:
    fout.write("\n".join(preds))

# Print some examples
print(
    "The results have been saved in the file: %s, some examples are shown below: "
    % file_path)
print("\n".join(preds[:10]))
```

utils.py文件

```python
import numpy as np
import paddle
import paddle.nn.functional as F
from paddlenlp.data import Stack, Tuple, Pad

def load_dict(dict_path):
    vocab = {}
    i = 0
    for line in open(dict_path, 'r', encoding='utf-8'):
        key = line.strip('\n')
        vocab[key] = i
        i+=1
    return vocab

def convert_example(example, tokenizer, label_vocab):
    tokens, labels = example
    tokenized_input = tokenizer(
        tokens, return_length=True, is_split_into_words=True)

    # Token '[CLS]' and '[SEP]' will get label 'O'
    # 处理[CLS]和[SEP]标记
    labels = ['O'] + labels + ['O']
    tokenized_input['labels'] = [label_vocab[x] for x in labels]
    return tokenized_input['input_ids'], tokenized_input['token_type_ids'], tokenized_input['seq_len'], tokenized_input['labels']

# 装饰器：禁用动态图
@paddle.no_grad()
def evaluate(model, metric, data_loader):
    model.eval()
    metric.reset()

    for input_ids, seg_ids, lens, labels in data_loader:
        logits = model(input_ids, seg_ids)
        preds = paddle.argmax(logits, axis=-1)
        n_infer, n_label, n_correct = metric.compute(None, lens, preds, labels)
        metric.update(n_infer.numpy(), n_label.numpy(), n_correct.numpy())
        precision, recall, f1_score = metric.accumulate()
    print("eval precision: %f - recall: %f - f1: %f" %
          (precision, recall, f1_score))
    model.train()

def predict(model, data_loader, ds, label_vocab):
    pred_list = []
    len_list = []
    for input_ids, seg_ids, lens, labels in data_loader:
        logits = model(input_ids, seg_ids)
        pred = paddle.argmax(logits, axis=-1)
        pred_list.append(pred.numpy())
        len_list.append(lens.numpy())
    preds = parse_decodes(ds, pred_list, len_list, label_vocab)
    return preds

def parse_decodes(ds, decodes, lens, label_vocab):
    decodes = [x for batch in decodes for x in batch]
    lens = [x for batch in lens for x in batch]
    id_label = dict(zip(label_vocab.values(), label_vocab.keys()))

    outputs = []
    for idx, end in enumerate(lens):
        sent = ds.data[idx][0][:end]
        tags = [id_label[x] for x in decodes[idx][1:end]]
        sent_out = []
        tags_out = []
        words = ""
        for s, t in zip(sent, tags):
            if t.endswith('-B') or t == 'O':
                if len(words):
                    sent_out.append(words)
                tags_out.append(t.split('-')[0])
                words = s
            else:
                words += s
        if len(sent_out) < len(tags_out):
            sent_out.append(words)
        outputs.append(''.join(
            [str((s, t)) for s, t in zip(sent_out, tags_out)]))
    return outputs

# 预测
def predict(model, data_loader, ds, label_vocab):
    pred_list = []
    len_list = []
    for input_ids, seg_ids, lens, labels in data_loader:
        logits = model(input_ids, seg_ids)
        pred = paddle.argmax(logits, axis=-1)
        pred_list.append(pred.numpy())
        len_list.append(lens.numpy())
    preds = parse_decodes(ds, pred_list, len_list, label_vocab)
    return preds
```



### 2）文本语义相似度计算

连接地址：https://aistudio.baidu.com/aistudio/projectdetail/3294591?contributionType=1

【数据】LCQMC语义匹配数据，格式如下：

```
{'query': '喜欢打篮球的男生喜欢什么样的女生', 'title': '爱打篮球的男生喜欢什么样的女生', 'label': 1}
{'query': '我手机丢了，我想换个手机', 'title': '我想买个新手机，求推荐', 'label': 1}
{'query': '大家觉得她好看吗', 'title': '大家觉得跑男好看吗？', 'label': 0}
{'query': '求秋色之空漫画全集', 'title': '求秋色之空全集漫画', 'label': 1}
{'query': '晚上睡觉带着耳机听音乐有什么害处吗？', 'title': '孕妇可以戴耳机听音乐吗?', 'label': 0}
```



代码（增加注释，未做改动）：

```python
# 语义匹配

"""
# 正式开始实验之前首先通过如下命令安装最新版本的 paddlenlp
!python -m pip install --upgrade paddlenlp==2.0.2 -i https://pypi.org/simple

# paddlenlp 会自动下载 lcqmc 数据集解压到 "${HOME}/.paddlenlp/datasets/LCQMC/lcqmc/lcqmc/" 目录下
!ls ${HOME}/.paddlenlp/datasets/LCQMC/lcqmc/lcqmc
"""

import time
import os
import numpy as np

import paddle
import paddle.nn.functional as F
from paddlenlp.datasets import load_dataset
import paddlenlp
from paddlenlp.data import Stack, Pad, Tuple
from functools import partial
import paddle.nn as nn
from paddlenlp.transformers import LinearDecayWithWarmup

# 一键加载 Lcqmc 的训练集、验证集
# lcqmc:A Large-scale Chinese Question Matching Corpus 语义匹配数据集
train_ds, dev_ds = load_dataset("lcqmc", splits=["train", "dev"])

# 输出训练集的前 20 条样本
for idx, example in enumerate(train_ds):
    if idx <= 20:
        print(example)

##################### 数据预处理 ##################
# 因为是基于预训练模型 ERNIE-Gram 来进行，所以需要首先加载 ERNIE-Gram 的 tokenizer，
# 后续样本转换函数基于 tokenizer 对文本进行切分

tokenizer = paddlenlp.transformers.ErnieGramTokenizer.from_pretrained('ernie-gram-zh')


# 将 1 条明文数据的 query、title 拼接起来，根据预训练模型的 tokenizer 将明文转换为 ID 数据
# 返回 input_ids( 表示输入文本的token ID) 和 token_type_ids(属于输入的第一个句子还是第二个句子)

def convert_example(example, tokenizer, max_seq_length=512, is_test=False):
    query, title = example["query"], example["title"]

    # 样本格式转换，返回字的索引(索引为1表示开头, 索引为2表示结束)、类型(0:问题, 1:答案)
    encoded_inputs = tokenizer(text=query, 
                               text_pair=title, 
                               max_seq_len=max_seq_length)

    input_ids = encoded_inputs["input_ids"]
    token_type_ids = encoded_inputs["token_type_ids"]

    if not is_test:  # 训练阶段，需要返回label
        label = np.array([example["label"]], dtype="int64")
        return input_ids, token_type_ids, label
    else:  # 在预测或者评估阶段，不返回 label 字段
        return input_ids, token_type_ids  # 返回索引、类型


### 对训练集的前5条数据进行转换
# for i in range(5):
#     input_ids, token_type_ids, label = convert_example(train_ds[i], tokenizer)
#     print(input_ids)
#     print(token_type_ids)
#     print(label)
#     print("--------------------")


# 为了后续方便使用，我们给 convert_example 赋予一些默认参数
# partial函数：为convert_example函数设置固定参数
# 训练集和验证集的样本转换函数
trans_func = partial(convert_example,
                     tokenizer=tokenizer,
                     max_seq_length=512)

# 我们的训练数据会返回 input_ids, token_type_ids, labels 3 个字段
# 因此针对这 3 个字段需要分别定义 3 个组 batch 操作
# 将前两个字段填充(Pad)，后一个堆叠(Stack)
batchify_fn = lambda samples, fn=Tuple(Pad(axis=0, pad_val=tokenizer.pad_token_id),  # input_ids
                                       Pad(axis=0, pad_val=tokenizer.pad_token_type_id),  # token_type_ids
                                       Stack(dtype="int64")  # label
                                       ): [data for data in fn(samples)]

# 定义分布式 Sampler(批处理): 自动对训练数据进行切分，支持多卡并行训练
batch_sampler = paddle.io.DistributedBatchSampler(train_ds, batch_size=32, shuffle=True)

# 基于 train_ds 定义 train_data_loader
# 因为我们使用了分布式的 DistributedBatchSampler, train_data_loader 会自动对训练数据进行切分
# DataLoader：返回一个迭代器，每次从数据集返回一个批次数据
train_data_loader = paddle.io.DataLoader(dataset=train_ds.map(trans_func),  # 数据集
                                         batch_sampler=batch_sampler,  # 样本批处理器
                                         collate_fn=batchify_fn,  # 分成小批次,该参数为0，则在第0维上堆叠
                                         return_list=True)  # 是否返回列表

# 针对验证集数据加载，我们使用单卡进行评估，所以采用 paddle.io.BatchSampler 即可
# 定义 dev_data_loader(验证集数据批处理器)
batch_sampler = paddle.io.BatchSampler(dev_ds, batch_size=32, shuffle=False)
dev_data_loader = paddle.io.DataLoader(dataset=dev_ds.map(trans_func),  # 数据集
                                       batch_sampler=batch_sampler,  # 样本批处理器
                                       collate_fn=batchify_fn,
                                       return_list=True)

# 我们基于 ERNIE-Gram 模型结构搭建 Point-wise 语义匹配网络
# 所以此处先定义 ERNIE-Gram 的 pretrained_model
pretrained_model = paddlenlp.transformers.ErnieGramModel.from_pretrained('ernie-gram-zh')


# pretrained_model = paddlenlp.transformers.ErnieModel.from_pretrained('ernie-1.0')
class PointwiseMatching(nn.Layer):

    # 此处的 pretained_model 在本例中会被 ERNIE-Gram 预训练模型初始化
    def __init__(self, pretrained_model, dropout=None):
        super().__init__()
        self.ptm = pretrained_model
        self.dropout = nn.Dropout(dropout if dropout is not None else 0.1)

        # 语义匹配任务: 相似、不相似 2 分类任务
        self.classifier = nn.Linear(self.ptm.config["hidden_size"], 2)

    def forward(self,
                input_ids,
                token_type_ids=None,
                position_ids=None,
                attention_mask=None):
        # 此处的 Input_ids 由两条文本的 token ids 拼接而成
        # token_type_ids 表示两段文本的类型编码
        # 返回的 cls_embedding 就表示这两段文本经过模型的计算之后而得到的语义表示向量
        _, cls_embedding = self.ptm(input_ids, token_type_ids, position_ids, attention_mask)
        cls_embedding = self.dropout(cls_embedding)

        # 基于文本对的语义表示向量进行 2 分类任务
        logits = self.classifier(cls_embedding)
        probs = F.softmax(logits)

        return probs


# 定义 Point-wise 语义匹配网络
model = PointwiseMatching(pretrained_model)

epochs = 3
num_training_steps = len(train_data_loader) * epochs

# 定义 learning_rate_scheduler，负责在训练过程中对 lr 进行调度
lr_scheduler = LinearDecayWithWarmup(5E-5, num_training_steps, 0.0)

# Generate parameter names needed to perform weight decay.
# All bias and LayerNorm parameters are excluded.
# 生成权重衰减参数(不包括偏置，LayerNorm的参数)
decay_params = [
    p.name for n, p in model.named_parameters()
    if not any(nd in n for nd in ["bias", "norm"])
]

# 定义 Optimizer AdamW: 可以解决Adam优化器中L2正则化失败的问题
optimizer = paddle.optimizer.AdamW(learning_rate=lr_scheduler,  # 学习率
                                   parameters=model.parameters(),  # 模型参数
                                   weight_decay=0.0,  # 权重衰减系数
                                   apply_decay_param_fun=lambda x: x in decay_params)  # 衰减策略应用于哪些参数

# 采用交叉熵 损失函数
criterion = paddle.nn.loss.CrossEntropyLoss()

# 评估的时候采用准确率指标
metric = paddle.metric.Accuracy()


# 因为训练过程中同时要在验证集进行模型评估，因此我们先定义评估函数

@paddle.no_grad()
def evaluate(model, criterion, metric, data_loader, phase="dev"):
    model.eval()  # 评估
    metric.reset()  # 清空
    losses = []
    for batch in data_loader:
        input_ids, token_type_ids, labels = batch
        probs = model(input_ids=input_ids, token_type_ids=token_type_ids)
        loss = criterion(probs, labels)
        losses.append(loss.numpy())
        correct = metric.compute(probs, labels)
        metric.update(correct)
        accu = metric.accumulate()
    print("eval {} loss: {:.5}, accu: {:.5}".format(phase,
                                                    np.mean(losses), accu))
    model.train()
    metric.reset()


# 接下来，开始正式训练模型

global_step = 0
tic_train = time.time()

for epoch in range(1, epochs + 1):
    for step, batch in enumerate(train_data_loader, start=1):  # start为下标起始位置

        input_ids, token_type_ids, labels = batch
        probs = model(input_ids=input_ids, token_type_ids=token_type_ids)  # 预测
        loss = criterion(probs, labels)  # 计算loss
        correct = metric.compute(probs, labels)  # 计算正确率
        metric.update(correct)  # 更新指标数据，以便计算累计数据
        acc = metric.accumulate()  # 返回累计指标

        global_step += 1

        # 每间隔 10 step 输出训练指标
        if global_step % 10 == 0:
            print(
                "global step %d, epoch: %d, batch: %d, loss: %.5f, accu: %.5f, speed: %.2f step/s"
                % (global_step, epoch, step, loss, acc,
                   10 / (time.time() - tic_train)))  # 时间
            tic_train = time.time()
        loss.backward()  # 反向传播计算
        optimizer.step()  # 优化器更新参数
        lr_scheduler.step()  # 更新学习率，下一个优化周期生效
        optimizer.clear_grad()  # 清除梯度

        # 每间隔 100 step 在验证集和测试集上进行评估
        if global_step % 100 == 0:
            evaluate(model, criterion, metric, dev_data_loader, "dev")

# 训练结束后，存储模型参数
save_dir = os.path.join("checkpoint", "model_%d" % global_step)
os.makedirs(save_dir)

save_param_path = os.path.join(save_dir, 'model_state.pdparams')
paddle.save(model.state_dict(),  # 将所有待持久化参数写入字典中
            save_param_path)  # 保存路径
tokenizer.save_pretrained(save_dir)


####################### 使用预训练好的模型预测 ############################
"""
# 下载我们基于 Lcqmc 事先训练好的语义匹配模型并解压
! wget https://paddlenlp.bj.bcebos.com/models/text_matching/ernie_gram_zh_pointwise_matching_model.tar
! tar -xvf ernie_gram_zh_pointwise_matching_model.tar

# 测试数据由 2 列文本构成 tab 分隔
# Lcqmc 默认下载到如下路径
! head -n10 "${HOME}/.paddlenlp/datasets/LCQMC/lcqmc/lcqmc/test.tsv"
"""
def predict(model, data_loader):
    batch_probs = []

    # 预测阶段打开 eval 模式，模型中的 dropout 等操作会关掉
    model.eval()

    with paddle.no_grad():
        for batch_data in data_loader:
            input_ids, token_type_ids = batch_data
            input_ids = paddle.to_tensor(input_ids)
            token_type_ids = paddle.to_tensor(token_type_ids)

            # 获取每个样本的预测概率: [batch_size, 2] 的矩阵
            batch_prob = model(
                input_ids=input_ids, token_type_ids=token_type_ids).numpy()

            batch_probs.append(batch_prob)
        batch_probs = np.concatenate(batch_probs, axis=0)

        return batch_probs


# 预测数据的转换函数
# predict 数据没有 label, 因此 convert_exmaple 的 is_test 参数设为 True
trans_func = partial(convert_example,
                     tokenizer=tokenizer,
                     max_seq_length=512,
                     is_test=True)

# 预测数据的组 batch 操作
# predict 数据只返回 input_ids 和 token_type_ids，因此只需要 2 个 Pad 对象作为 batchify_fn
batchify_fn = lambda samples, fn=Tuple(
    Pad(axis=0, pad_val=tokenizer.pad_token_id),  # input_ids
    Pad(axis=0, pad_val=tokenizer.pad_token_type_id),  # segment_ids
): [data for data in fn(samples)]

# 加载预测数据
test_ds = load_dataset("lcqmc", splits=["test"])

batch_sampler = paddle.io.BatchSampler(test_ds, batch_size=32, shuffle=False)

# 生成预测数据 data_loader
predict_data_loader = paddle.io.DataLoader(
    dataset=test_ds.map(trans_func),
    batch_sampler=batch_sampler,
    collate_fn=batchify_fn,
    return_list=True)

pretrained_model = paddlenlp.transformers.ErnieGramModel.from_pretrained('ernie-gram-zh')

model = PointwiseMatching(pretrained_model)

# 刚才下载的模型解压之后存储路径为 ./ernie_gram_zh_pointwise_matching_model/model_state.pdparams
state_dict = paddle.load("./ernie_gram_zh_pointwise_matching_model/model_state.pdparams")

# 刚才下载的模型解压之后存储路径为 ./pointwise_matching_model/ernie1.0_base_pointwise_matching.pdparams
# state_dict = paddle.load("pointwise_matching_model/ernie1.0_base_pointwise_matching.pdparams")
model.set_dict(state_dict)

for idx, batch in enumerate(predict_data_loader):
    if idx < 1:
        print(batch)
# 执行预测函数
y_probs = predict(model, predict_data_loader)

# 根据预测概率获取预测 label
y_preds = np.argmax(y_probs, axis=1)

# 我们按照千言文本相似度竞赛的提交格式将预测结果存储在 lcqmc.tsv 中，用来后续提交
# 同时将预测结果输出到终端，便于大家直观感受模型预测效果

test_ds = load_dataset("lcqmc", splits=["test"])

with open("lcqmc.tsv", 'w', encoding="utf-8") as f:
    f.write("index\tprediction\n")
    for idx, y_pred in enumerate(y_preds):
        f.write("{}\t{}\n".format(idx, y_pred))
        text_pair = test_ds[idx]
        text_pair["label"] = y_pred
        print(text_pair)
```

