# Installing custom Spark Libraries

Please note there are two kinds of libraries:
1. That are designed to run on top of Spark natively and should run normally
2. That are not designed to run on top of Spark natively and one would have to use some sort of workaround to make it run (if possible).

There are two ways of installing libraries for your Spark Cluster:

1. Create customized docker images which will install the libraries in all the workers and the driver by using this [template](https://github.com/CSCfi/spark-openshift/blob/master/docker-images/custom/Dockerfile)  **(Recommended)**
Provide this image as the value of the variables for the driver, worker, master(not needed normally), when launching a cluster via the Openshift UI or using the template through command line.

The [template](https://github.com/CSCfi/spark-openshift/blob/master/docker-images/custom/Dockerfile) has examples for both Python and Java based libraries.

  a ) Python: Use the Python libraries via pip (if they are comptable with Spark), as shown in the template.
  You should be able to import the libraries in Jupyter notebook. For proper usage, you should refer to the documentation of the libraries itself.
  
  b ) Java: To install custom Java jars, you need to add the jars, as shown in the template.
  Then you need to add the jars in your SparkConf, and you may also need to specify some extra configuration depending upon the library (again, for this part, you should refer to the documentation of the libraries itself)
  
  One of the example could be like this :
  ```
  conf.set("spark.jars", "file:/opt/jars/aws-java-sdk.jar,file:/opt/jars/hadoop-aws.jar")
  conf.set("spark.hadoop.fs.s3a.impl", "org.apache.hadoop.fs.s3a.S3AFileSystem")
```
  

2. A quick way to install custom libraries is by creating a python virtualenv in */mnt/\<cluster-name\>* directory 
**Note: You cannot use Jupyter notebook anymore to type your spark code, if you use this method. Instead, you would have to use a Jupyter Terminal !**

As an example, let's install *spark_sklearn* library, which is designed to run on Spark natively. To understand how it works, read the [documentation here](https://github.com/databricks/spark-sklearn)
```
$ cd /mnt/\<cluster-name\>-pvc/
$ virtualenv scikit
$ source scikit/bin/activate
$ which pip # check if it is using the correct pip location
$(scikit) pip install spark_sklearn scipy
$(scikit) which python # check if the correct python path is being used
$(scikit) python
>>>from pyspark import SparkConf, SparkContext
>>>conf = SparkConf()
>>>conf.set("spark.pyspark.driver.python","/mnt/\<cluster-name\>-pvc/scikit/bin/python")
>>>conf.set("spark.pyspark.python","/mnt/\<cluster-name>\-pvc/scikit/bin/python")
>>>sc = SparkContext(conf=conf)
>>>import sklearn, spark_sklearn
```
