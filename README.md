# k8s-faq
Kubernetes Frequently Asked Questions 


## Spark on Kubernetes 

### How to compile spark with Kubernetes and create your own docker image 

You need java 8 for compiling. Java 9 is known to run into issues.


1. git clone http://github.com/apache/spark
2. cd spark
3. ./dev/make-distribution.sh --name mdl-spark --pip --tgz -Phadoop-2.7 -Pkubernetes
4. cp spark-2.4.0-SNAPSHOT-bin-mdl-spark.tgz ..
5. cd ..
6. tar zxf spark-2.4.0-SNAPSHOT-bin-mdl-spark.tgz
7. cd spark-2.4.0-SNAPSHOT-bin-mdl-spark
8. ./bin/docker-image-tool.sh -t latest -r dharmeshkakadia build
9. ./bin/docker-image-tool.sh -t latest -r dharmeshkakadia push


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
