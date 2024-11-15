---
sectionid: enablingpublicaccess
sectionclass: h2
title: Enabling public access (30m)
parent-id: upandrunning
---

Now that you have deployed the application to AKS, it is time to expose it to the internet to allow clients to use it.
By default, Kubernetes blocks all external traffic, so you will need to add a service with a public IP to allow traffic into the cluster.

In this section, you will expose the application by creating a [Service](https://kubernetes.io/docs/concepts/services-networking/service/).

#### Test the app

Make a request to the newly deployed web app and ensure it displays something in your browser using the port 80. Warning the exposed port is not the same than the one expected by the application.

**Task Hints**

* Create a service with a public IP to expose you web app
* Exposes the Service externally using an external load balancer. Try to find the right service type to use in the [documentation](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)  

{% collapsible %}

Create a `service.yaml` file with the following contents:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-helloworld-svc
spec:
  type: LoadBalancer
  selector:
    app: aks-helloworld
  ports:
    - port: 80 # SERVICE exposed port
      name: http # SERVICE port name
      protocol: TCP # The protocol the SERVICE will listen to
      targetPort: 5000 # Port to forward to in the POD    
```

Deploy the service:

```sh
kubectl apply -f ./service.yaml
```

Then ensure it was successful:

```sh
kubectl get svc
```

You should see an output similar to:

```sh
NAME                 TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)        AGE
aks-helloworld-svc   LoadBalancer   10.0.203.158   4.207.207.53   80:31493/TCP   13s
```

You can now load the URL `http://<EXTERNAL-IP>` on your browser and ensure it returns `Hello, this is my first AKS deployment`.

{% endcollapsible %}

> **Resources**
>
> * <https://kubernetes.io/docs/concepts/services-networking/>
