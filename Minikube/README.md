# Minikube

## Install MiniKube

[https://kubernetes.io/docs/tasks/tools/install-minikube/](https://kubernetes.io/docs/tasks/tools/install-minikube/)

### Mac

```bash
brew install minikube
```

## VM dirviers

[https://kubernetes.io/docs/setup/learning-environment/minikube/#specifying-the-vm-driver](https://kubernetes.io/docs/setup/learning-environment/minikube/#specifying-the-vm-driver)

You can change the VM driver by adding the --driver=<enter_driver_name> flag to minikube start. For example the command would be.

+ virtualbox
+ vmwarefusion
+ docker (EXPERIMENTAL)
+ kvm2 (driver installation)
+ hyperkit (driver installation)
+ hyperv (driver installation) Note that the IP below is dynamic and can change. It can be retrieved with minikube ip.
+ vmware (driver installation) (VMware unified driver)
+ parallels (driver installation)
+ none (Runs the Kubernetes components on the host and not in a virtual machine. You need to be running Linux and to have Docker + installed.)

## Start MiniKube

```bash
minikube start --mount-string="$HOME/go/src/github.com/nginx:/data"
```

## check MiniKube Status

```bash
minikube status
```

## Use Local Docker Images with MiniKube

```bash
eval $(minikube docker-env)

#undo it
eval "$(docker-machine env -u)"
```

## Move docker image to minikube

```bash
docker save "${1}" | pv | (eval $(minikube docker-env) && docker load)
```
