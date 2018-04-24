# k8s-faq
Kubernetes Frequently Asked Questions 


## Spark on Kubernetes 

### How to compile spark with Kubernetes and create your own docker image 

1. git clone http://github.com/apache/spark
2. cd spark
3. ./dev/make-distribution.sh --name mdl-spark --pip --tgz -Phadoop-2.7 -Pkubernetes
4. cp spark-2.4.0-SNAPSHOT-bin-mdl-spark.tgz ..
5. cd ..
6. tar zxf spark-2.4.0-SNAPSHOT-bin-mdl-spark.tgz
7. cd spark-2.4.0-SNAPSHOT-bin-mdl-spark
8. ./bin/docker-image-tool.sh -t latest -r dharmeshkakadia build
9. ./bin/docker-image-tool.sh -t latest -r dharmeshkakadia push
