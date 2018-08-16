# spark-openshift
Run Apache Spark on Openshift

## Quickstart:

The template provisions a Spark cluster based on the configuration on Openshift. Make sure, you run this in a new openshift project.

There are 3 deployments created - 
1. **Spark Master**: Serves as the master of the cluster and runs Spark UI
2. **Spark Worker**: Runs worker instances in the cluster
3. **Jupyter Notebook**: Serves as the Spark Driver, where one writes the code and submits it to the master


### Requirements: 
- oc client for openshift
- apache-utils or httpd-tools package (yum install httpd-tools)

### Variables:

Listed below are some of the variables that should be changed.

- **Cluster Name**: Unique identifier for your cluster
- **Master CPU (Request)**: Number of cores for the master node of the cluster
- **Master Memory (Request)**: Memory for the master node of the cluster
- **Worker Replicas**: Number of workers to have (Default: 4)
- **Worker CPU (Request)**: Number of cores for each worker of the cluster (Default: 2)
- **Worker Memory (Request)**: Memory of each worker of the cluster (Default: 2G)

- **Driver CPU (Request)**: Number of cores for the driver (Jupyter Notebook)
- **Driver Memory (Request)**: Memory of the driver (Jupyter Notebook)
- **htpasswd String**: A string made up of a username and a password for logging in to the Spark UI and Jupyter Notebook (*To get the string: use httpd-tools package and then run the following command: **htpasswd -n any_username***) (Default username: demo, password: demo, and thus the generated string: demo:$apr1$2dqlKdHU$/XC8SjRtPXWwiM6qwR5jF/ )
- **Application Hostname Suffix**: The exposed hostname suffix that will be used to create routes for Spark UI and Jupyter Notebook
