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
