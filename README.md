# Add Basic Authentication to a service in Kubernetes with NGINX
This repo is a sample of how to use NGINX as an authentication proxy for a service in Kubernetes AKS.

There can be a requirement to be able to provide some *basic* level of authentication to a service in an AKS cluster. NGINX has a number of these capabilities, so this repo describes how to configure NGINX for this.

It should be noted that there are no real secrets in this repo - just test ones for NGINX that can be applied to the NGINX deployment. So these credentials are of no use outside in the wider world.

## Create a Test Application.
It's useful to have a test application that is the target for the proxy. This is really simple and it is just about exposing a web application as a Kubernetes service internal to the cluster. You can bring your own, but one is supplied in this repo.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
     replicas: 1
     selector:
       matchLabels:
         app: myapp
     template:
       metadata:
         labels:
           app: myapp
       spec:
         containers:
         - name: myapp
           image: dockersamples/static-site:latest
           ports:
           - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
     selector:
       app: myapp
     ports:
     - name: http
       port: 80
       targetPort: 80
```
As can be seen above:
1. there is a deployment called myapp that uses an image *static-site*
2. This is exposed on port 80
3. There is a service *myapp* which also is exposed on port 80.
