<h1>Create a deployment</h1>
<h2>Preview</h2>
Ampache is a world-wide famous online music player which can be built as part of a kubernetes deployment fine.
In the next section, I'll mention all the steps I did on a brand new k8s cluster in order to host an app such this.
Had to replace weave with calico as I noticed there wasn't DNS resolution inside my running containers causing further issues.
<h2>What CNI is?</h2>
In a nutshell, it's Container Network Interface which is required for configuring network properly. Further, the calico is a third party plugin for using CNI.
<h2>Install calico</h2>

```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.1/manifests/crds.yaml
```
```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.1/manifests/custom-resources.yaml
```
```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.1/manifests/calico.yaml
```
As a next step what other component we need is an ingress controller. Again, there's many variant available but I've chosen traefik this time.
> It's pretty simple and easy to use
<h2>Install traefik</h2>

> Make sure there is helm previously installed with version 3.X

```bash
helm repo add traefik https://traefik.github.io/charts
helm repo update
```
```bash
kubectl create namespace traefik
```
```bash
helm install traefik traefik/traefik --namespace traefik
```
One more thing I had to done with since I use baremetal for my k8s cluster is patching my external (bridged IP in virtualbox) IP to use the ingress properly:
```bash
kubectl patch svc traefik -n traefik -p '{"spec": {"type": "LoadBalancer", "externalIPs":["192.168.1.231"]}}'
```
Once done that only one thing which is left is creating the deployment...
<h2>Launch a deployment</h2>

> Ingress is used to expose HTTP and HTTPS routes from outside the cluster to services within the cluster. Besides, can use TLS termination which is my preference in the next deployment.
> Prerequisites for using it properly is getting a domain and valid SSL key and certs. I created a secret resource for using it:
```bash
kubectl create secret tls my-tls-secret -n ampache --cert=ssl/mycert.crt --key=ssl/mycert.key
```
also, was needed to put the cert's contents in the deployment yaml file in base64 coded.
