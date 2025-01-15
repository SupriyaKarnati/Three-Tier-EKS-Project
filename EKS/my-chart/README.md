# Microservices

## Cart Microservice Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **Cart** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **Cart** microservice, ensuring one instance of the service is always running.
- The container image for the **Cart** microservice is dynamically set using Helm values:
  - `{{ .Values.image.repo }}` for the image repository.
  - `{{ .Values.image.version }}` for the image version.
- The container exposes port `8080` for communication with other services and clients.
- Resource limits and requests are defined to optimize the performance of the service:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 100Mi (limit), 50Mi (request)
- The **restartPolicy** is set to `Always`, ensuring the container is automatically restarted if it fails or is terminated.

### **Service**
- The **Service** exposes the **Cart** microservice internally within the Kubernetes cluster.
- It routes traffic to the correct Pods based on the selector `service: cart`, which matches the labels defined in the Deployment.
- The service listens on port `8080` and forwards traffic to the container's internal port `8080`.

This configuration ensures the **Cart** microservice is deployed as a single replica and is accessible to other services in the cluster. The use of Helm values makes it easy to configure and update the container image, while resource requests and limits ensure the service performs efficiently.


## Catalogue Microservice Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **Catalogue** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **Catalogue** microservice, ensuring one instance of the service is always running.
- The container image for the **Catalogue** microservice is dynamically set using Helm values:
  - `{{ .Values.image.repo }}` for the image repository
  - `{{ .Values.image.version }}` for the image version.
- The container exposes port `8080`, which is used for communication.
- Resource limits and requests are set to optimize the microservice’s performance:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 100Mi (limit), 50Mi (request)
- The **restartPolicy** is set to `Always`, ensuring that the container is restarted automatically if it fails or is terminated.

### **Service**
- The **Service** exposes the **Catalogue** microservice to other services within the Kubernetes cluster.
- It routes traffic to the correct Pods using a selector that matches the label `service: catalogue`.
- The service listens on port `8080` and forwards traffic to the container’s internal port `8080`.
  
## Dispatch Microservice Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **Dispatch** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **Dispatch** microservice, ensuring that one instance of the service is running at all times.
- The container image for the **Dispatch** microservice is dynamically configured using Helm values:
  - `{{ .Values.image.repo }}` specifies the image repository.
  - `{{ .Values.image.version }}` specifies the version of the container image.
- The container exposes port `8080` for communication between services.
- Resource limits and requests are set to manage the performance of the microservice:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 100Mi (limit), 50Mi (request)
- The **restartPolicy** is set to `Always`, ensuring that the container is restarted automatically if it fails or is terminated.

### **Service**
- The **Service** exposes the **Dispatch** microservice to other services within the Kubernetes cluster.
- It routes traffic to the appropriate Pods based on the label selector `service: dispatch`.
- The service listens on port `55555` and uses a headless service configuration (`targetPort: 0`) to allow direct communication between Pods in the cluster.

## Payment Microservice Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **Payment** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **Payment** microservice, ensuring that one instance of the service is always running.
- The container image for the **Payment** microservice is dynamically set using Helm values:
  - `{{ .Values.image.repo }}` for the image repository.
  - `{{ .Values.image.version }}` for the image version.
  This setup allows easy management and updates to the microservice's container image.
- **Environment Variables**:
  - `INSTANA_AGENT_HOST`: Configures the Instana agent with the host IP using Kubernetes field reference.
  - `PAYMENT_GATEWAY`: If configured in the Helm values (`.Values.payment.gateway`), it will inject the payment gateway URL into the container environment.
- The container listens on port `8080` for communication with other services.
- Resource limits and requests are set to optimize performance:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 100Mi (limit), 50Mi (request)
- The **restartPolicy** is set to `Always`, ensuring that the container is restarted if it fails or is terminated.

### **Service**
- The **Service** exposes the **Payment** microservice to other services within the Kubernetes cluster.
- It routes traffic to the appropriate Pods based on the selector `service: payment`.
- The service listens on port `8080` and forwards traffic to the container’s internal port `8080`.

## Ratings Microservice Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **Ratings** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **Ratings** microservice, ensuring that one instance of the service is always running.
- The container image for the **Ratings** microservice is dynamically set using Helm values:
  - `{{ .Values.image.repo }}` for the image repository.
  - `{{ .Values.image.version }}` for the image version.
- The container listens on port `80` to receive incoming traffic.
- Resource limits and requests are defined to optimize the microservice's performance:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 100Mi (limit), 50Mi (request)
- The **restartPolicy** is set to `Always`, ensuring that the container is automatically restarted if it fails or is terminated.

### **Service**
- The **Service** exposes the **Ratings** microservice to other services within the Kubernetes cluster.
- It routes traffic to the correct Pods using the selector `service: ratings`, which matches the label defined in the Deployment.
- The service listens on port `80` and forwards traffic to the container's internal port `80`.

## Shipping Microservice Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **Shipping** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **Shipping** microservice, ensuring that one instance of the service is always running.
- The container image for the **Shipping** microservice is dynamically set using Helm values:
  - `{{ .Values.image.repo }}` for the image repository.
  - `{{ .Values.image.version }}` for the image version.
- The container listens on port `8080` for incoming traffic.
- Resource limits and requests are defined to optimize the performance:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 1000Mi (limit), 500Mi (request)
- The **readinessProbe** ensures the microservice is healthy before traffic is routed to it. It performs an HTTP GET request to `/health` on port `8080` with:
  - **initialDelaySeconds**: 5
  - **periodSeconds**: 5
  - **failureThreshold**: 30
  - **successThreshold**: 1
- The **restartPolicy** is set to `Always`, ensuring the container is restarted if it fails or is terminated.

### **Service**
- The **Service** exposes the **Shipping** microservice to other services within the Kubernetes cluster.
- It routes traffic to the appropriate Pods based on the selector `service: shipping`.
- The service listens on port `8080` and forwards traffic to the container’s internal port `8080`.

## User Microservice Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **User** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **User** microservice, ensuring that one instance of the service is always running.
- The container image for the **User** microservice is dynamically set using Helm values:
  - `{{ .Values.image.repo }}` specifies the image repository.
  - `{{ .Values.image.version }}` specifies the image version.
- The container listens on port `8080` for communication between services and clients.
- Resource limits and requests are defined to optimize the microservice's performance:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 100Mi (limit), 50Mi (request)
- The **restartPolicy** is set to `Always`, ensuring the container is restarted if it fails or is terminated.

### **Service**
- The **Service** exposes the **User** microservice to other services within the Kubernetes cluster.
- It routes traffic to the appropriate Pods using the selector `service: user`.
- The service listens on port `8080` and forwards traffic to the container’s internal port `8080`.

## Web Microservice Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **Web** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **Web** microservice, ensuring one instance of the service is always running.
- The container image for the **Web** microservice is dynamically set using Helm values:
  - `{{ .Values.image.repo }}` specifies the image repository.
  - `{{ .Values.image.version }}` specifies the image version.
- The container listens on port `8080` for incoming traffic.
- Resource limits and requests are defined to optimize the performance of the microservice:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 100Mi (limit), 50Mi (request)
- The **restartPolicy** is set to `Always`, ensuring the container is automatically restarted if it fails or is terminated.

### **Service**
- The **Service** exposes the **Web** microservice to other services within the Kubernetes cluster and to external traffic.
- It routes traffic to the appropriate Pods using the selector `service: web`, which matches the labels defined in the Deployment.
- The service listens on port `8080` and forwards traffic to the container’s internal port `8080`.
- The **type** is set to `LoadBalancer`, allowing external access to the microservice via a load balancer, which is particularly useful for managing traffic in production environments.

# Databases

## MongoDB Database Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **MongoDB** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **MongoDB** microservice, ensuring that one instance of the database is running at all times.
- The container image for the **MongoDB** microservice is dynamically set using Helm values:
  - `{{ .Values.image.repo }}` for the image repository.
  - `{{ .Values.image.version }}` for the image version.
- The container listens on port `27017`, the default port for MongoDB, to accept incoming database connections.
- Resource limits and requests are defined to optimize the performance of the microservice:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 200Mi (limit), 100Mi (request)
- The **restartPolicy** is set to `Always`, ensuring the container is automatically restarted if it fails or is terminated.

### **Service**
- The **Service** exposes the **MongoDB** microservice to other services within the Kubernetes cluster.
- It routes traffic to the appropriate Pods using the selector `service: mongodb`.
- The service listens on port `27017` and forwards traffic to the container’s internal port `27017`, which is the default MongoDB port.

## MySQL Database Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **MySQL** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **MySQL** microservice, ensuring that one instance of the MySQL database is always running.
- The container image for the **MySQL** microservice is dynamically set using Helm values:
  - `{{ .Values.image.repo }}` specifies the image repository.
  - `{{ .Values.image.version }}` specifies the image version.
- The container listens on port `3306`, which is the default port used by MySQL for database connections.
- Resource limits and requests are defined to optimize the microservice's performance:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 1024Mi (limit), 700Mi (request)
- The **restartPolicy** is set to `Always`, ensuring that the container is automatically restarted if it fails or is terminated.

### **Service**
- The **Service** exposes the **MySQL** microservice to other services within the Kubernetes cluster.
- It routes traffic to the appropriate Pods using the selector `service: mysql`.
- The service listens on port `3306` and forwards traffic to the container’s internal port `3306`, which is the default MySQL port.

# Message queuing

## RabbitMQ Deployment and Service

This section describes the **Deployment** and **Service** configuration for the **RabbitMQ** microservice in the **Robot-Shop** application.

### **Deployment**
- The **Deployment** defines a single replica (`replicas: 1`) for the **RabbitMQ** microservice, ensuring that one instance of RabbitMQ is always running to handle messaging between microservices.
- The container image for the **RabbitMQ** microservice is set to `rabbitmq:3.7-management-alpine`, which includes the RabbitMQ server with the management plugin enabled, allowing for easy monitoring and management.
- The container listens on two ports:
  - **5672**: The default AMQP (Advanced Message Queuing Protocol) port for RabbitMQ.
  - **15672**: The port for the RabbitMQ management web UI, which is used for monitoring and managing RabbitMQ.
- Resource limits and requests are defined to optimize the microservice's performance:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 512Mi (limit), 256Mi (request)
- The **restartPolicy** is set to `Always`, ensuring that the RabbitMQ container is automatically restarted if it fails or is terminated.

### **Service**
- The **Service** exposes the **RabbitMQ** microservice to other services within the Kubernetes cluster.
- It routes traffic to the appropriate Pods using the selector `service: rabbitmq`.
- The service listens on two ports:
  - **5672**: AMQP port, used for messaging between services.
  - **15672**: HTTP port for the RabbitMQ management interface.

# Inmemory storage

## Redis StatefulSet and Service

This section describes the **StatefulSet** and **Service** configuration for the **Redis** microservice in the **Robot-Shop** application.

### **StatefulSet**
- The **StatefulSet** is used to manage the **Redis** microservice, which requires stable, persistent storage across Pod restarts. It ensures that a stable network identity and persistent storage are available for Redis.
- A single replica (`replicas: 1`) is defined, ensuring only one instance of the Redis service runs in the cluster at a time.
- The container image for Redis is set to `redis:4.0.6`, which is the Redis version used in this deployment.
- The container listens on port `6379`, the default port for Redis.
- The **volumeMounts** section ensures that Redis data is persisted across Pod restarts. The data is mounted to `/mnt/redis` within the container and is backed by a **PersistentVolumeClaim**.
- The **volumeClaimTemplates** section defines a Persistent Volume (PV) claim:
  - `accessModes: [ "ReadWriteOnce" ]` ensures that the volume can be mounted by a single Pod for read-write access.
  - The requested storage size is `1Gi` and is backed by the storage class `gp2` (for non-OpenShift environments).
- Resource limits and requests are defined for the Redis container:
  - **CPU**: 200m (limit), 100m (request)
  - **Memory**: 100Mi (limit), 50Mi (request)
- The **restartPolicy** is set to `Always`, ensuring that the container is automatically restarted if it fails or is terminated.

### **Service**
- The **Service** exposes the **Redis** microservice to other services within the Kubernetes cluster.
- It routes traffic to the appropriate Pods using the selector `service: redis`.
- The service listens on port `6379` and forwards traffic to the container’s internal port `6379`, which is the default Redis port.

# Helm Chart: `chart.yaml`

The `chart.yaml` file defines the basic metadata for the **Robot-Shop** Helm chart. This file contains essential information required for Helm to manage the deployment of the application.

### **Key Fields in `chart.yaml`:**

- **name**: `robot-shop`  
  This specifies the name of the Helm chart. In this case, the name of the application is **Robot-Shop**.

- **version**: `1.1.0`  
  The version number of the Helm chart. This helps track the changes in the chart over time and facilitates version management during deployment.

- **home**: `https://github.com/instana/robot-shop`  
  The homepage URL for the application, directing users to the repository or official documentation. For **Robot-Shop**, this points to the GitHub repository where the source code and further documentation can be found.

- **description**: `Sample microservices application`  
  A brief description of the application. In this case, **Robot-Shop** is described as a sample microservices application, showcasing a set of services that can be deployed in a Kubernetes environment using Helm.

# Ingress Configuration for Robot-Shop

The **Ingress** resource manages external access to the services within the **Robot-Shop** application, specifically routing HTTP traffic to the appropriate microservice.

### **Key Fields in the Ingress Configuration:**

- **namespace**: `robot-shop`  
  The namespace in which the Ingress resource is deployed. In this case, it is under the **robot-shop** namespace, isolating the application within its own namespace for better management and organization.

- **name**: `robot-shop`  
  The name of the Ingress resource, used to reference and manage the Ingress in the Kubernetes cluster.

- **annotations**:  
  Annotations define configuration options for the AWS Application Load Balancer (ALB), which is used as the ingress controller:
  - `alb.ingress.kubernetes.io/scheme: internet-facing` ensures that the ALB is publicly accessible, meaning it can handle internet traffic.
  - `alb.ingress.kubernetes.io/target-type: ip` specifies that the backend targets for the ALB will be identified by their IP addresses.

- **ingressClassName**: `alb`  
  The `ingressClassName` defines which ingress controller to use. In this case, it specifies that the AWS ALB Ingress Controller will manage the routing of traffic.

- **rules**:  
  The rules section defines how incoming HTTP requests are routed:
  - **path**: `/`  
    The path `/` matches all incoming traffic and forwards it to the specified backend service.
  - **backend service**:  
    The traffic is directed to the **web** service on port `8080`, which serves as the entry point to the **Robot-Shop** application.

# `values.yaml` Configuration

The **`values.yaml`** file defines the default configuration values for the **Robot-Shop** Helm chart. It allows users to customize various parameters during deployment, such as container image settings, service configurations, and storage options. Below is an explanation of the key values defined in this file:

### **Image Configuration**
- **repo**: `robotshop`  
  Specifies the container image repository. In this case, the **Robot-Shop** application’s images are pulled from the `robotshop` repository.
  
- **version**: `latest`  
  Defines the version of the container image to be used. By setting this to `latest`, the chart will always pull the most recent image version.
  
- **pullPolicy**: `IfNotPresent`  
  Defines the image pull policy. `IfNotPresent` ensures that the image is pulled only if it is not already present on the node, optimizing deployment time and resource usage.

### **Payment Gateway Configuration**
- **gateway**: `null`  
  This field is used to define the payment gateway configuration for the **payment** microservice. As it is set to `null`, no gateway is specified by default. You can customize it based on your requirements.

### **Openshift Configuration**
- **openshift**: `false`  
  This flag is used to specify whether the application is running on OpenShift. In this case, it is set to `false`, indicating that the deployment is not intended for OpenShift environments. If you are deploying on OpenShift, set this value to `true`.

### **Redis Storage Configuration**
- **redis.storageClassName**: `gp2`  
  Defines the storage class for Redis' PersistentVolume. By default, it is set to `gp2`, which is a general-purpose storage class for AWS environments. You can modify this value to use a different storage class based on your cloud provider or storage requirements.

# steps to deploy on AWS EKS with Ec2 instance:
1. Demo Cluster Setup

This command is used to create a Kubernetes cluster named `demo-cluster-three-tier` using `eksctl` in the AWS region `eu-central-1`. The cluster will be configured for a three-tier architecture, suitable for deploying applications with distinct frontend, backend, and database components.

## Command Overview

```bash
eksctl create cluster --name demo-cluster-three-tier --region eu-central-1

2. Set the EKS Cluster Name

Before proceeding with any configuration or operations on your EKS cluster, you need to specify the name of the cluster you are working with. This is done by exporting the `cluster_name` variable.

## Command

```bash
export cluster_name=<CLUSTER-NAME>

3. Retrieve OIDC Issuer URL for EKS Cluster

This command retrieves the OIDC (OpenID Connect) issuer URL associated with your EKS cluster, which is necessary for configuring the IAM OIDC provider. The OIDC issuer URL allows Kubernetes service accounts to securely assume IAM roles and access AWS resources.

## Command

```bash
oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)

4. Check Existing IAM OIDC Providers

This command checks if an IAM OIDC provider already exists for the specified OIDC issuer ID. It is useful for verifying whether the OIDC provider has been configured previously before attempting to create a new one.

## Command

```bash
aws iam list-open-id-connect-providers | grep $oidc_id | cut -d "/" -f4

5. Download IAM Policy for AWS Load Balancer Controller

This command downloads the IAM policy JSON file required for the AWS Load Balancer Controller installation. The policy grants the necessary permissions for the controller to interact with AWS services.

## Command

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json

6. Create IAM Policy for AWS Load Balancer Controller

This command creates an IAM policy in AWS using the downloaded `iam_policy.json` file. The policy grants the necessary permissions for the AWS Load Balancer Controller to manage AWS resources like Elastic Load Balancers and security groups.

## Command

```bash
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json

7. Create IAM Service Account for AWS Load Balancer Controller

This command creates an IAM service account in your EKS cluster, allowing the AWS Load Balancer Controller to use the necessary IAM role for managing AWS resources such as Elastic Load Balancers and security groups.

## Command

```bash
eksctl create iamserviceaccount \
  --cluster=<your-cluster-name> \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::<your-aws-account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve

8. Add Helm Chart Repository for EKS

This command adds the official Helm chart repository for Amazon EKS. The repository contains the Helm charts required to deploy and manage various AWS-related applications, including the AWS Load Balancer Controller, on EKS clusters.

## Command

```bash
helm repo add eks https://aws.github.io/eks-charts

9. Install AWS Load Balancer Controller Using Helm

This command installs the AWS Load Balancer Controller in your EKS cluster using Helm. The controller manages AWS Elastic Load Balancers (ELBs) and security groups for Kubernetes services, enabling seamless integration between Kubernetes and AWS networking resources.

## Command

```bash
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=<your-cluster-name> \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=<region> \
  --set vpcId=<your-vpc-id>

10. Check AWS Load Balancer Controller Deployment Status

This command checks the status of the AWS Load Balancer Controller deployment in the `kube-system` namespace of your EKS cluster. It is useful for verifying that the controller is running correctly and has the desired number of pods available.

## Command

```bash
kubectl get deployment -n kube-system aws-load-balancer-controller

11. Create IAM Service Account for EBS CSI Driver

This command creates an IAM service account for the Amazon EBS CSI (Container Storage Interface) Driver in your EKS cluster. It associates the service account with the necessary IAM role and policy, allowing the EBS CSI driver to manage EBS volumes in your Kubernetes environment.

## Command

```bash
eksctl create iamserviceaccount \
    --name ebs-csi-controller-sa \
    --namespace kube-system \
    --cluster <YOUR-CLUSTER-NAME> \
    --role-name AmazonEKS_EBS_CSI_DriverRole \
    --role-only \
    --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
    --approve

12. Install Amazon EBS CSI Driver Addon in EKS

This command installs the Amazon EBS CSI Driver as an addon in your EKS cluster. The driver enables dynamic provisioning and management of Amazon EBS volumes for your Kubernetes workloads.

## Command

```bash
eksctl create addon --name aws-ebs-csi-driver --cluster <YOUR-CLUSTER-NAME> --service-account-role-arn arn:aws:iam::<AWS-ACCOUNT-ID>:role/AmazonEKS_EBS_CSI_DriverRole --force

13. Install Robot Shop Application with Helm

This command installs the Robot Shop application in your Kubernetes cluster using Helm. The application will be deployed in the `robot-shop` namespace.

## Command

```bash
helm install --name robot-shop --namespace robot-shop .

14. Install Robot Shop Application with Helm in a New Namespace

This set of commands creates a new Kubernetes namespace called `robot-shop` and then installs the Robot Shop application into that namespace using Helm.

## Commands

```bash
kubectl create ns robot-shop

15. Install Robot Shop Application with Helm

This command installs the Robot Shop application into the `robot-shop` namespace of your Kubernetes cluster using Helm.

## Command

```bash
helm install robot-shop --namespace robot-shop .






