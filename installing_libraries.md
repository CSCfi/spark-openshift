# Installing custom Spark Libraries

Please note there are two kinds of libraries:
1. That are designed to run on top of Spark natively and should run normally
2. That are not designed to run on top of Spark natively and one would have to use some sort of workaround to make it run (if possible).

For the libraries that are designed to run on spark -

There are again two ways of installing them:

1. Create customized docker images which will install the libraries in all the workers and the driver. **(Recommended)**
Provide this image as the value of the variables for the driver, worker, master(not needed normally), when launching a cluster via the Openshift UI or using the template through command line.

2. A quick way to install custom libraries is by creating a python virtualenv in */mnt/\<cluster-name\>* directory 
**Note: You cannot use Jupyter notebook anymore to type your spark code, if you use this method. Instead, you would have to use a Jupyter Terminal !**
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
