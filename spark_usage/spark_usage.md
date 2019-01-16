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

# code example
```
cities = spark.read.csv('cities', header=True, inferSchema=True)

cities.printSchema()

```

## Column Expressions

## UDF (user defined function)

`pyspark.sql.functions.udf(f=None, returnType=StringType)`

> Creates a user defined function (UDF).

### [Pandas UDFs (Vectorized UDFs)](https://spark.apache.org/docs/latest/sql-pyspark-pandas-with-arrow.html)

[Here is another introduction.](https://www.jianshu.com/p/87d70918e16e)


## Tpyes

```
from pyspark.sql.types import IntegerType
```
Supported types are: 
* DataType
* NullType
* StringType
* BinaryType
* BooleanType
* DateType
* TimestampType
* DecimalType
* DoubleType
* FloatType
* ByteType
* IntegerType
* LongType
* ShortType
* ArrayType
* MapType
* StructField
* StructType

## Input/Output

```
spark.read.csv('filename')
df.write.json('output', compression='gzip', mode='overwrite')
df.write.csv('output', compression='lz4', mode='append')
```

# [class pyspark.sql.DataFrame](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?#pyspark.sql.DataFrame)

+ `drop(\*cols)`

+ `cache()`

+ `coalesce(numPartitions)`
  > Returns a new DataFrame that has exactly numPartitions partitions.

+ `agg(\*exprs)`: multiple lines becomes one line
  ```
  df.agg(  F.min( df['age'] )  )
  ```

+ explode(\*exprs): one line becomes multiple lines

# [class pyspark.sql.Column](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?#pyspark.sql.Column)

+ `pyspark.sql.functions.concat_ws(sep, *cols)`

   > Concatenates multiple input string columns together into a single string column, using the given separator.



