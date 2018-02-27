# movie-recommendation

## Data
该数据集有1000+个用户，1700+部电影，十万的评分数据。
[source](https://grouplens.org/datasets/movielens/)

## 项目描述

首先获取对应的电影，评分，用户数据，然后自定义一个按个人喜好的打分，根据该用户的观影习惯和个人信息完成对该自定义用户的电影推荐。本次实现使用基于协同过滤的推荐。
协同过滤包括基于用户（用户之间的相似性）和基于物品（某个用户对不同物品的喜好）和基于模型（通过样本的用户喜好信息，训练推荐模型，从而达到根据实时的用户喜好信息进行预测）。

## 环境

基于linux下spark-shell编程实现。

依赖库： 
```scala
import org.apache.log4j.{Level, Logger}
import org.apache.spark.mllib.recommendation.{ALS, MatrixFactorizationModel, Rating}
import org.apache.spark.rdd._
import org.apache.spark.{SparkContext, SparkConf}
import scala.io.Source
```

## 流程

#### 数据读取
-定义读取函数（数据切割）
```scala
def addRatings(path:String):Seq[Rating] = {
    val lines = Source.fromFile(path).getLines()
    val ratings = lines.map{
      line =>
        val fields = line.split("::")
        Rating(fields(0).toInt,fields(1).toInt,fields(2).toDouble)
    }.filter(_.rating > 0.0)
    if(ratings.isEmpty){
      sys.error("No ratings provided.")
    }else{
      ratings.toSeq
    }
  }
    val myRatings = addRatings("/home/shiyanlou/ml-1m/person.txt")
    val myRatingsRDD = sc.parallelize(myRatings, 1)
 ```
 - 加载样本评分数据
 ``` scala
 val ratings = sc.textFile("/home/shiyanlou/ml-1m/ratings.dat").map {
      line =>
        val fields = line.split("::")

        (fields(3).toLong % 10, Rating(fields(0).toInt, fields(1).toInt, fields(2).toDouble))
    }
 ```
 - 加载电影数据
```scala
 val movies = sc.textFile("/home/shiyanlou/ml-1m/movies.dat").map {
      line =>
        val fields = line.split("::")
        (fields(0).toInt, fields(1)) 
    }.collect().toMap
 ```
 
 #### 预处理
 
    
