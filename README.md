# Lab 5 – Containers and Kubernetes

## Basics

- **Introduction to containers, e.g., Docker**: [Docker Get Started](https://docs.docker.com/get-started/)
  - Lightweight virtualization
  - Key concepts:
    - Dockerfile, Image, Container, Image repositories
- **Kubernetes, container orchestration**: [Kubernetes Overview](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)
  - Cluster, Pods, Services, etc.
  - Used to manage a cluster of hosts/containers
  - Why do we need orchestration? What functionalities are provided by Kubernetes (k8s) that exceed those of a simple `dockerd` daemon?
- **Alternative implementations**:
  - Containers: apptainer, podman, lxc
  - Container orchestration: OpenShift, Rancher, Nomad, commercial services: e.g., AWS ECS, Azure AKS, Google GKE

## Prerequisites

- Preferably a Linux environment (Linux VM, or Windows/WSL 2)
- Docker
- Kubernetes cluster. While installing a full-fledged cluster is quite complicated, it’s possible to prepare a development “mini k8s cluster” with the help of:
  - [Minikube](https://minikube.sigs.k8s.io/docs/start): Simple, only 1-node cluster
  - [Kind](https://kind.sigs.k8s.io): Kubernetes in Docker, possible to emulate multiple nodes on a single machine
  - Or others...

## Assignments

### 1) Dockerize AWS-CLI (3 points)

1. **Create a Dockerfile with aws-cli, built from source files** (using tar.gz file / make).
   - Use a multi-stage build to create a small image. Also use a small base image, e.g., based on Alpine Linux.
   - Read more on Python and Docker multistage build.
   - Follow best practices for writing Dockerfiles. In particular, read about `ADD` or `COPY`.
2. **Build an image based on your Dockerfile and test it**.
   - Use a volume (`-v` option) to make AWS credentials available in the container.

### 2) Kubernetes deployment (5 points)

1. **Create a k8s cluster using Amazon Elastic Kubernetes Service (EKS)**.
   - You can also use minikube, kind, or any other Kubernetes distribution, or existing cluster.
   - Minikube, by default, uses its own internal Docker daemon. This daemon doesn’t know anything about images built previously. Prepare your environment by directing it to access the internal Docker daemon by using the `$(minikube docker-env)` command and rebuild your images. This way, images will be available within the k8s cluster.
     - [Guide on using local Docker images with Minikube](https://medium.com/bb-tutorials-and-thoughts/how-to-use-own-local-docker-images-with-minikube-2c1ed0b0968)

2. **Using Helm, install an NFS server and provisioner in the cluster**.
   - Go to `charts/nfs-server-provisioner` for a README.
   - Pay attention to configuration parameters, in particular, override `storageClass.name` which denotes the name of the `StorageClass` that you’ll have to use when creating Persistent Volume Claims.

3. **Create a Persistent Volume Claim** which will bind to an NFS Persistent Volume provisioned dynamically by the provisioner installed in the previous step.

4. **Create a Deployment with an HTTP server** (e.g., Apache or Nginx).
   - The web content directory should be mounted as a volume using the PVC created in the previous step.

5. **Create a Service** associated with the Pod(s) of the HTTP server Deployment.

6. **Create a Job** which mounts the PVC and copies a sample content through the shared NFS PV.

7. **Test the HTTP server** by showing the sample web content in a browser.

