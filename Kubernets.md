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

he following are other major advantages to using containers:

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

In contras, many applications are not well suited for a containerized environment. For example, application accessing low level hardware information, such as memory, file systems, and devices may be unreliable dua to container limitations.

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

# Running minikube on Docker Desktop
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

# Kuberctl
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