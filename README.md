# movie-recommendation

## Data
该数据集有1000+个用户，1700+部电影，十万的评分数据。
[source](https://grouplens.org/datasets/movielens/)

## 项目描述

首先获取对应的电影，评分，用户数据，然后自定义一个按个人喜好的打分，根据该用户的观影习惯和个人信息完成对该自定义用户的电影推荐。本次实现使用基于协同过滤的推荐。
协同过滤包括基于用户（用户之间的相似性）和基于物品（某个用户对不同物品的喜好）和基于模型（通过样本的用户喜好信息，训练推荐模型，从而达到根据实时的用户喜好信息进行预测）。

## 环境

基于pyspark编程实现。

依赖库： 
```python
from pyspark.mllib.recommendation import Rating
from pyspark.mllib.recommendation import ALS
```

## 原理
在Spark MLlib中，推荐算法实现了基于矩阵分解的协同过滤算法，该算法基于FunkSVD,将评分矩阵分解为两个低维的矩阵。

#### Recommendation:
-Rating类：封装用户，物品，评分三元组
-ALS（交替最小二乘法）类：用来训练FunkSVD模型
```python
#参数
#ratings - 评分矩阵对应的RDD
#rank-矩阵分解的维度
#iteration-使用ALS求解的最大迭代次数
#lambda-FunkSVD分解时的正则化系数
```
-MatrixFactorizationModel:用ALS训练出来的模型，可以用来帮助做预测



## 流程

#### 数据读取
```python
user_data = sc.textFile("u.data")
``` 
 #### 预处理
 将数据按分割符分开并转化成Rating类对应的数组
```python
rates = user_data.map(lambda x: x.split("\t")[0:3])
rates_data = rates.map(lambda x: Rating(int(x[0]),int(x[1]),int(x[2])))
```
#### 训练模型
```python
sc.setCheckpointDir('checkpoint/')
ALS.checkpointInterval = 2
model = ALS.train(ratings=rates_data, rank=20, iterations=5, lambda_=0.02)
```
#### 推荐
```python
model.predict(i,j)
#得到用户i对物品j的评分
model.recommendProducts(i,j)
#用户i最喜欢的j个物品
model.recommendUsers(i,j)
#物品i最值得推荐给的j个用户
```


    
