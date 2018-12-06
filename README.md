[TOC]
# chatBot
知识型聊天机器人 自建知识库搜索api


# 微信机器人功能


## 1. 知识库-问答对：
    1. 错误代码KT003
    2. 点播视频播放不完整


```
错误代码：
KT001
KT003
KT005
KT006
10000
10001
10010
1305
40002
1302
KT007
KT011
KT016
```

```
账户已停机 提示：您的账户已停机
UserToken失效 点播时提示：UserToken失效
未知的异常 开机时提示：未知的异常
LOS亮红灯 LOS红灯亮
部分直播黑屏
全部直播黑屏
黑白屏
点播视频播放不完整或回跳 点播视频播放不完整 点播视频播放回跳
```
## 2. 知识库-数据库查询:
    1. 石家庄网络覆盖类LTE数据问题

```
fault:
网络覆盖类 20000 fault
其他 20000 fault
系统容量类 20000 fault
bussiness:
GSM话音和数据 20000 bus
LTE话音和数据 20000 bus
TD话音和数据 20000 bus
LTE数据 20000 bus
```

## 3. 舆情：
    1. '天气预警','灾害预警'
    2. '微博热搜排行','微博热搜排行榜','新浪热搜排行'
    3. '网易新闻排行','网易新闻'
    4. 更新排行榜
    5. （磁盘预测？）

## 4. 定时任务
    AT 8:57 定时任务测试

## 5. 键值存取
    @Auto机器人 PUT "CMCC:中国移动通信集团有限公司"
    GET XXXXX
## 6. 闲聊


# 微信机器人知识库部分
```
graph TD
A[原始文本]-->B[分词]
B --> C[判断意图]
B --> R{or}
R --> D[抽取实体]
R --> G[句子相似度匹配]
C --> E[确定知识库/领域]
D --> F[确定查询条件]
G --> F
```

## 1. 分词
利用jieba分词算法，总结建立用户词典提高关键词（影响查询）的分词准确率

## 2. 确定知识库
### 判断意图
通过判断句子意图，可以确定所要查询的知识库。
利用关键词、正则以及神经网络等方法实现意图判断算法。  
* 利用神经网络判断的方法需要在前期积累各个意图领域的语料

## 3. 确定查询条件
根据知识库的构建方式，若为问答对形式，可以使用句子相似度的方法，计算输入语句与知识库中问答对之间相似度，匹配最相近的答案。若知识库为知识点的形式，可以抽取输入语句中相关实体，作为限定条件，搜索符合答案的结果。
### 句子相似度匹配
可利用算法例如 LSI LDA simhush等进行句子相似度的匹配
### 实体抽取
从输入语句中识别出查询条件范围内的实体，中间需要进行近义词的转换，否定词的识别等工作。

# 实现功能

### 1. 搜索列知识库
#### 家客知识库
```
jiake_ecodes = ['KT001', 'KT003', 'KT005', 'KT006', '10000', '10001', '10010',
                '1305', '40002', '1302', 'KT007', 'KT011', 'KT016',
                '账户已停机', 'UserToken失效', '未知的异常',
                'LOS亮红灯', '部分直播黑屏', '全部直播黑屏', '黑白屏', '点播视频播放不完整或回跳']
```
### 2. 调用api返回网名

### 3. 利用 GET PUT 关键词进行redis key value存取

### 4. 利用 AT 关键词进行 AT 时:分 sentence 形式定时任务发布

通过监听


```
__keyevent@0__:expired
```

发布

### 5. 舆情接口
#### 网易新闻和新浪热搜排行榜爬虫

```
触犯更新排行关键词： ['更新排行榜']
触发显示网易新闻排行关键词： ['网易新闻排行']
触发显示微博热搜关键词： ['微博热搜排行']

词库可以扩展，没有分词，前后可以有空格，采用句子完全匹配的方式进行识别
```
#### 全省天气预警
```
关键词 ['天气预警']
```
返回发生预警的城市

