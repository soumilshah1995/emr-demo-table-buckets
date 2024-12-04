# emr-demo-table-buckets
![Screenshot 2024-12-04 at 6 44 47 PM](https://github.com/user-attachments/assets/b3f3c1aa-5eb6-4fb3-a488-26720d657000)

# Commands
```
chmod 400  <PATH>.<PEMFILE>.pem
ssh -i <PATH>.<PEMFILE>m hadoop@<IP>

pyspark \
 --packages 'software.amazon.s3tables:s3-tables-catalog-for-iceberg:0.1.0,org.apache.iceberg:iceberg-spark-runtime-3.4_2.12:1.6.0'

from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("iceberg_lab") \
    .config("spark.sql.catalog.s3tablesbucket", "org.apache.iceberg.spark.SparkCatalog") \
    .config("spark.sql.catalog.s3tablesbucket.client.region", "us-east-1") \
    .config("spark.sql.catalog.defaultCatalog", "s3tablesbucket") \
    .config("spark.sql.catalog.s3tablesbucket.warehouse", "XXXXX") \
    .config("spark.sql.catalog.s3tablesbucket.catalog-impl", "software.amazon.s3tables.iceberg.S3TablesCatalog") \
    .getOrCreate()

spark.sql("CREATE NAMESPACE IF NOT EXISTS s3tablesbucket.example_namespace")
spark.sql("SHOW NAMESPACES IN s3tablesbucket").show()
spark.sql("SHOW TABLES in s3tablesbucket.example_namespace").show()

spark.sql("""CREATE TABLE IF NOT EXISTS s3tablesbucket.example_namespace.table1
 (id INT,
  name STRING,
  value INT)
  USING iceberg
  """)

spark.sql("""
    INSERT INTO  s3tablesbucket.example_namespace.table1
    VALUES
        (1, 'ABC', 100),
        (2, 'XYZ', 200)
""")
spark.sql("SELECT * FROM  s3tablesbucket.example_namespace.table1 ").show()
```
