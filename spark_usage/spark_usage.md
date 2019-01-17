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

#### Three ways to use `pandas_udf`

##### use decorator
```python
from pyspark.sql import functions as F
from pyspark.sql import types as T


@F.pandas_udf(returnType=T.ArrayType(T.StringType()))
def tokenizer(a):
    return a.apply(lambda x: x.split())


amz = pd.Series(['apple 16G made in china', 'lenovo 256g made in china', 'chrome 0G made in america'])
df_amz = spark.createDataFrame(pd.DataFrame(amz, columns=["title"]))
df_amz.show()
df_amz.select( tokenizer(df_amz['title']) ).show()

#+--------------------+
#|               title|
#+--------------------+
#|apple 16G made in...|
#|lenovo 256g made ...|
#|chrome 0G made in...|
#+--------------------+
#
#+--------------------+
#|    tokenizer(title)|
#+--------------------+
#|[apple, 16G, made...|
#|[lenovo, 256g, ma...|
#|[chrome, 0G, made...|
#+--------------------+
```
##### use udf function 
```python
from pyspark.sql import functions as F
from pyspark.sql import types as T

def tokenizer_func(a):
    return a.apply(lambda x: x.split())
tokenizer = F.pandas_udf(tokenizer_func, returnType=T.ArrayType(T.StringType()) )


amz = pd.Series(['apple 16G made in china', 'lenovo 256g made in china', 'chrome 0G made in america'])
df_amz = spark.createDataFrame(pd.DataFrame(amz, columns=["title"]))
df_amz.show()
df_amz.select( tokenizer(df_amz['title']) ).show()


#+--------------------+
#|               title|
#+--------------------+
#|apple 16G made in...|
#|lenovo 256g made ...|
#|chrome 0G made in...|
#+--------------------+
#
#+---------------------+
#|tokenizer_func(title)|
#+---------------------+
#| [apple, 16G, made...|
#| [lenovo, 256g, ma...|
#| [chrome, 0G, made...|
#+---------------------+
```
##### use `pyspark`.udf.register


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

Be careful, some of the type need specify the element type.
> For example: for a string list you should write like this: `T.ArrayType(T.String())`

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



