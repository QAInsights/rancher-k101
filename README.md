# rancher-k101
Rancher K101 training files and notes

## Overview

This repo contains guides and notes for [Rancher K101 course](https://academy.rancher.com)

### Installation of RKE

1. Install Vagrant
2. Install rke
3. Clone this repo
4. Create kubernetes master and worker nodes:
```
vagrant up
```
5. Install RKE distribution:
```bash
rke up
```
6. Set up kube config file for current sesstion
```bash
export KUBECONFIG=$(pwd)/kube_config_cluster.yml
```
7. Verify that you can connect to K8S cluster
```
kubectl get nodes
```

## Quiz

## 1. Intro to Rancher and RKE

### 1.2 Discovering RKE

* RKE deploys Kubernetes components as:  Docker containers
* SSH is used to orchestrate RKE across servers: True
* Which is not something required on hosts to run RKE? SELinux is set to
  enforcing
* Cluster.yml contains all the information RKE needs to provision a Kubernetes
  cluster: True
* You can supply your own certificates that Rancher will serve for its UI/API:
  True
* rke start will start the provisioning of Kubernetes.: False
* Which versions of Kubernetes does RKE support by default?: The Latest patch
  releases from the three most recent minor releases

### 1.3 Day Two Operations for RKE

* What are the two files that RKE generates that need to be stored safely?
  cluster.rkestate and kube_config_cluster.yml
* What are the two locations RKE can write backup archives to? S3, Local Disk
* Upgrading Kubernetes involves changing the version in the system_images key
  and running rke upgrade.: False
* Certificates cannot be regenerated after install.: False
* You can add additional nodes to an RKE cluster after initial deployment. :
  True
* Which of the following is NOT something that RKE manages? Determining the
  amount of nodes for a Kubernetes cluster

## 2. Installing and Managing Rancher

### 2.1 Installing Rancher with Docker

* When might you want to use the Docker method for installing Rancher? For
  sandbox or demonstration purposes
* The first step in backing up a single container Docker install is stopping
  the container.: True
* Upgrading Rancher is similar to making a backup except we start a container
  with a new image tag: True
* Why do you want to delete the old Rancher container after an upgrade?:  So it
  doesnt startup accidentally and overwrite the data volume

### 2.2 Installing Rancher with Kubernetes

* Which of the following is a valid type of load balancer when fronting Rancher
  server?: A TCP-based load balancer
* Its possible to migrate from alpha to latest, but not to stable.: False
* Which type of TLS certificate can you use with Rancher?: All of options
* Lets Encrypt doesnt actually use the email address you give them.: False
* How do you make a backup of Rancher in RKE?: Make a normal RKE backup
* You can restore a Rancher/RKE backup with zero downtime.: False
* You can roll back a failed Rancher upgrade with helm rollback.: False

## 3. Deploying Kubernetes With Rancher

### 3.1 Designing and Provisioning Clusters

* Which features are unavailable with Hosted Kubernetes Clusters?: etcd
  backup/restore and some monitoring of etcd
* What is the difference between a Node Template and an RKE Template?:  Node
  templates describe how to provision nodes and RKE templates how to deploy
  Kubernetes
* Where can you find detailed information on the port requirements for each
  role?: Rancher Documentation
* When you create a cluster in Rancher, what RBAC role are you assigned?  Owner

### 3.2 Deploying Kubernetes with Rancher


* Which is true about RKE Templates?: They specify the Kubernetes configuration
  options that should be used
* Node Templates allow you to deploy new nodes without having to re-enter the
  node parameters. True
* What is the purpose of a cloud provider?: To allow Kubernetes to leverage
  cloud-specific features such as load balancers and network storage
* To use the Custom provider only requires that a supported version of Docker
  to be installed on the node.: True
* The command provided by the custom provider can be used repeatedly on each
  node you want to provision for that role.: True
* If you already deployed a Kubernetes cluster, you cant ever import that
  cluster into Rancher.: False
* Which is the best provider if you are provisioning with Terraform?: Custom
  Provider
* Imported clusters have a special import flag that enables you to manage the
  nodes from within Rancher.: False
* The best practice is to colocate etcd, control plane, and worker roles on a
  single node in downstream Kubernetes clusters.: False
* Which network provider works with Windows clusters? Flannel

### 3.3 Performing basic troubleshooting

* How do you check the logs for the Rancher api-server?: Using Kubectl with the
  logs subcommand for one of the pods in the cattle-system namespace
* Where can you find details on node sensors?: kubectl describe node < node
  name >
* Infrastructure issues on a particular node are often revealed by the kubelet
  logs: True
* Docker daemon logs are useful for understanding if an image cannot be pulled
  for some reason. : True

### 3.4 Performing advanced troubleshooting

* How do you find the logs for etcd in an RKE cluster?:  Using Docker logs
* Control plane nodes only have one active controller manager so log data may
  not be present equally across nodes.: True
* What is the purpose of nginx-proxy?:  So non-control plane nodes can reach
  control plane components without knowing node addresses
* Which log is the best place to start the troubleshooting process?: Rancher
  API server logs
* What is a common cause of CNI failure?:  Network port restrictions between
  nodes

## 4. Managing Kubernetes with Rancher

### 4.1 Editing Clusters

* After provisioning a cluster, you can change the network provider in the
  cluster options on the Edit screen.: False
* In what circumstances should you take a backup?:  Before starting a
  Kubernetes upgrade
* When you update the version of a Kubernetes cluster, it immediately starts
  updating the worker, control plane, and etcd nodes with new components.: True

### 4.2 Using CLI Tools

* Where does the kubectl CLI tool make API calls?:  The Kubernetes API of the
  cluster you are working with
* Where is the link to download the Rancher CLI?:  In the bottom right of the
  UI
* How can you see what API keys are active for your account?:  Click on your
  user profile icon in the top right and select "API & Keys"

### 4.3 Interacting With Monitoring and Logging

* Enabling Advanced Monitoring deploys Prometheus and Grafana.: True
* How do you see the Grafana dashboard for a given component? By clicking the
  Grafana icon next to a component in the UI
* What is the role of a notifier?:  Handles delivery of alerts from Prometheus
  to a destination such as Email or Pagerduty
* What types of clusters permit alerts for etcd?:  Clusters that are built
  using RKE
* How do you enable project vs. cluster-wide monitoring?:  Selecting the
  Monitoring section under tools at either the Cluster or Project level
  contexts and enabling first at the desired context
* Which of the following is NOT one of the log destinations?: InfluxDB


### 4.4. Configuring Namespaces and Namespace Groups

* A Project can have more than one namespace.: True
* Which CNI supports project level isolation?: Canal
* Resources in different namespaces in the same Project cannot use the same
  name.: False
* Which of the following can only be assigned to a namespace?: Service
  discovery records
* Rancher recommends assigning Pod Security Policies to Projects.: False
* What is a resource quota for?:  Controlling the amount of resources a Project
  can use
* Why would you set a default resource limit on a Project?:  To set a limit on
  workloads that dont set one in their configuration

### 4.5 Working inside of a Project

* Which of these is the most accurate?:  Namespaces cannot move to a Project
  with resource quotas
* You cannot use Rancher Advanced Monitoring to scrape metrics from your
  workloads.: False
* Where are notifiers configured?: At the cluster level
* Rancher does not offer any monitoring without first activating Advanced
  Monitoring.: False
* If logging is enabled in the Project, it must be deactivated at the Cluster
  level.: False
* Which of these is used to create direct queries to Prometheus for alerts in
  Rancher? : PromQL

## 5. Running Kubernetes Workloads

### 5.1 Deploying and managin Workloads

* Which of the following WOULD NOT prevent a workload from being scheduled on a
  node?: Liveness Probe
* You can deploy a workload by importing YAML through the Rancher UI.: True
* Why should you not use the latest tag for container images?: All of the above
* Rancher will create services for ports that you configure in the Workload
  deployment screen.: True
* Where can Rancher pull environment variables from?: All of the above
* The best way to deploy a workload is to tell Rancher the specific node to run
  the workload on.: False
* Which of the following is NOT a health check method for Workloads?: UDP
* Which of the following CANNOT be used as a volume?: Environment variable
* Changing the console type has no effect on Ranchers ability to collect log
  data from stdout/stderr.: False
* How do you tell Rancher to launch a Pod with multiple containers?: Launch the
  pod with the primary container and then add sidecars
* How many revisions can you list when doing a rollback?: All of them

### 5.2 Using Persistent Storage

* Common cloud providers have their storage providers built into
  Rancher/Kubernetes.: True
* Which of the following provisioning types allows Kubernetes to allocate
  storage as users request it?: Dynamic Provisioning
* Where are storage classes configured?: Cluster level
* How can you attach storage to a workload?: All of the above
* You can only have one StorageClass defined per cluster.: False
* Rancher allows you to have multiple default StorageClasses.: False

### 5.3 Dynamic Data with ConfigMaps, Secrets and Certificates

* Where do ConfigMaps get stored?:  Kubernetes datastore
* You can store text files in ConfigMaps.: True
* You can only assign a Secret to a Project.: False
* Kubernetes encrypts Secrets at rest by default.: False
* Where can you find more information about how to configure additional
  security for Secrets in RKE?:  Rancher Hardening Guide

### 5.4 Understanding Service Discovery and Load Balancing

* The ClusterIP for a Service changes whenever the Workload is upgraded.: False
* How do you configure a Kubernetes Ingress to terminate TLS/SSL?:  By
  uploading your certs as a Certificate and configuring the Ingress with a
  reference to that Certificate correct
* Which of the following is NOT true about Ingress?:  It is only available in
  the cloud
* Which DNS record type does an ExternalName mimic?  A record or CNAME record
* Which layer has the most intelligence for traffic management?  Layer 7
* What is required for LoadBalancer services to provision?  Cloud provider
  configuration
* Which of the following is NOT available as an ingress controller?:  None of
  the above
* What can you use to dynamically generate certificates?:  Cert Manager

### 5.5 Discovering the Rancher Application Catalog

* Which of the following is NOT a valid format for Rancher to use as a catalog
  repo?:  A SVN server
* What happens when there is a newer version of a Helm chart made available in
  a catalog repo?:  The page of installed apps will show that the app has an
  upgrade available
* Helm apps are installed through a Rancher service account with administrator
  privileges.: False
* What is the difference between a Helm chart and a Rancher app?:  Helm charts
  require you to enter key/value pairs for answers and Rancher apps use a form
  to configure the answers
* A catalog that is enabled at the Global scope can be disabled at the Project
  scope.: False
* Who can add new catalogs?:  It depends on the RBAC permissions for the
  account and the scope at which the catalog is being added
* You can clone an application into the same namespace.: False
* You can roll back an application to a previous version from the UI.: True
* What is the proper way to delete an application?:  Delete the application and
  then delete the namespace


## Install Rancher as container


### Run runcher with Podman as self-signed certificate

```bash
sudo podman run -d --restart=always -p 80:80 -p 443:443 -v /opt/rancher:/var/lib/rancher rancher/rancher:v2.4.1
```

## Installing Rancher with Kubernetes

### Deploying Into RKE

1. Install nodes and RKE
2. Install helm3
```bash
wget https://get.helm.sh/helm-v3.2.4-linux-amd64.tar.gz -O /tmp/helm-v3.2.4-linux-amd64.tar.gz
tar xvzf /tmp/helm-v3.2.4-linux-amd64.tar.gz -C /tmp
mv /tmp/linux-amd64/helm ~/bin/helm3
chmod +x ~/bin/helm3
```
3.  Add Helm3 Rancher repo
```bash
helm3 repo add rancher-latest https://releases.rancher.com/server-charts/latest
```
4. Follow other steps from [guide](https://rancher.com/docs/rancher/v2.x/en/installation/k8s-install/helm-rancher/#3-create-a-namespace-for-rancher)




# Tips

## Reset admin password

```bash
kubectl --kubeconfig $KUBECONFIG -n cattle-system exec $(kubectl --kubeconfig $KUBECONFIG -n cattle-system get pods -l app=rancher | grep '1/1' | head -1 | awk '{ print $1 }') -- reset-password
```
