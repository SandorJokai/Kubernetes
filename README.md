![npm package](https://img.shields.io/badge/kubernetes-1.24.3-blue.svg)
![npm package](https://img.shields.io/badge/centos-7.9.2009-purple.svg)
![npm package](https://img.shields.io/badge/git-1.8.3-red.svg)
![npm package](https://img.shields.io/badge/go-1.18.3-cyan.svg)
![npm package](https://img.shields.io/badge/docker-20.10.17-blue.svg)


<h1>Create an own cluster</h1>
<h2>Prerequisites</h2>
<h3>System HW requirements:</h3>

  - 2 cores of CPU at least
  - 2GiB of Memory

  - If there's a running FW, it needs to be disabled otherwise we need to add ports and services later on.
 The swap also needs to be disabled.
 Kubernetes works with cli so-called kubectl. In order to use this easily, let's install bash-completion if hasn't installed yet:
 
 ```bash
 yum install -y vim git bash-completion
 ```
 also needs to append to ~/.bashrc:
 
 ```bash
 echo 'source <(kubectl completion bash)' >>~/.bashrc
 ```
 
Before we go any further, don't forget to add all nodes into /etc/hosts on all nodes.

  - We can use another repo for installing docker and the kubernetes with all those dependencies:
 
 Run all these commands below for doing that:
 
 ```bash
 git clone https://github.com/sandervanvugt/cka
 ```
 ```bash
 cd cka/
 ```
 ```bash
 ./setup-container.sh
 ```
 ```bash
 sed -i -e 's/gpgcheck=1/gpgcheck=0/' -e 's/repo_gpgcheck=1/repo_gpgcheck=0/' setup-kubetools-new.sh
 ```
 ```bash
 ./setup-kubetools-new.sh
 ```
 ```bash
 rm -f /etc/containerd/config.toml
 ```
 ```bash
 systemctl restart containerd.service
 ```
 
 ------------------------------------------------------------------------
 From this point, we should be able to initialize the cluster:

```bash
kubeadm init
```

Follow the instructions after the "Your Kubernetes control-plane has initialized successfully!" message:

```bash
su - regular-user
```
```bash
mkdir -p $HOME/.kube
```
```bash
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```
```bash
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

 - Once done that, we should deploy a pod network to the cluster as well as join the worker nodes. In order to do that, make sure the "kubeadm join..." line be saved into a file for the future usage.

![npm package](https://img.shields.io/badge/kubernetes-1.24.3-blue.svg)
![npm package](https://img.shields.io/badge/centos-7.9.2009-purple.svg)
![npm package](https://img.shields.io/badge/git-1.8.3-red.svg)
![npm package](https://img.shields.io/badge/go-1.18.3-cyan.svg)
![npm package](https://img.shields.io/badge/docker-20.10.17-blue.svg)
