# movie-recommendation

## Data
该数据集有6000+个用户，3900+部电影，一百多万的评分数据。

主要包括四个数据文件：

#### movies.dat:提供电影数据（电影id,电影名称，电影类型）
#### ratings.dat:提供评分数据（用户id,电影id,评分，时间）
#### users.dat:提供用户数据（用户id,性别，年龄，职业编号，邮编）
#### person.txt: 提供我的数据（用户id0,看过的电影id,对该电影的评分，评分时间）

## 项目描述

首先获取对应的电影，评分，用户数据，然后自定义一个按个人喜好的打分，根据该用户的观影习惯和个人信息完成对该自定义用户的电影推荐。本次实现使用基于协同过滤的推荐，
协同过滤包括基于用户（用户之间的相似性）和基于物品（某个用户对不同物品的喜好）。

##环境

基于linux下spark-shell编程实现。

依赖库： 
```scala
import org.apache.log4j.{Level, Logger}
import org.apache.spark.mllib.recommendation.{ALS, MatrixFactorizationModel, Rating}
import org.apache.spark.rdd._
import org.apache.spark.{SparkContext, SparkConf}
import scala.io.Source
```
