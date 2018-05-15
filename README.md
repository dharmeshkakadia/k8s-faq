# k8s-faq
Kubernetes Frequently Asked Questions 


## Spark on Kubernetes 

### How to compile spark with Kubernetes and create your own docker image with Azure Storage Support(WASB/ADLS)

You need java 8 for compiling. Java 9 is known to run into issues.


1. git clone http://github.com/apache/spark
2. cd spark
3. ./dev/make-distribution.sh --name mdl-spark --pip --tgz -Phadoop-2.7 -Pkubernetes
4. cp spark-2.4.0-SNAPSHOT-bin-mdl-spark.tgz ..
5. cd ..
6. tar zxf spark-2.4.0-SNAPSHOT-bin-mdl-spark.tgz
7. cd spark-2.4.0-SNAPSHOT-bin-mdl-spark
8. cd jars
9. curl -O http://repo1.maven.org/maven2/commons-lang/commons-lang/2.6/commons-lang-2.6.jar && \
    curl -O http://repo1.maven.org/maven2/org/mortbay/jetty/jetty-util/6.1.26/jetty-util-6.1.26.jar && \
    curl -O http://repo1.maven.org/maven2/com/microsoft/azure/azure-storage/2.0.0/azure-storage-2.0.0.jar && \
    curl -O http://repo1.maven.org/maven2/org/apache/hadoop/hadoop-azure/2.7.3/hadoop-azure-2.7.3.jar && \
    curl -O http://repo1.maven.org/maven2/com/microsoft/azure/azure-data-lake-store-sdk/2.1.5/azure-data-lake-store-sdk-2.1.5.jar && \
    curl -O http://repo1.maven.org/maven2/org/apache/hadoop/hadoop-azure-datalake/3.0.0-alpha3/hadoop-azure-datalake-3.0.0-alpha3.jar
10. cd ..
8. ./bin/docker-image-tool.sh -t latest -r dharmeshkakadia build
9. ./bin/docker-image-tool.sh -t latest -r dharmeshkakadia push

You can quickly verify the Azure storage integration by running the word count with your account key.

bin/spark-submit \
--deploy-mode cluster \
--class org.apache.spark.examples.JavaWordCount \
--master k8s://http://127.0.0.1:8001/ \
--conf spark.executor.instances=2 \
--conf spark.app.name=spark-wc-wasb \
--conf spark.kubernetes.container.image=dharmeshkakadia/spark:latest \
--conf spark.hadoop.fs.azure.account.key.<YOUR_ACCOUNT_KEY>.blob.core.windows.net=4tZV+I55A== \
--jars local:///opt/spark/examples/jars/spark-examples_2.11-2.4.0-SNAPSHOT.jar \
local:///opt/spark/examples/jars/spark-examples_2.11-2.4.0-SNAPSHOT.jar wasb://<YOUR_STORAGE_CONTAINER_NAME>@<YOUR_STORAGE_ACCOUNT>.blob.core.windows.net/<PATH>

## Tensorflow on Kubernetes 


## ACS Engine

### How do I enable alpha features of Kubernetes while creating cluster through Azure ACS engine?

Specify following in your spec, as described here : https://github.com/Azure/acs-engine/blob/master/docs/clusterdefinition.md#apiserverconfig

```
....
"kubernetesConfig": {
    "apiServerConfig": {
        "--enable-admission-plugins": ""
    }
}
....
```

### How do I get the logs of the last run of scheduled spark pipeline?
```
APP_NAME=pyspark-example
kubectl logs -f $(kubectl get sparkapplications/$(kubectl get scheduledsparkapplications/$APP_NAME -o=jsonpath='{.status.pastRunNames[0]}') -o jsonpath='{.status.driverInfo.podName}')
```
