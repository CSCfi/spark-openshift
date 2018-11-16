FROM docker-registry.rahti.csc.fi/spark-images/spark:latest

# Do not change this
USER notebook

# Put your libraries here
RUN pip install spark_sklearn \
    && pip install h5py

# Do not change this
USER root
