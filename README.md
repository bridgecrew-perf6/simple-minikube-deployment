# simple-minikube-deployment

## Overview




## Quick start 

See more detailed instructions below

* Install Docker
* Install minikube
* Start minikube (ensure ingress addon is enabled)
* Run following to build Docker image

```

```

* Run the following to create the deployment

```
kubectl apply -f deployment.yaml
```

Check with the following command

```
kubens helloworld
kubectl get pods
kubectl get svc
```

Run this command in a seperate terminal to allow connection to the ingress

```
minikube service -n ingress-nginx ingress-nginx-controller
```

Run `ps -ef | grep "docker@127.0.0.1"` to find the port used for proxying the
ingress and then run a `curl` command to access the nginx server

```
curl -vvv -H "Host: hello.internal" localhost:55243/hello
```

Replace `55243` with the actual proxy port

NOTE: Running `minikube tunnel` may also work but this requires admin
      rights and I was trying to avoid that 
	  

## Setup 

### Installation 

This has been tested on an M1 Mac

#### Docker

Downloading [docker desktop](https://www.docker.com/products/docker-desktop/)

Start Docker Desktop


#### Homebrew

The easiest way to get many of the scripts and utilies is to use
[homebrew](https://brew.sh) I've followed the [untar anywhere](https://docs.brew.sh/Installation#untar-anywhere) approach as this doesn't require admin rights
and hopefully doesn't risk damage to my system.

Also the `$PATH` variable is set with homebrew at the end so it can't override 
anything that may be important from a security point of view


#### Minikube

Installed using homebrew

```
brew install minikube
```

Also added `kubectx` as this is useful to avoid long `kubectl` command that 
require the context and namespace being specified all the time

```
brew install kubectx
```


### Startup

#### Minikube

Start minikube using the following command

```
minikube start --driver=docker
```

Enable the ingress addon

```
minikube addons enable ingress
```


### Development 

#### Python setup

Using Python3 run the following commands under `service/python`

```
python3 -mvenv .matrix
. .matrix/bin/activate
pip install -r requirements
```

To setup the initial packages needed for the code ran this:

```
pip install flask && pip freeze > requirements.txt
```


## Troubleshooting

### Minikube Ingress controller not running

When applying the deployment you get this result

```
kubectl get ingress
NAME            CLASS           HOSTS   ADDRESS   PORTS   AGE
hello-ingress   nginx-example   *                 80      6m16
```

Check the ingress controller is running

```
kubectl get pods -n ingress-nginx
```

If this returns no pods, then delete the ingress and check the instructions
above for startup of minikube to ensure the ingress addon is running

```
kubectl delete ingress hello-ingress
```



## References

### k8s Manifests

* [Creating a pod](https://kubernetes.io/docs/concepts/workloads/pods/)
* [Creating a namespace](https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-organizing-with-namespaces)
* [Services](https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/)
* [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
* [Define environment variables for container](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)


### k8s

* [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
* [Connecting applications with services](https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/)

### Minikube

* [Accessing applications](https://minikube.sigs.k8s.io/docs/handbook/accessing/)
* [Minikube github repo](https://github.com/kubernetes/minikube)
* [Setup Ingress controller](https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/)
* [Howto use local docker images with minikube](https://stackoverflow.com/questions/42564058/how-to-use-local-docker-images-with-minikube)
* [README section on using minikube docker for images](https://github.com/kubernetes/minikube/blob/0c616a6b42b28a1aab8397f5a9061f8ebbd9f3d9/README.md#reusing-the-docker-daemon)


### Docker 

* [Accessing HyperKit VM for Docker Desktop on Mac](https://stackoverflow.com/questions/39739560/how-to-access-the-vm-created-by-dockers-hyperkit)
* [Image for nginx](https://hub.docker.com/_/nginx)


### Homebrew

* [List all files in Homebrew package](https://stackoverflow.com/questions/19010784/list-all-files-in-a-homebrew-package)


### Ingress Controllers

* [Advanced nginx configuration with annotations](https://docs.nginx.com/nginx-ingress-controller/configuration/ingress-resources/advanced-configuration-with-annotations/)
* [Artifact hub - ingress-nginx](https://artifacthub.io/packages/helm/ingress-nginx/ingress-nginx)
* [Change ports ingress controller uses](https://stackoverflow.com/questions/57926545/change-kubernetes-nginx-ingress-controller-ports)


### Python

* [Creating requirements.txt from pip](https://stackoverflow.com/questions/19135867/what-is-pips-equivalent-of-npm-install-package-save-dev)

