---
sectionid: networkpolicy
sectionclass: h2
title: Network Policy (10m)
parent-id: upandrunning
---

Now that your application is exposed, you are going to test how network policies work.

#### Block all traffic

A general good practice with network security is to block all traffic by default (in both ways) and then whitelist the allowed traffic.
Create a [NetworkPolicy](https://kubernetes.io/docs/concepts/services-networking/network-policies/) applied to all pod in your namespace. Block ALL traffic in ingress and egress. 

Once deployed, check if you can still access your application.

{% collapsible %}

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: web-deny-all
spec:
  podSelector: {}
  ingress: []
  egress: []
```

{% endcollapsible %}

> warning: it is possible that it does not work. it's because when you created the cluster, you didn't enable the network plugin and thus, all network policies are disabled by default. thus, you must **recreate** the cluster from scratch, and use the parameter --network-plugin

#### Enable again the traffic from Internet

Now, without removing the DenyAll policy, add a second policy, specically applied to your pod to allow traffic coming from Internet.
Once again, deploy it and check that you can now access to your application.

{% collapsible %}

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: web-all-from-internet
spec:
  podSelector: 
    matchLabels:
      app: aks-helloworld
  ingress: 
  - {}
```

{% endcollapsible %}

Easy, isn't it ?
