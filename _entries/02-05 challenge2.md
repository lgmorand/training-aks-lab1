---
sectionid: deployingwebapptoaks
sectionclass: h2
title: Deploying the app to AKS (20m)
parent-id: upandrunning
---

In this section, you will import an image (web app) from a public repository in your Azure Container registry and then deploy this web app in your AKS cluster.

#### Import an image in ACR

**Task Hints**

* It's recommended to use the Azure CLI and the `az acr import` command to import the image in your ACR. Refer to [ACR import image](https://learn.microsoft.com/en-us/cli/azure/acr?view=azure-cli-latest#az-acr-import()), or run `az acr import -h` for details
* The image that will be imported is a hello world web app **[docker.io/lgmorand/catnip](docker.io/lgmorand/catnip)**, please use tag v1
* Rename the image to aks-helloworld, tag v1

> **Warning**: If you get an error during the command or can't see container images in the portal, it means that you don't have enough rights. You need to add yourself as contributor to the ACR.

{% collapsible %}
Import image to ACR

```sh
az acr import \
  --name <registry_name> \
  --source docker.io/lgmorand/catnip:v1 \
  --image aks-helloworld:v1
```

{% endcollapsible %}

Check that your image was successfully imported. You can do it graphically (Web Portal) or using [the CLI](https://learn.microsoft.com/fr-fr/cli/azure/acr/repository?view=azure-cli-latest)

{% collapsible %}

```sh
az acr repository list --name <registry_name>
```

You should see an output similar to:

```sh
[
  "aks-helloworld"
]
```

```sh
az acr repository show-tags -n <registry_name> --repository aks-helloworld
```

You should see an output similar to:

```sh
[
  "v1"
]
```

{% endcollapsible %}

#### Create a deployment manifest

The application is now in your own container registry so you don't rely on the availability of hub.docker.com. Now, you need a deployment manifest file to deploy your application. The manifest file allows you to define what type of resource you want to deploy and all the details associated with the workload.

Kubernetes groups containers into logical structures called pods, which have no intelligence. Deployments add the missing intelligence to create your application.

Create a deployment file to deploy your application. The application expect to run on port 5000 and using 'http'.

{% collapsible %}

Create a `deployment.yaml` file with the following contents, and make sure to replace `<registry-fqdn>` with the fully qualified name of YOUR registry (FQDN = full server URL):

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aks-helloworld
spec:
  selector: # Define the wrapping strategy
    matchLabels: # Match all pods with the defined labels
      app: aks-helloworld # Labels follow the `name: value` template
  template: # This is the template of the pod inside the deployment
    metadata:
      labels:
        app: aks-helloworld
    spec:
      nodeSelector:
        kubernetes.io/os: linux
      containers:
        - image: <YOUR-registry-fqdn>/aks-helloworld:v1 # Replace registry-fqdn with the fully qualified name of your registry
          name: aks-helloworld
          ports:
            - name: http
              containerPort: 5000   
```

{% endcollapsible %}

#### Deploy the web app image using the manifest

Use `kubectl` to apply the manifest and deploy the app.

{% collapsible %}

Apply the deployment:

```sh
kubectl apply -f ./deployment.yaml
```

Then ensure it was successful:

```sh
kubectl get deploy aks-helloworld
```

You should see an output similar to:

```sh
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
aks-helloworld   1/1     1            1           32s
```

{% endcollapsible %}

Use `kubectl get pods` to check if the pod is running. Obtain the name of the created pod.

{% collapsible %}

```sh
kubectl get pods
```

You should see an output similar to:

```sh
NAME                               READY   STATUS    RESTARTS   AGE
aks-helloworld-7bb8fc8c5-vksfc     1/1     Running   0          55s
```

{% endcollapsible %}

> **Resources**
>
> * <https://kubernetes.io/docs/concepts/workloads/controllers/deployment/>
> * <https://kubernetes.io/docs/concepts/services-networking/service/>
> * <https://learn.microsoft.com/en-us/training/modules/aks-deploy-container-app/5-exercise-deploy-app/>
