<h1>Create an own cluster</h1>
<h2>Prerequisites</h2>

  - If there's a running FW, it needs to be disabled otherwise we need to add ports and services later on.
 Kubernetes works with cli so-called kubectl. In order to use this easily, let's install bash-completion if hasn't installed yet:
 
 ```bash
 yum install -y vim git bash-completion
 ```
 also needs to append to ~/.bashrc:
 
 ```bash
 echo 'source <(kubectl completion bash)' >>~/.bashrc
 ```

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
 rm /etc/containerd/config.toml
 ```
 ```bash
 systemctl restart containerd.service
 ```
 
  - From this point, we should be able to initialize the cluster:

```bash
kubeadm init
```
