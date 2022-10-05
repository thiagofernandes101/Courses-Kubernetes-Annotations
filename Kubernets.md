# **Containers**
Is a set of one or more processes that are isolated from the rest of the system.

In traditional software applications, there usually are dependencies on other libraries, configuration files, or services that are provided by the runtime environment, which is a physical host or virtual machine and those dependencies mentioned earlier are installed as part of the host.

The major drawback of those environments is the possibility to break the software application everytime there is a need to update the runtime environment.

Futhermore, applications must be stopped before updating the associated dependencies. To minimize downtime, organiztions use complex systems to provide high availability. Maintaning multiple applications on a single host becomes cumbersome, since any update, as previously mentioned, has the potential to brak one of applications.

The image below shows the difference between container and operation systems.

![Container and operation system differences](./Assets/ContainerVersusOperatingSystemsDifferences.drawio.svg)

Using containers provides many of the benefits as virtual machines, such as security, storage and network isolation, demanding fewer hardware resources. Also they are quick to start and to terminate and isolate the libraries and the runtime resources, such as cpu and storage, and minimize the impact of os updates.

Futhermore, beyond improving efficiency, elasticity and reusability of hosted applications, container usage improves application portability. The Open Container Initiative (OCI) provides a set of industry standards that define a container runtime specification and a container image specification. 

The image specification defines the format for the bundle of files and metadata that form a container image. When you build a container image compliant with the OCI standard, you can use any OCI-compliant container engine to execute the contained application. There are many container engines available to manage and execute containers, including Rocket, Drawbridge, LXC, Docker, and Podman.

The following are other major advantages to using containers:

### **Low hardware footprint**
- Containers use OS-internal features to create an isolated environment where resources are
managed using OS facilities such as namespaces and cgroups. This approach minimizes the
amount of CPU and memory overhead compared to a virtual machine hypervisor. Running an
application in a VM isolates the application from the running environment, but it requires a
heavy service layer to achieve the level of isolation provided by containers.

### **Environment isolation**
- Containers work in a closed environment where changes made to the host OS or other
applications do not affect the container. Because the libraries needed by a container are self-
contained, the application can run without disruption. For example, each application can exist
in its own container with its own set of libraries. An update made to one container does not
affect other containers.

### **Quick deployment**
- Containers deploy quickly because there is no need to install the entire underlying operating
system. Normally, to support isolation, a host requires a new OS installation, and any update
might require a full OS restart. A container restart does not require stopping any services on
the host OS.

### **Multiple environment deployment**
- In a traditional deployment scenario using a single host, any environment differences could
break the application. By using containers, all application dependencies and environment
settings are encapsulated in the container image.

### **Reusability**
- The same container can be reused without the need to set up a full OS. For example, the
same database container that provides a production database service can be used by each
developer to create a development database during application development. By using
containers, there is no longer a need to maintain separate production and development
database servers. A single container image is used to create instances of the database
service.

In short, containers are an ideal approach when using microservices for application development which each service is encapsulated in a lightweight and reliable container environment that can be deployed to a production or development environment. The collection of containerized services required by an application can be hosted on a single machine, removing the need to manage a machine for each service.

In contrast, many applications are not well suited for a containerized environment. For example, application accessing low level hardware information, such as memory, file systems, and devices may be unreliable due to container limitations.

## **Limitations of containers**
When using containers in a production environment, enterprises often require the following capabilities:

- Easy communication between a large number of services;
- Resources limits on applications;
- Ability to respond to application usage spikes by increasing or decreasing replicas;
- Gradual rollout of a new release to different users.

Enterprises often require a container orchestration technology because container runtimes, by themselves, do not adequately address the above requirements.

# **Kubernetes**
Kubernetes is a container ochestration platform that simplifies the deployment, management, and scaling of containerized applications.

A pod is the smallest manageable unit in Kubernets and consists of, at least, one container that is used by the container ochestration to manage the containers and their resource limits as a single unit.

Kubernetes offers the following features on top of a container engine:

### **Service discovery and load balacing**
- Kubernetes enables inter-service communication by assigning a single DNS entry to each set
of containers. This way, the requesting service only needs to know the target's DNS name,
allowing the cluster to change the container's location and IP address. This permits load-
balancing requests across the pool of container replicas.

### **Horizontal scaling**
- Applications can scale up and down manually or automatically with a configuration set, by
using either the command-line interface or the web UI.

### **Self-healing**
- Kubernetes can use user-defined health checks to monitor containers to restart and
reschedule them in case of failure.

### **Automated rollout**
- Kubernetes can gradually release updates to your application's containers while checking their
status. If something goes wrong during the rollout, Kubernetes can roll back to the previous
version of the application.

### **Secrets and configuration maps**
- You can manage the configuration settings and secrets of your applications without rebuilding
containers. Configuration maps store these settings in a way that decouples them from the
pods and containers using them. Application secrets can include any configuration setting
that must be kept private, such as user names, passwords, and service endpoints.

### **Operators**
- Operators are packaged Kubernetes applications that bring the knowledge of application
lifecycles into the Kubernetes cluster. Applications packaged as Operators use the Kubernetes
API to update the cluster's state by reacting to changes in the application state.

Because of its versatility, Kubernetes can solve the same problems in different ways depending on need and opinions evolving, because of this, into differents distributions based on:

- <ins>The target size of the cluster</ins>: from small single-node clusters to large scale clusters of hundreds of thousands of nodes.

- <ins>The location of the nodes</ins>: either locally on the developer workstation, on premises (such as a private data center), on the cloud, or a hybrid solution of those two.

- <ins>The ownership of the management</ins>: self-managed clusters versus Kubernetes-as-a-service.

The following table shows a classification for some of the most popular Kubertes distributions:

|                                    | Big Scale                                                | Small Scale                                                 |
|------------------------------------|----------------------------------------------------------|-------------------------------------------------------------|
| Self-Managed - Local               |                                                          | minikube, CodeReady Containers, Microk8s, Docker Kubernetes |
| Self-Managed-On Premises / Hybrid  | Red Hat OpenShift, VMWare Tanzu, Rancher                 |                                                             |
| Kubernetes-as-a-Service - On Cloud | OpenShift Dedicated, Google Container Engine, Amazon EKS | Developer Sandbox                                           |

It's also important to mention that each distribution provides different approaches (or none) for adding capabilities to Kubernetes. Following is a comparison summary of Kubertes features.

|                | minikube                    | Developer Sandbox                         |
|----------------|-----------------------------|-------------------------------------------|
| DNS            |                             |                                           |
| Dashboard      | Dashboard add-on            | OpenShift Console                         |
| Ingress        | NGINX Ingress add-on        | Operator-controled HAProxy                |
| Storage        | Local or Gluster add-ons    | Red Hat OpenShift Data Foundation         |
| Authentication | Administrator minikube user | Developer used restricted to 2 namespaces |
| Operators      | OLM add-on. No restrictions | Limited to RHOAS and SErvice Binding      |

# **Running minikube on Docker Desktop**
1. Download the latest release from the github [link](https://github.com/kubernetes/minikube/releases).

2. [Get started with Docker](https://www.docker.com/get-started/).

3. Make sure to have virtualization enable in the bios (can be seen if its enabled, or not, in the windows task manager -> performance)

> OBS: During the process, I got the error: <ins>**Failed to start virtualbox VM. Running "minikube delete" may fix it: creating host: create: precreate: This computer doesn't have VT-X/AMD-v enabled. Enabling it in the BIOS is mandatory**</ins>. This probably occour because of the wls2 that I have running, so the following command was executed as administrator: <ins>minikube start</ins>

``` bash
minikube start

minikube v1.26.1 on Microsoft Windows 11 Home Single Language 10.0.22000 Build 22000
✨  Automatically selected the docker driver. Other choices: hyperv, virtualbox, ssh
📌  Using Docker Desktop driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
    > gcr.io/k8s-minikube/kicbase:  386.61 MiB / 386.61 MiB  100.00% 11.86 MiB
    > gcr.io/k8s-minikube/kicbase:  0 B [________________________] ?% ? p/s 23s
🔥  Creating docker container (CPUs=2, Memory=8100MB) ...
🐳  Preparing Kubernetes v1.24.3 on Docker 20.10.17 ...
   ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

4. Since I have Docker installed, it created an image with root privileges.

> OBS: To use virtual box or hyper-v, here are the following commands:
> - <ins>minikube start --driver=hyperv</ins>
> - <ins>minikube start --driver=virtualbox</ins>

5. Enable the ingress addon with the following command: <ins>minikube addons enable ingress</ins>
``` bash
minikube addons enable ingress

💡  ingress is an addon maintained by Kubernetes. For any concerns contact minikube on GitHub.
You can view the list of minikube maintainers at: https://github.com/kubernetes/minikube/blob/master/OWNERS
💡  After the addon is enabled, please run "minikube tunnel" and your ingress resources would be available at "127.0.0.1"
    ▪ Using image k8s.gcr.io/ingress-nginx/kube-webhook-certgen:v1.1.1
    ▪ Using image k8s.gcr.io/ingress-nginx/kube-webhook-certgen:v1.1.1
    ▪ Using image k8s.gcr.io/ingress-nginx/controller:v1.2.1
🔎  Verifying ingress addon...
🌟  The 'ingress' addon is enabled
```

# **Kuberctl**
The kubectl tool is a Kubernetes command-line tool that allows you to interact with your Kubernetes cluster. It provides an easy way to perform tasks such as creating resources or
redirecting cluster traffic. The kubectl tool is available for the three main operating systems (Linux, Windows and macOS).

# Installing kuberctl in Windows

1. Create a new folder, such as C:\kube, to use as the destination directory of the kubectl binary download.

2. [Download the latest release of the kubectl binary](https://dl.k8s.io/release/v1.21.0/bin/windows/amd64/kubectl.exe) and save it to the previously created folder. **It isn't mandatory to run the .exe file**

3. Add the downloaded binary file to the windows environment path.

4. Run the command <ins>kubectl version --client</ins> to verify that kubectl has been installed successfully.
``` bash
kubectl version --client

Client Version: version.Info{Major:"1", Minor:"24", GitVersion:"v1.24.2", GitCommit:"f66044f4361b9f1f96f0053dd46cb7dce5e990a8", GitTreeState:"clean", BuildDate:"2022-06-15T14:22:29Z", GoVersion:"go1.18.3", Compiler:"gc", Platform:"windows/amd64"}
Kustomize Version: v4.5.4
```

5. Donwload the [DO100x-apps](https://github.com/RedHatTraining/DO100x-apps) repository.
> OBS: The DO100x-apps contains a script that handles all configurations for you under the setup directory. The script you should run depends on your operating system (Linux, macOS or Windows) and the Kubernetes distribution you use (Minikube or the OpenShift Developer Sandbox). If you run the Minikube script, it will configure Minikube to work as a Kubernetes cluster with restricted access. You will only have access to two namespaces. This way, we simulate a real Kubernetes cluster, where usually developers do not have full access.

6. In the DO100x-apps directory, run the command <ins>Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass</ins>. This command allows you to run unsigned PowerShell scripts in your current terminal session.

7. Make sure that OpenSSL is installed and its bin folder path is configured in the environment variables.
> OBS: Usually, git alread has an executable openssl file. So all it is needed to do is add a new environment variable using the git openssl bin folder path (C:\Program Files\OpenSSL-Win64\bin).

8. Run the command <ins>.\setup\windows\setup.ps1</ins>

# Running and interacting with an application

As it was mentioned before, Kubernetes does not view a container directly and instead works with the concept of pods as showed at the image below.

![Container view](./Assets/ContainerView.drawio.svg)

In this case, each pod can container multiple containers and one container och ochestration platform contain more than one pod.

With that in mind, to create a pod with an image, the following commands can be executed.

> OBS: Before executing the commands to create an image, make sure the kubectl is configured to use minikube (in case it is not just run <ins>minukube start</ins>) and the kubectl context refers to the user-dev by running <ins>kubectl config set-context --current --namespace=[your user]-dev</ins>
>
> In case the namespaces are not know (probably forgotten), then run the command <ins>kubectl config get-contexts</ins>
>
> If everything is alright, then the command to show kubernetes pods should show the message that no resources were found in the user namespace
>
> ``` bash
> kubectl get pods
> No resources found in thiag-dev namespace.
> ```
> <br>

<br>

``` bash
# Create a pod with an image
kubectl run webserver --image=registry.access.redhat.com/ubi8/httpd-24:1-161
pod/webserver created

# Confirm the pod is running
kubectl get pods
| NAME      | READY  | STATUS   | RESTARTS  | AGE     |
|-----------|--------|----------|-----------|---------|
| webserver | 1/1    | Running  | 0         | 111s    |

# Connect to the pod by using kubectl exec
kubectl exec --stdin --tty webserver -- /bin/bash

# View the contents of the httpd configuration file within the pod
cat /etc/httpd/conf/httpd.conf

# Disconnect from the pod by using the exit command.
exit
```

To create a new pod from a yml file, the following commands can be used as an example.

``` bash
# Get a yml file base on an image
# In this case, the content will be created based on the specified image using the command to run a pod
kubectl run probes --image=quay.io/redhattraining/do100-probes:external --dry-run=client -o yaml
> apiVersion: v1
> kind: Pod
> metadata:
>   creationTimestamp: null
>   labels:
>     run: probes
>   name: probes
> spec:
>   containers:
>   - image: quay.io/redhattraining/do100-probes:external
>     name: probes
>     resources: {}
>   dnsPolicy: ClusterFirst
>   restartPolicy: Always
> status: {}

# Create a file called probes-pod.yml with the generated content (unix commands to write a file are necessary)
vim probes-pod.yml

# Create a new pod based on a file. In this case, the probes-pod.yml file
kubectl create -f probes-pod.yml

# Verify that the pod is running
kubectl get pods
| NAME      | READY  | STATUS   | RESTARTS  | AGE     |
|-----------|--------|----------|-----------|---------|
| probes    | 1/1    | Running  | 0         | 23s     |
| webserver | 1/1    | Running  | 0         | 37s     |
```

To modify the metadata.labels.app field of the pod manifest and apply the changes, the following commands can be used.

``` bash
# Add a new label, for example, in the probes-pod.yml file
vim probes-pod.yml

# The command below will create a new pod in case it does not exist, or update an existing pod
kubectl apply -f probes-pod.yml

# Verify the changes by describing a specific pod
kubectl describe pod probes
```

# **Deploy managed applications**

One of the features of Kubernetes is that it enables developers to use a declarative approach for automatic container life cycle management, in other words, developers can declare what should be the status of the application and Kubernetes will update the container to reach that state.

For that to happen, some basic values must be declared:

- The container images used by the application;
- The number of instances (replicas) of the application that Kubernetes must run simultaneously;
- The strategy for updating the replicas when a new version of the application is available.

With this information, Kubernetes deploys the application, keep the number of replicas and terminates or redeploys application containers when the state of the application does not match the declared configuration. Also, it continuously revisits this information and updates the state of the application accordingly.

This behavior enables important features of Kubernetes as a container management platform:

### **Automatic deployment**

- Kubernetes deploys the configured application without manual intervention.

### **Automatic scaling**

- Kubernetes creates as many replicas of the application as requested. If the number of replicas requested increases or decreases, then Kubernetes automatically creates new containers (scales-up) or terminates and automatically spins up a new one to match the expected replica count.

### **Automatic restart**

- If a replica terminates unexpectedly or becomes unresponsive, then Kubernetes deletes the associated container and automatically spins up a new one to match the expected replica count.

### **Automatic rollout**

- When a new version of the application is detected, or a new configuration applies, Kubernetes automatically updates the existing replicas. Kubernetes monitors this rollout process to make sure the application retains the declared number of active replicas.

# Creating a Deployment

A Deployment resource container all the information Kubernetes needs to manage the life cyle of the application's containers.

Using kuberctl is a simple way to create a deployment resource. For that, the following command must be executed.

```bash
kubectl create deployment [deployment-name] --image [image] --replicas=[number of replicas]
```

This command creates a deployment resource in which instructs Kubernetes to deploy the number of replicas specified of the application pod and to use the container image.

The get command <ins>get [deployment-name]</ins> retrieve the Deployment resource from Kubernetes.

Also, it is possible to use the <ins>--output yaml<ins> parameter to get detailed information about the resource in the YAML format. Alternatively, it is also possible to use the short <ins>-o yaml</ins> version.

```bash
kubectl get deployment [deployment-name] -o yaml
```

The Kubernetes declarative deployment approach enables you to use the GitOps principles, which focuses on a versioned repository, such as git, which stores the deployment configuration.

Following GitOps principles, the Deployment manifest can be stored in YAML or JSON format in the application repository. Then, after the appropriate changes, the Deployment can be created manually or programmatically by using the <ins>kubectl apply -f [deployment-file]</ins> command.

The Deployment resource manifest can also be edited directly from the command line. The <ins>kubectl edit deployment [deployment-name]</ins> command retrieves de Deployment resource and opens it in a local text editor.

# Deployment schema

The following depicts the main entries in a Deployment manifest:

``` yml
apiVersion: apps/v1
kind: Deployment # Manifest kind identifies the resource type.
metadata: # Manifest metadata. Include deployment name and labels.
  labels:
  app: versioned-hello
  name: versioned-hello
spec: # Deployment specification contains deployment configuration
  replicas: 3 # Number of desired replicas of the container
  selector:
    matchLabels:
      app: versioned-hello
  strategy:
      type: RollingUpdate # Deployment strategy to use when updating pods
  template:
    metadata:
      labels:
        app: versioned-hello
    spec: #Includes a list of pod definitions for each new container created by the deployment as well as other fields to control container management
      containers:
      - image: quay.io/redhattraining/versioned-hello:v1.14 DO100B-K1.22-en-2-7067502Chapter 1 | Deploying ManagedApplications #Container image used to create new containers
          name: versioned-hello
status: #Current status of the deployment. This section is automatically generated and updated by Kubernetes
   replicas: 3 #The current number of replicas currently deployed
```

# Replica:

The replicas section under the <ins>spec</ins> section (also denoted as <ins>sec.replicas</ins> section) declares the number of expected replicas that Kubernetes should keep running. Kubernetes will continuously review the number of replicas that are running and responsive, and scale accordingly.

# Deployment strategy:

When the application changes dues to an image change or a configuration change, Kubernetes replaces the old running containers with updated ones. However, just redeploying all replicas at once can lead to problems with the application, such as:

1. Leaving the application with too few running replicas.
2. Creating too many replicas and leading to an overcommitment of resources.
3. Rendering the application unavailable if new version is faulty.

To avoid this issues, Kubernetes defines two strategies:

### **1. RollingUpdate:**
Kubernetes terminates and deploys pods progressively. This strategy defines a maximum amount of pods unavailable anytime. It defines the difference between the available pods and the desired available replicas. The RollingUpdate strategy also defines an amount of pods deployed at any time over the number of desired replicas. Both values default to 25% of the desired replicas.

### **2. Recreate:**
This strategy means that no issues are expected to impact the application, so Kubernetes terminates all replicas and recreates them on a best effort basis.

# Template:
When Kubernetes deploys new pods, it needs the exact manifest to create the pod. The <ins>spec.template.spec</ins> sectinon holds exactly the same structure as a Pod manifest. Kubernetes uses this section to create new pods as needed.

The following entries in the template deserve special attention:

- The <ins>spec.template.spec.container.image</ins> entry declares the image (or images) Kubernetes will deploy in the pods manages by this Deployment.

- Kubernetes uses the <ins>spec.template.spec.containers.name</ins> entry as a prefix for the names of the pods it creates.

# Labels:
Labels are key-value pairs assigned in resource manifests. Both developers and Kubernetes use labels to identify sets of groupes resources, such as resources belonging to the same application or environment. Depending on the position inside the Deployment, labels have a different meaning:

- **metada.labels:**  Labels applied directly to the manifest, in this case the Deployment resource. Objects matching these labels with the <ins>kubectl get king --selector="key=value"</ins> For example, <ins>kubectl get deployment --selector="app=myapp"</ins> returns all deployments with a label app=myapp in the <ins>metadata.labels</ins> section.

- **spec.selector.matchLabels.selector:** Determine what pods are under the control of the Deployment resource. Even if some pods in the cluster are not deployed via this Deployment, if they match the labels in this section then they will count as replicas and follow the rules defined in this Deployment manifest.

- **spec.template.metadata.labels:** Like the rest of the template, it defines how Jubernetes creates new pods using this Deployment. Kubernetes will label all the pods created by this Deployment resource with these values.

# Example deploying managed applications

- The deployment image will be the container image quay.io/redhattraining/do100-versioned-hello:v1.0-external.

1. Create a default deployment that starts one replica of the do100-versioned-hello application and review the deployment was successful.

```bash
kubectl create deployment do100-versioned-hello --image quay.io/redhattraining/do100-versioned-hello:v1.0-external
> deployment.apps/do100-versioned-hello created

kubectl get deployment do100-versioned-hello
> NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
> do100-versioned-hello   1/1     1            1           2m2s

kubectl describe deployment do100-versioned-hello
> Name:                   do100-versioned-hello
> Namespace:              thiag-dev
> CreationTimestamp:      Tue, 04 Oct 2022 19:54:28 -0300
> Labels:                 app=do100-versioned-hello
> Annotations:            deployment.kubernetes.io/revision: 1
> Selector:               app=do100-versioned-hello
> Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
> StrategyType:           RollingUpdate
> MinReadySeconds:        0
> RollingUpdateStrategy:  25% max unavailable, 25% max surge
> Pod Template:
>   Labels:  app=do100-versioned-hello
>   Containers:
>    do100-versioned-hello:
>     Image:        quay.io/redhattraining/do100-versioned-hello:v1.0-external
>     Port:         <none>
>     Host Port:    <none>
>     Environment:  <none>
>     Mounts:       <none>
>   Volumes:        <none>
> Conditions:
>   Type           Status  Reason
>   ----           ------  ------
>   Available      True    MinimumReplicasAvailable
>   Progressing    True    NewReplicaSetAvailable
> OldReplicaSets:  <none>
> NewReplicaSet:   do100-versioned-hello-5544cf98b (1/1 replicas created)
> Events:
>   Type    Reason             Age   From                   Message
>   ----    ------             ----  ----                   -------
>   Normal  ScalingReplicaSet  3m5s  deployment-controller  Scaled up replica set do100-versioned-hello-5544cf98b to 1
```

2. Enable high availability by increasing the number of replicas to two.

```bash
kubectl scale deployment do100-versioned-hello --replicas=2
> deployment.apps/do100-versioned-hello scaled

kubectl get pods -w
> NAME                                    READY   STATUS    RESTARTS   AGE
> do100-versioned-hello-5544cf98b-d4wcd   1/1     Running   0          33s
> do100-versioned-hello-5544cf98b-jftb9   1/1     Running   0          11m
```

3. Verify high availability features in Kubernetes. Kubernetes must ensure that two replicas are available at all times. Terminate one pod and observe how Kubernetes creates a new pod to ensure the desired number of replicas.

```bash
kubectl delete pod do100-versioned-hello-5544cf98b-d4wcd
> pod "do100-versioned-hello-5544cf98b-d4wcd" deleted

kubectl get pods -w
> NAME                                    READY   STATUS    RESTARTS   AGE
> do100-versioned-hello-5544cf98b-jftb9   1/1     Running   0          13m
> do100-versioned-hello-5544cf98b-x8bh9   1/1     Running   0          5s
```

4. Deploy a new version of the application and observe the default deployment rollingUpdate strategy.

```bash
# Look for the image: quay.io/redhattraining/do100-versionedhello:v1.0-external entry and change the image tag from v1.0-external to v1.1-external. Save the changes and exit the editor.
kubectl edit deployment do100-versioned-hello
> deployment.apps/do100-versioned-hello edited

kubectl get pods -w 
> NAME                                     READY   STATUS    RESTARTS   AGE
> do100-versioned-hello-68f449dddc-2hfzw   1/1     Running   0          2m31s
> do100-versioned-hello-68f449dddc-4v6mz   1/1     Running   0          2m23s
```

5. Delte the deployment to clean the cluster. Kubernetes automatically deletes the associated pods.

```bash
kubectl delete deployment do100-versioned-hello
> deployment.apps "do100-versioned-hello" deleted
```