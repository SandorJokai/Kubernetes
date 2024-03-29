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
 sed -i -e 's/gpgcheck=1/gpgcheck=0/' -e 's/repo_gpgcheck=1/repo_gpgcheck=0/' setup-kubetools-new-PRERELEASE.sh
 ```
 ```bash
 ./setup-kubetools-new-PRERELEASE.sh
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
kubeadm init      # --apiserver-advertise-address <IPAddress>
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

<h2>Create a deployment</h2>
In order to use kubernetes for deploying applications and publishing them, of course we need to have a Container Network Interface (CNI):
(Possibly this won't work anymore...)

```bash
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```
Try this instead:

```bash
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

There's more available in the official documentation, I use weave for now.

```bash
kubectl get nodes
```

We should see the "Ready" status this time, which means we can add the worker nodes finally.
Don't forget to run all commands on workers before we run the "kubeadm join" cmd on workers:

```bash
yum install -y vim git bash-completion
```
...
```bash
systemctl restart containerd.service
```

Note: Everytime we add a new node, worth checking the network status:
```bash
kubectl get pods -n kube-system
```
If we see 1/2 in READY column next to the new nodes, a rule need to be added in iptables:
```bash
iptables -t nat -I KUBE-SERVICES -d 10.96.0.1/32 -p tcp -m comment --comment "default/kubernetes:https cluster IP" -m tcp --dport 443 -j KUBE-MARK-MASQ
```
Check again and should be seen 2/2 READY this time

If we like to publish an app from host, we need to expose the service to an external IP:
```bash
kubectl patch svc <my-service> -p '{"spec":{"externalIPs":["192.168.1.119"]}}'
```

If we see this on the master:

```bash
kubectl get nodes
NAME          STATUS   ROLES           AGE   VERSION
k8s-master    Ready    control-plane   32m   v1.24.3
worker-test   Ready    <none>          28s   v1.24.3
```

means we can make deployments. :)

![npm package](https://img.shields.io/badge/kubernetes-1.24.3-blue.svg)
![npm package](https://img.shields.io/badge/centos-7.9.2009-purple.svg)
![npm package](https://img.shields.io/badge/git-1.8.3-red.svg)
![npm package](https://img.shields.io/badge/go-1.18.3-cyan.svg)
![npm package](https://img.shields.io/badge/docker-20.10.17-blue.svg)
