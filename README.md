# spark-openshift
Run Apache Spark on Openshift

## Quickstart:

The template provisions a Spark cluster based on the configuration on Openshift. Make sure, you run this in a new openshift project.

There are 3 deployments created - 
1. **Spark Master**: Serves as the master of the cluster and runs Spark UI
2. **Spark Worker**: Runs worker instances in the cluster
3. **Jupyter Notebook**: Serves as the Spark Driver, where one writes the code and submits it to the master


### Requirements: 
- apache-utils or httpd-tools package (yum install httpd-tools)

### Variables:

Listed below are some of the variables that should be changed.

**Please NOTE : The values for the CPU and Memory should only be changed (to avoid errors) after checking the project quota allocated to your Openshift project.** You should increase or request the admins to increase it for you, if needed.

#### Mandatory Required Values:
- **Cluster Name**: Unique identifier for your cluster
- **Username**: Username for authenticating and logging into your Spark cluster and Jupyter (Recommended: create a new username, don't use any existing one)
- **Password**: Password for authenticating and logging into your Spark cluster and Jupyter (Recommended: create a new password, don't use any existing one)
- **Worker Replicas**: Number of workers to have (Default: 4)

- **Storage Size**: Persistent storage volume size (Default: 1Gi)

#### Optional Required Values:
- **Enable Jupyter Lab**: Specify whether if you want to use Jupyter Lab instead of the default Jupyter Notebook (Default: false) 
- **Master CPU (Request)**: Number of cores for the master node of the cluster
- **Master Memory (Request)**: Memory for the master node of the cluster
- **Worker CPU (Request)**: Number of cores for each worker of the cluster (Default: 2)
- **Worker Memory (Request)**: Memory of each worker of the cluster (Default: 2G)

- **Executor Default Cores**: Default value for Spark Executor Cores (See official Spark documention for more) (Default: 2)
- **Executor Default Memory**: Default value for Spark Executor Memory (**Should always be less than the Worker memory!**) (Default: 3G)

- **Driver CPU (Request)**: Number of cores for the driver (Jupyter Notebook)
- **Driver Memory (Request)**: Memory of the driver (Jupyter Notebook)

#### Do not change the following variables, unless you know what you're doing
- **Master Image**: Docker Image for the Master
- **Worker Image**: Docker Image for the Worker 
- **Driver Image**: Docker Image for the Driver 
- **Application Hostname Suffix**: The exposed hostname suffix that will be used to create routes for Spark UI and Jupyter Notebook

*NOTE: The template assumes that the request and the limits are same for all the containers. If you wish to have different limits, it's recommended to edit the template*


## If running through the Command line:

* Download the oc client for openshift
* oc login
* oc new-project <project-name>
* Change the variables
* oc process -f spark-template.yml | oc apply -f -
  
### Deleting

* oc delete all -l app=<CLUSTER_NAME>-spark
* oc delete configmap -l app=<CLUSTER_NAME>-spark
* oc delete secret -l app=<CLUSTER_NAME>-spark
* You might also want to delete the persistent volume created by the setup by oc delete pvc -l app=<CLUSTER_NAME>-spark
