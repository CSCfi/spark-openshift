# It's better to put this file in an empty directory and add files which are necessary for building this image
FROM docker-registry.rahti.csc.fi/spark-images/spark:latest

# Do not change this
USER notebook

# ---- Python ----
# Put your libraries here and other code (if required) with the RUN command
# RUN pip install spark_sklearn \
#    && pip install scipy

# ---- Java ----
# In case if you have Java JAR files, please add them from your local machine like this
# The first parameter here is the jar file which is present on the machine where you will build this image
# Additionally, it's best to keep the jar file in the same folder where this Dockerfile is present

# The second paramter is the location of the jar file in the spark cluster 
# Make sure, you put the location as /opt/jars for the cluster

# ADD custom-lib.jar /opt/jars/custom-lib.jar
# ADD another-lib.jar /opt/jars/another-lib.jar

# Do not change this
USER root
