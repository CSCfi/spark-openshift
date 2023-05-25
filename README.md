# spark-openshift
Run Apache Spark on Openshift. Based on https://github.com/Uninett/helm-charts

## Quickstart:

The template provisions a Spark cluster based on the configuration on Openshift. 
**NOTE: Make sure, you create a new openshift project in order to run this. It is also recommended that you have only one cluster per openshift project**

*If you are looking for instructions on how to install custom spark libraries on top of your spark cluster, [click here](https://github.com/CSCfi/spark-openshift/blob/master/installing_libraries.md)*

There are 3 deployments created - 
1. **Spark Master**: Serves as the master of the cluster and runs Spark UI
2. **Spark Worker**: Runs worker instances in the cluster
3. **Jupyter Notebook**: Serves as the Spark Driver, where one writes the code and submits it to the master

### Variables:

Listed below are some of the variables that should be changed.

**Please NOTE : The values for the CPU and Memory should only be changed (to avoid errors) after checking the project quota allocated to your Openshift project.** You should increase or request the admins to increase it for you, if needed.

#### Mandatory Required Values:
- **Cluster Name**: Unique identifier for your cluster
- **Username**: Username for authenticating and logging into your Spark cluster and Jupyter (Recommended: create a new username, don't use any existing one)
- **Password**: Password for authenticating and logging into your Spark cluster and Jupyter (Recommended: create a new password, don't use any existing one)
- **Worker Replicas**: Number of workers to have (Default: 4)

- **Storage Size**: Persistent storage volume size (Default: 10Gi)

#### Optional Required Values:
- **Enable Jupyter Lab**: Specify whether if you want to use Jupyter Lab instead of the default Jupyter Notebook (Default: false) 
- **Master CPU**: Number of cores for the master node of the cluster
- **Master Memory**: Memory for the master node of the cluster
- **Worker CPU**: Number of cores for each worker of the cluster (Default: 2)
- **Worker Memory**: Memory of each worker of the cluster (Default: 4G)

- **Executor Default Cores**: Default value for Spark Executor Cores (See official Spark documention for more) (Default: 2)
- **Executor Default Memory**: Default value for Spark Executor Memory (**Should always be less than the Worker memory!**) (Default: 3G)

- **Driver CPU**: Number of cores for the driver (Jupyter Notebook)
- **Driver Memory**: Memory of the driver (Jupyter Notebook)

#### Do not change the following variables, unless you know what you're doing
- **Master Image**: Docker Image for the Master
- **Worker Image**: Docker Image for the Worker 
- **Driver Image**: Docker Image for the Driver 
- **Application Hostname Suffix**: The exposed hostname suffix that will be used to create routes for Spark UI and Jupyter Notebook

*NOTE: The template assumes that the request and the limits are same for all the containers. If you wish to have different limits, it's recommended to edit the template*


## If running through the Command line:

* Download the oc client for openshift
* `oc login`
* `oc new-project <project-name>`
* `oc process -f spark-template.yml -p CLUSTER_NAME="cluster_name" -p USERNAME="username" -p PASSWORD="password" | oc apply -f -`

### Adding more workers
By default, the template will deploy 4 workers. If you know that you will need more than 4 at the beginning, you can use this command:  
```sh
oc process -f spark-template.yml -p CLUSTER_NAME="cluster_name" -p USERNAME="username" -p PASSWORD="password" -p WORKER_REPLICAS="x"
```

If after the deployment you need more workers, you can type this command to increase the number of worker pods:  
```sh
oc scale dc/<your_deployment_name> --replicas=x
```

You can list your DeploymentConfig with this command:
```sh
oc get dc
```
*You can also downscale the number of pods with the command above*

### Deleting

* `oc delete all -l app=spark`
* `oc delete configmap -l app=spark`
* `oc delete secret -l app=spark`
* You might also want to delete the persistent volume created by the setup by typing `oc delete pvc -l app=spark`

### Adding more storage from OpenShift UI
From OpenShift console
* open Storage -> Create Storage -> Fill required fields and press Create button.
* Application -> Deployments -> For each of the items go Configuration tab -> Add Storage -> Fill desired Mount Path ie. **/mnt/data**  -> Type **Volume Name** or leave empty for automatically generated -> press Add.

Automatic redeployment starts and after repeating above steps to all items, new pvc will be mounted to application.
