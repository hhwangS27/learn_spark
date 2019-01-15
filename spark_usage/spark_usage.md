# basic frame

Spark2.0 use spark session instead of spark context as the main entry point.
Aslo, you can find spark context using `spark.sparkcontext`.

In spark session, we mainly use DataFrame instead of RDD.

```

from pyspark.sql import sparksession, functions, types

spark = sparksession.builder.appname('weather etl').getorcreate()

df = spark.read.csv(inputs, schema = givenschema(), sep = ' ').withcolumn('filename', functions.input_file_name())

```

> In pyspark, we do not need to create a spark session. It already has a spark
> session called `spark`.

# [pyspark.sql.DataFrame](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=explode#pyspark.sql.DataFrame)

+ `drop(\*cols)`

+ `cache()`

+ `coalesce(numPartitions)`
  > Returns a new DataFrame that has exactly numPartitions partitions.

+ `agg(\*exprs)`: multiple lines becomes one line
  ```
  df.agg(  F.min( df['age'] )  )
  ```

+ explode(\*exprs): one line becomes multiple lines
