---
layout: post
title: "Moving Target Defense on Hybrid Kubernetes: Windows and Linux"
description: "Self-healing Windows and Linux containers on the same cluster"
date: 2017-12-07 21:26:32 +0000
original_url: https://medium.com/@archisgore/moving-target-defense-on-hybrid-kubernetes-windows-and-linux-f78386647a36
---

This post is a very brief followup to my original post about [Moving Target Defense on Kubernetes](https://blog.polyverse.io/moving-target-defense-on-kubernetes-ccac100940e9).

This post is an addendum to demonstrate the same cycler/goal-seeker working on a hybrid Linux/Windows cluster so that the Polyverse control plane can manage Windows-based goals.

#### Prerequisites:

Before you begin, make sure you have three things ready:

1. Have a Kubernetes Cluster configured correctly with at least one Windows node, and one Linux node. `kubectl` must already be working.
2. Ensure that your Cluster has an ImagePullSecrets named “polyverse-credentials” that gives you access to pull our images.
3. Ensure that you have a Windows Server Container (WSC) based Pod that already works correctly. See **Appendix II** for why this is important. With WSC being new, there’s a lot that can go wrong.

#### Create the configuration

```
polyverse.config:  
  router:  
    port: "30080"  
    ingress: "true"  
  kubernetes.polyverse_image_pull_secret: "polyverse-credentials"  
  vfi: |  
    {  
      "etcd": {  
        "type": "dockerImage",  
        "address": "gcr.io/etcd-development/etcd:v3.2.8"  
      },  
      "nsq": {  
        "type": "dockerImage",  
        "address": "nsqio/nsq:v1.0.0-compat"  
      },  
      "supervisor": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/supervisor:5bb4a0d6b0da57df4dbcb98335b2ee295bbf62bf"  
      },  
      "router": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/router:22d8773b0b387e218a42485a88fbe97d9f1b5777"  
      },  
      "containerManager": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/container-manager:be1efa121c8f0f4b05257f1bc4cd33df4d357c5a"  
      },  
      "eventService": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/event-service:d4b09c536625d6c0872ff8541201c96306f2bd69"  
      },  
      "api": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/api:c97bc6ed0b438ba282d3d343fbd1086c38cc54ff"  
      },  
      "status": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/status:b0cb48533fcc10f114f0ab1ba48274a1c37f9ba3"  
      },  
      "polyverse": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/polyverse:69871a6a8725b186e3d326264dd8484c39fcef64"  
      }  
    }
```

```
apps/example: |  
  app = function() {  
    
      var windowsRouteInfo = {  
        RouteType: 5,  
        ID:                 "windows_route",  
        Timeout:            365 * 24 * 60 * 60 * 1000000000,  
        ConnectionDrainGracePeriod: 3 * 60 * 1000000000,  
        PerInstanceTimeout: 20 * 60 * 1000000000,  
        DesiredInstances:   2,  
        KubePod: {  
          HealthCheckURLPath: "/",  
          LaunchGracePeriod: 10 * 60 * 1000000000,  
          BindingPort: 80,  
          Pod: {  
              Spec: {  
                  Containers: [  
                      {  
                          Name: "hello-world",  
                          Image: "microsoft/iis:windowsservercore-1709"  
                      }  
                  ],  
                  NodeSelector: {  
                      "beta.kubernetes.io/os": "windows"  
                  }  
              }  
          }  
        }  
      };  
    
      var linuxRouteInfo = {  
        RouteType: 5,  
        ID:                 "linux_route",  
        Timeout:            365 * 24 * 60 * 60 * 1000000000,  
        ConnectionDrainGracePeriod: 3 * 60 * 1000000000,  
        PerInstanceTimeout: 300 * 1000000000,  
        DesiredInstances:   2,  
        KubePod: {  
          HealthCheckURLPath: "/",  
          LaunchGracePeriod: 5 * 60 * 1000000000,  
          BindingPort: 80,  
          Pod: {  
              Spec: {  
                  Containers: [  
                      {  
                          Name: "hello-world",  
                          Image: "tutum/hello-world"  
                      }  
                  ],  
                  NodeSelector: {  
                      "beta.kubernetes.io/os": "linux"  
                  }  
              }  
          }  
        }  
      };  
    
  return {  
    Name: function() {  
      return "default_appdef";  
    },  
    IsRequestSupported: function(r,c) {  
      return true;  
    },  
    Route: function(r,c) {  
      if (r.URL.Path == "/linux") {  
          return linuxRouteInfo;  
      }  
      return windowsRouteInfo;  
    }  
  };  
  }();
```

Two things of note that are different from the previous blog post:

1. The VFI is obviously updated (it’s been over six months and that’s a lot of updates.)
2. Most importantly, you’ll notice the power of the Application Definition. We have a routing rule based on the request path. If the path is “/linux”, those requests go to the linux `tutum/hello-world`. All other requests go to Windows. (IIS tends to be fussy when not hit at root. The hello-world though is perfectly fine with whatever path we throw at it.)

There are a few other minor differences such as WSC rotating every 20 minutes and requiring a larger launch grace time, it being a three-gig beast.

Let’s inject this configuration into the cluster as a secret (I just haven’t gotten around to config maps yet.)

```
kubectl create secret generic polyverse-yaml --from-file=./polyverse.yaml
```

#### Create the Supervisor Replication Controller

```
apiVersion: v1  
kind: ReplicationController  
metadata:  
  name: supervisor  
spec:  
  replicas: 1  
  selector:  
    io.polyverse.supervisor: manual-supervisor  
  template:  
    metadata:  
      name: supervisor  
      labels:  
        io.polyverse.supervisor: manual-supervisor  
    spec:  
      containers:  
        - name: supervisor  
          image: runtime.hub.polyverse.io/supervisor:dd8e11e9aec501fcb8cf3a994e74ed8438b36021  
          args:  
            - --config-yaml-file  
            - /run/secrets/polyverse.yaml  
          volumeMounts:  
            - name: polyverse-yaml  
              mountPath: /run/secrets  
              readOnly: true  
      imagePullSecrets:  
        - name: polyverse-credentials  
      volumes:  
        - name: polyverse-yaml  
          secret:  
            secretName: polyverse-yaml  
      nodeSelector:  
        beta.kubernetes.io/os: linux
```

Again, you’ll notice two minor differences:

1. The supervisor’s tag is updated to reflect the tag from the latest VFI.
2. The supervisor has a node-selector for Linux.

Let’s launch this supervisor:

```
kubectl create -f polyverse.replication-controller.yaml
```

#### Wait for Polyverse to come up

I usually run `watch kubectl get all` to track what’s going on. Eventually a few things will be running. You need to make sure the container manager and router and running:

```
Every 2.0s: kubectl get all                                                                                                                      Archishmats-MacBook-Pro-2.local: Thu Dec  7 12:40:19 2017
```

```
NAME                                   READY     STATUS    RESTARTS   AGE  
po/polyverse-container-manager-vqwss   1/1       Running   0          15s  
po/polyverse-etcd-1                    1/1       Running   0          37s  
po/polyverse-nsq-1-6lsqt               1/1       Running   0          23s  
po/polyverse-router-kcl3k              1/1       Running   0          19s  
po/supervisor-94dxd                    1/1       Running   0          52s
```

```
NAME                             DESIRED   CURRENT   READY     AGE  
rc/polyverse-container-manager   1         1         1         15s  
rc/polyverse-nsq-1               1         1         1         23s  
rc/polyverse-router              1         1         1         19s  
rc/supervisor                    1         1         1         52s
```

Next up, wait for the router to have an External IP:

```
Every 2.0s: kubectl get services                                                                                                                 Archishmats-MacBook-Pro-2.local: Thu Dec  7 12:47:22 2017
```

```
NAME               TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)             AGE  
kubernetes         ClusterIP      10.0.0.1       <none>          443/TCP             14h  
polyverse-etcd-1   ClusterIP      10.0.137.77    <none>          2379/TCP,2380/TCP   7m  
polyverse-nsq-1    ClusterIP      10.0.122.135   <none>          4150/TCP            7m  
polyverse-router   LoadBalancer   10.0.190.163   40.71.194.167   30080:30080/TCP     7m
```

#### Hit a Linux container

First up, let’s prime the Linux container. This part is easy:

```
curl http://40.71.194.167:30080/linux
```

You’ll notice that Pods were launched to meet the requested goal:

```
Every 2.0s: kubectl get all                                                                                                                      Archishmats-MacBook-Pro-2.local: Thu Dec  7 12:52:07 2017
```

```
NAME                                                   READY     STATUS    RESTARTS   AGE  
po/default-appdef--linux-route--06696562108612858319   1/1       Running   0          15s  
po/default-appdef--linux-route--16680611848214289005   1/1       Running   0          15s  
po/polyverse-container-manager-vqwss                   1/1       Running   0          12m  
po/polyverse-etcd-1                                    1/1       Running   0          12m  
po/polyverse-nsq-1-6lsqt                               1/1       Running   0          12m  
po/polyverse-router-kcl3k                              1/1       Running   0          12m  
po/supervisor-94dxd                                    1/1       Running   0          12m
```

```
NAME                             DESIRED   CURRENT   READY     AGE  
rc/polyverse-container-manager   1         1         1         12m  
rc/polyverse-nsq-1               1         1         1         12m  
rc/polyverse-router              1         1         1         12m  
rc/supervisor                    1         1         1         12m
```

#### Hit a Windows container

```
curl http://40.71.194.167:30080/
```

The first run will fail with a Polyverse error that will tell you it’s taking too long to launch the goal. (Priming containers is a topic for a deep dive some other time.)

However, you should see the Pods getting launched. You may `kubectl describe` them to see what’s up:

```
Every 2.0s: kubectl get all                                                                                                                      Archishmats-MacBook-Pro-2.local: Thu Dec  7 12:53:52 2017
```

```
NAME                                                     READY     STATUS              RESTARTS   AGE  
po/default-appdef--linux-route--06696562108612858319     1/1       Running             0          2m  
po/default-appdef--linux-route--16680611848214289005     1/1       Running             0          2m  
po/default-appdef--windows-route--05094996203598508593   0/1       ContainerCreating   0          6s  
po/default-appdef--windows-route--17292551557378544927   0/1       ContainerCreating   0          6s  
po/polyverse-container-manager-vqwss                     1/1       Running             0          13m  
po/polyverse-etcd-1                                      1/1       Running             0          14m  
po/polyverse-nsq-1-6lsqt                                 1/1       Running             0          13m  
po/polyverse-router-kcl3k                                1/1       Running             0          13m  
po/supervisor-94dxd                                      1/1       Running             0          14m
```

```
NAME                             DESIRED   CURRENT   READY     AGE  
rc/polyverse-container-manager   1         1         1         13m  
rc/polyverse-nsq-1               1         1         1         13m  
rc/polyverse-router              1         1         1         13m  
rc/supervisor                    1         1         1         14m
```

Eventually, you’ll find that the Windows routes are up and running, and hitting the port will return this:

```
curl http://40.71.194.167:30080/
```

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

```
<html xmlns="http://www.w3.org/1999/xhtml">
```

```
<head>
```

```
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
```

```
<title>IIS Windows Server</title>
```

```
<style type="text/css">
```

```
<!--
```

```
body {
```

```
color:#000000;
```

```
background-color:#0072C6;
```

```
margin:0;
```

```
}
```

```
#container {
```

```
margin-left:auto;
```

```
margin-right:auto;
```

```
text-align:center;
```

```
}
```

```
a img {
```

```
border:none;
```

```
}
```

```
-->
```

```
</style>
```

```
</head>
```

```
<body>
```

```
<div id="container">
```

```
<a href="http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409"><img src="iisstart.png" alt="IIS" width="960" height="600" /></a>
```

```
</div>
```

```
</body>
```

Success! That is the default IIS text!

#### Appendix I: Launching a [Hybrid Kubernetes Cluster](https://medium.com/@maxsparr0w/hybrid-kubernetes-in-azure-9c0c8269aca8)

A wonderful fellow, [Maxim Vorobjov](https://medium.com/@maxsparr0w), [provided a perfect recipe](https://medium.com/@maxsparr0w/hybrid-kubernetes-in-azure-9c0c8269aca8) that worked for me the first time. I am beyond impressed.

#### Appendix II: Building the right Windows Server Container Image

At least for the setup used above (the 1709 Fall creators update), only images built on the base windowsservercore-1709 work.

Over time, I’ll build a reference hello world here: <https://github.com/polyverse/iis-hello-world>

I’m still too new to this to have successfully built a Windows-based image yet. It is crucial that you first test out your Pods work on the target cluster, **before** you attempt to Polyverse it.
