FROM docker-registry.rahti.csc.fi/spark-images/spark:latest

# Do not change this
USER notebook

# Put your libraries here and other code (if required)
RUN pip install spark_sklearn \
    && pip install scipy

# Do not change this
USER root
