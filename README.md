# PySpark Operations On Amazon Tweets  

### TABLE OF CONTENTS
* [Objective](#objective)
* [Technologies](#technologies)
* [Algorithms](#algorithms)
* [Data](#data)
* [Implementation](#implementation)
* [Results](#results)

## OBJECTIVE 
* Repository to showcase the implementation of Spark context, Spark SQL context on Amazon Tweets data set with 400k Tweets. 
* Dealt with tweet_id (id_str), Tweet_created_time, Retweet_count, Favourite_count to find the days with high influx of tweets.  
* Analyzed the tweets on the busiest day to find the words that were repeated the most in the selected tweets.

## TECHNOLOGIES
Project is created with: 
* Python - **pandas, seaborn, sklearn**
* Spark

## ALGORITHMS
Map Reduce in Spark

## DATA
The used dataset is a collection of tweets where Amazon was tagged by the users on Twitter to review its service. For each and every tweet/record in dataset, the sentiment of the tweet is also recorded and that is the binary target variable for the dataset. As the dataset consists of 400k records, we cannot build and run machine learning models directly and we would need to have a Big Data service to distribute and run the program. So, PySpark was used as it is easy to integrate it with Python for analysis.

## IMPLEMENTATION
Summary of operations:  

**1. Import csv file:**  
data = spark.read.format("csv").option("header","true").load("filepath/filename.csv")  

**2. Selecting the required columns from a data frame:**  
data.select("column_name1",'colname2','colname3','colname4')  

**3. Printing data types of columns:**  
types = [f.dataType for f in data1.schema.fields]  
types  

**4. Printing distinct values of a column:**  
data.select("column_name").distinct().show()  

**5. Parsing a column to create additional columns:**  
from pyspark.sql.functions import split  
tweet_created_at is in the format: "Tue Nov 01 02:39:55 +0000 2016"  
a = split(dat_filtered["tweet_created_at"], ' ')  
dat_filtered = dat_filtered.withColumn('Month', a.getItem(1))  
dat_filtered = dat_filtered1.withColumn('Date', a.getItem(2))  
dat_filtered = dat_filtered1.withColumn('Year', a.getItem(5))  

**6. Concatenating multiple columns to form a new one**  
dat_filtered.select(concat(col("Month"), lit(" "), col("Date"),lit(" "), col("Year")).alias("Date"))  

**7. Importing SQL funtions col,lit**  
import pyspark.sql.functions as sq 
dat_filtered.withColumn("tweet_created_at",sq.concat(col("Month"), sq.lit(" "), sq.col("Date"),sq.lit(" "), sq.col("Year")))

**8. Aggregations on dataframe**  
df.groupby(df.date).agg(sq.count('id_str').alias("count_of_tweets"))  
Above command counts the number of tweets grouped by the date  

**9. Initializing spark context and sql context to perform SQL queries**  
conf = pyspark.SparkConf()  
sc = pyspark.SparkContext.getOrCreate(conf=conf)  
from pyspark.sql import SQLContext  
sqlcontext = SQLContext(sc)  
counts.registerTempTable("tmpcounts")  
counts_ordered = sqlcontext.sql("SELECT * FROM tmpcounts order by count_of_tweets desc limit 5")  

## RESULTS
The Built model has processed 400k records in under 3 minutes and the same data took 15 minutes to load in Python without spark for data processing.

