Setting up Kafka on Kubernetes on AWS with Debezium plugins

If you want to run Kafka on AWS, you could use the managed service of Amazon. But if you want more flexibility you could also run your own Kafka on top of Amazon's managed Kubernetes service. Advantage is more flexibility, and the costs is different and probably lower as you pay for compute resources used by EKS instead. 

### Kubernetes
Create a  Kubernetes cluster on AWS
### Kubectl
Install kubectl if you didn't already do that.

### Kafka cluster
Then set up Kafka Strimzi. As a starter, you could use [this template](kafka-strimzi.yml):
   ```apply -f kafka-strimzi.yml```
### Kafka Connect
Then you have to configure connectors. For most connectors you would need to install plugins. See [this blog-post](https://strimzi.io/blog/2021/03/29/connector-build/) as well as other information on strimzi-website for more information.

