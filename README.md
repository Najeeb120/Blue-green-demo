

### Create Kind cluster

```bash
kind create cluster --name deployment-test-cluster --image= kindest/node:v1.29.14@sha256:8703bd94ee24e51b778d5556ae310c6c0fa67d761fae6379c8e0bb480e6fea29

### 2.Deploy Jenkins on Kind cluster

1. Create new namespace for Jenkins installation.
2. Create Jenkins deployment with Service and Volumes.


### 3. Create JenkinsPipeline for Blue-Green deployment

1.  Define stages in Jenkins pipeline ranging from checkout SCM to deploying target version.

2. Once testing is completed switch traffic from application version-1 to version-2.
