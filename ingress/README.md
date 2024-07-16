In order to work ingress, firstly we need to install the controller:
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.0/deploy/static/provider/baremetal/deploy.yaml
```

wait a couple of minutes then verify:
```bash
kubectl get all -n ingress-nginx
```
(might not all pods be running, but must be running for ingress-nginx-controller)

since we use k8s in virtualbox and have no load balancers, need to run this:
```bash
kubectl patch svc ingress-nginx-controller -n ingress-nginx -p '{"spec": {"type": "LoadBalancer", "externalIPs":["<external_ip>"]}}
```

verify with curl/browser its reachibility:
```bash
curl -I http://<external_ip>
```

SSL/TLS

	- get a valid cert (I can use mine, just need to make the dns locally in /etc/hosts)
 	- add the cert and key resources in base64 format to deployment
	- create a secret for the ssl:
```bash
kubectl create secret tls my-tls-secret -n <namespace> --key ssl/ssl.key --cert ssl/ssl.crt
```

launch the deployment, wait a bit then test. Should see the application as secured in the browser.

Useful links:
	for the controller: https://docs.k0sproject.io/v1.23.6+k0s.2/examples/nginx-ingress/
	for LB: https://docs.k0sproject.io/v1.23.6+k0s.2/examples/metallb-loadbalancer/ (this wasn't necessary in my case)
