## Installing Signal Sciences using Helm

### Introduction:

This repository contains an example of embedding the Signal Sciences Agent in Reverse Proxy Mode via the Helm package manager.  Simply include these files in your master package.  This will include the installation of the Signal Sciences agent as a separate container.  

- [/sigsci-agent/Dockerfile](/sigsci-agent/Dockerfile)
  - This container is a sidecar running the Signal Sciences Agent in the same pod as your web server container.
- [values-sigsci.yaml](values-sigsci.yaml)
  - This yaml file contains custom configuration options for Helm, mainly used to load the Signal Sciences container.

### Setup:

####  1. Set Agent Keys in values-sigsci.yaml

#### 2. Set the listener IP/port amd forwarding IP/port in values-sigsci.yaml. 
The Signal Sciences agent will set in front of the web application as a reverse proxy container.  Web traffic will be sent to the configured listener IP/port, processed by the agent, then forwarded via the forwarding IP/port on to the web application.

#### 3. Build the Signal Sciences Agent container
*Again, set whatever resgistry + repository name you'd like here, just be sure to set* `controller.extraContainers.image:` *to match in [values-sigsci.yaml](values-sigsci.yaml)*
```
cd ../sigsci-agent
docker build -t myregistry/sigsci-agent:latest .
```

#### 4. Use Helm to install the Signal Sciences yaml configuration
```
helm install stable/nginx-ingress -f values-sigsci.yaml

# (Optional alternate command if RBAC is enabled)
# helm install stable/nginx-ingress -f values-sigsci.yaml --set rbac.create=true
```
