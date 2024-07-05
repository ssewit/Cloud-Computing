# Word Count with Kibernets and Spark
Apache Spark: This high-performance analytics engine tackles large datasets with ease and offers advanced analytical capabilities.

Kubernetes: This open-source platform automates container deployment, scaling, and management.

The Spark-Kubernetes Advantage:

By running Spark on Kubernetes, we achieve:

Flexibility: Easily adapt resource allocation for various tasks.

Resource Efficiency: Optimize resource usage by scaling Spark workloads dynamically.

Enhanced Scalability: Effortlessly handle larger datasets and more complex data processing needs.

# Design

GKE Cluster Creation: A single GKE cluster with high memory is easily created and configured using the gcloud command.

Shared Storage: The NFS Server Provisioner is deployed through Helm, enabling efficient data sharing across Spark nodes.

Data Preparation: Application JAR files and input data are copied to the persistent storage for access by Spark jobs.

Spark Deployment: Bitnami's Helm chart simplifies the deployment of Apache Spark on the cluster, allowing for resource management and job execution.

A high-memory GKE cluster with a single node is set up for efficient data processing.

The NFS Server Provisioner, deployed via Helm charts, provides shared persistent storage across the node, ensuring data accessibility and consistency.

A Persistent Volume Claim (PVC) is created for persistent storage of application JAR files and input data, guaranteeing their survival even when the pod restarts.

The gcloud command streamlines the creation and configuration of a single-node GKE cluster optimized for memory.

Helm charts automate the deployment of the NFS Server Provisioner, enabling shared storage for data.

Application JAR files and input data are uploaded to the persistent storage for access by Spark jobs.

Finally, Bitnami's Helm chart simplifies Spark deployment on the cluster, allowing for resource management and job execution.

Input data and application JAR files reside in the PVC, readily available to all Spark nodes.

Spark jobs (e.g., WordCount, PageRank) are submitted to the Spark master, which efficiently distributes tasks across worker nodes for parallel processing.

Workers leverage the shared storage to access input data and store output results.

Final output data is conveniently stored back in the PVC for easy retrieval, verification, and further analysis.

# Implementation

```
$gcloud container clusters create spark \
  --num-nodes=1 \
  --machine-type=e2-highmem-2 \
  --region=us-west1
```

<img width="605" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/e74e3f79-ccab-4527-a306-1b3ec28c13c4">

Install the NFS Server Provisioner

```
$helm repo add stable https://charts.helm.sh/stable
$helm install nfs stable/nfs-server-provisioner \--set persistence.enabled=true,persistence.size=5Gi
```

Create a persistent disk volume and a pod to use NFS spark-pvc.yaml

```
$vi spark-pvc.yaml
```

put the following in the text

```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
name: spark-data-pvc
spec:
accessModes:
- ReadWriteMany
resources:
requests:
storage: 2Gi
storageClassName: nfs
---
apiVersion: v1
kind: Pod
metadata:
name: spark-data-pod
spec:
volumes:
- name: spark-data-pv
persistentVolumeClaim:
claimName: spark-data-pvc
containers:
- name: inspector
image: bitnami/minideb
command:
- sleep
- infinity
volumeMounts:
- mountPath: "/data"
name: spark-data-pv
```

Apply the yaml descriptor

```
$kubectl apply -f spark-pvc.yaml
```

Create and prepare your application JAR file

```
$docker run -v /tmp:/tmp -it bitnami/spark -- find
/opt/bitnami/spark/examples/jars/ -name spark-examples* -exec
cp {} /tmp/my.jar \;
```

Add a test file with a line of words that will be used later for the word count test
```
$echo "how much wood could a woodpecker chuck if a woodpecker could chuck wood" > /tmp/test.txt
```

Copy the JAR file containing the application, and any other required files, to the PVC using the mount point
```
$kubectl cp /tmp/my.jar spark-data-pod:/data/my.jar
$kubectl cp /tmp/test.txt spark-data-pod:/data/test.txt
```
Make sure the files a inside the persistent volume
```
$kubectl exec -it spark-data-pod -- ls -al /data
```

Deploy Apache Spark on the Kubernetes cluster using the Bitnami Apache Spark Helm chart and supply it with the configuration 
```
$helm repo add bitnami https://charts.bitnami.com/bitnami
$helm install spark bitnami/spark -f spark-chart.yaml
```

Get the external IP of the running pod
```
$kubectl get svc -l "app.kubernetes.io/instance=spark,app.kubernetes.io/name=spark"
```

<img width="498" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/ca944b87-4a17-4c7f-80fc-532afda57ae0">

Submit a word count task
```
kubectl run --namespace default spark-client --rm --tty -i --restart='Never' \
--image docker.io/bitnami/spark:3.0.1-debian-10-r115 \
-- spark-submit --master spark://YOUR-EXTERNAL-IP-ip-ADDRESS:7077 \
--deploy-mode cluster \
--class org.apache.spark.examples.JavaWordCount \
/data/my.jar /data/test.txt
```

# View Results

```
$kubectl get pods -o wide | grep WORKER-NODE-ADDRESS
$kubectl exec -it <worker node name>-- bash
$cd /opt/bitnami/spark/work
$cat <taskname>/stdout
```

You will see something like this

<img width="624" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/50726fa2-0554-4785-9e68-d34d073b1efe">


Execute the spark master pods

```
$kubectl exec -it spark-master-0 -- bash
```

Stark pyspark

```
$pyspark
```

Exit pyspark with
```
exit()
```

Go to the directory where pagerank.py located
```
cd /opt/bitnami/spark/examples/src/main/python
```

Run the pagerank using pyspark with the number of iteration you want it to go through (mine is 2)
```
spark-submit pagerank.py /opt 2
```

You will see something like this 
<img width="418" alt="image" src="https://github.com/ssewit/Cloud-Computing/assets/105317921/bd887ec9-da55-446a-af26-c3c4bbed6c19">
