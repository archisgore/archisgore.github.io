---
layout: post
title: "Moving Target Defense on Kubernetes"
description: "Moving Target Defense"
date: 2017-08-22 21:20:01 +0000
original_url: https://medium.com/@archisgore/moving-target-defense-on-kubernetes-ccac100940e9
---

**Moving Target Defense**

Moving Target Defense is a long theoretical approach to cybersecurity, predicated on the assumption that everything static is eventually hackable — machines, networks, firewalls, kernels, databases, you name it. Furthermore, the similarity of many of today’s static systems architectures makes them susceptible to “break once, exploit everywhere” attacks. If hackers find an exploitable security flaw, the malware they create to exploit it can then compromise any system running the same software, across huge numbers of computers or devices worldwide.

To counter this, Moving Target Defense employs a strategy of constant movement — an approach that can be applied to machines, containers, binary instructions, ports, networks, keys, passwords, and so on. The more things you can move, the more costly it is for an attacker to exploit the target (because the attack must be repeated after every move). Furthermore, if there are replicas, and every replica is moving independently, then the cost of gaining access to all replicas becomes near-infinite.

**Polyverse on Kubernetes**

The container revolution, along with a powerful, flexible and mature platform like Kubernetes, has opened up new scenarios that were previously difficult to realize. Kubernetes abstracts out the underlying implementation details of containers, networks and entire clusters.

Polyverse is a suite of tools that runs on top of Kubernetes and provides a higher-level abstraction layer for dynamic and intelligent goal-based management of Pods. Polyverse fundamentally changes the traditional deployment strategy that involves launching the containers first, and then exposing them through a routing infrastructure (such as a WAF, Reverse Proxy, ingress, etc). Instead, it operates the other way around.

For every request, Polyverse queries an Application Definition file that you provide, to ask it what “goal” this request must route to (today the goal is a single Pod definition), and in response to this, rapidly launches the necessary Pods to handle it along with automatically generating tight scaffolding around it — creating volumes and tight network policies and tracking connection traffic, etc. It also monitors the Pods for health, age and consistency. Additionally, Polyverse’s own components have a “maximum time to live,” so Polyverse Polyverses itself periodically.

But wait, there’s more. In the unlikely event that there’s a missed buffer overrun that might allow an attacker access to potential ROP attacks, Polyverse uses a different binary scramble every time it runs. We also ship pre-scrambled binaries for our customers to use, and scrambling compilers for popular languages, which enable our customers to compile their own.

**Detect and Respond vs Mitigate**

Polyverse is fundamentally different from traditional security methodologies in that, due to the company’s underlying assumption that “everything is hackable,” Polyverse seeks to mitigate the impact of a hack as if it were already happening, rather than waiting for when it happens. This is why the dynamic partitioning, self-healing, rapid cycling, and binary scrambling capabilities are applied proactively. They seek to mitigate the impact of a hack before it even happens. And because Polyverse doesn’t look for attacks, we reduce any blast radius even when the signatures of attacks are unknown and systems are unpatched.

**Goal-oriented system**

A goal-seeking system like Polyverse enables you to define highly partitioned and varied goals — potentially sending different requests, users, sessions, etc. to different Pods — and leaving potential attackers always trying to guess your system’s fingerprint. In an extreme case, in a stateful system that can be partitioned, Polyverse can launch an individual database Pod per user — with its own network policies, scrambled binaries (that change on every run), encryption keys, storage locations, and more.

While all this doesn’t make anything absolutely “hack-proof,” it greatly decreases the chances that an attacker gets “everything.” Every instance would incur the same cost to reverse-engineer and break. As your users grow, so does the cost of stealing “everything” in tandem.

You tell us what you want, and we ensure that it is done right. It flips the premise of “auditing” on its head. Rather than “deploy first, audit-later,” Polyverse encourages a model of “deploy what you want” — and let us clean up everything automatically.

**Polyverse in Action**

Let’s look at how this works in practice. For the purposes of this post, we’re going to pick a single stateless web service — let’s take the simplest “[tutum/hello-world](https://hub.docker.com/r/tutum/hello-world/)” off the Docker Hub.

We’ll also assume that you are provisioned with Polyverse repository credentials, and have access to our suite of images — and that these credentials are set as a secret in your Kube Cluster with the name “*polyverse-credentials*.”

From here on, deploying a Polyversed service is simple. Step 1, we create a configuration file (not very different from how you’d create Kubernetes configs).

```
polyverse.config:  
  router.port: "30080"
```

```
  kubernetes.polyverse_image_pull_secret: "polyverse-credentials"
```

```
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
        "address": "runtime.hub.polyverse.io/supervisor:75fcd0ea2136ddcf3987b8623f8bee3e580e52d3"  
      },  
      "router": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/router:78cd0fc78e900d848ecdf2ff16d358925efa3499"  
      },  
      "containerManager": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/container-manager:8545877bcf871102a1a31d4b2e784a5dd7167680"  
      },  
      "eventService": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/event-service:f270e2e1f2b88a9ac478e1df6df89947680b95e5"  
      },  
      "api": {  
        "type": "dockerImage",  
        "address": "runtime.hub.polyverse.io/api:71c7f7da51243353ffd3eae99995f33ad0600550"  
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
    
      var routeInfo = {  
        RouteType: 5,  
        ID:                 "default_route",  
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
                  ImagePullSecrets: [  
                      {  
                          Name: "polyverse-credentials"  
                      }  
                  ]  
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
      return routeInfo;  
    }  
  };  
  }();
```

You’ll notice that the YAML file is very simple. It has four primary things in it:

1. The router port (the port on which Polyverse exposes the service from the cluster) is set to a value of 30080. This is because the default Polyverse setting is 8080, and some Kube Clusters don’t allow binding to that port.
2. Next we set the name of the secret that Polyverse should use to pull its own components.
3. Third is a rather large JSON blob that locks down the versions of components to use (and are known to work together). We provide that blob periodically as newer builds pass our tests. (More details on what a VFI is, coming soon in a future blog.)
4. Finally we come to the part that makes Polyverse Polyverse, the Application Definition file. You’ll note that it’s a simple JavaScript file that has two key things:
5. A variable called “default\_goal” that contains the target goal Pod a request might be routed to.
6. A function called Route that returns this goal asymptotically (meaning all requests go to this singular goal).

Let’s put this file on the cluster as a secret, so we can pass it down to the supervisor:

```
kubectl create secret generic polyverse-yaml --from-file=./polyverse.yaml
```

Wee need ONE more file — the file that launches the main Replication Controller that will launch/manage the rest of Polyverse:

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
        image: runtime.hub.polyverse.io/supervisor:75fcd0ea2136ddcf3987b8623f8bee3e580e52d3  
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
```

For Kubernetes natives, this is a trivial Replication Controller that does two things: launches our “supervisor” and provides it with a secret called polyverse-yaml (which contains the settings in the yaml above).

Let’s launch this Replication Controller:

```
kubectl create -f polyverse.replication-controller.yaml
```

And that’s it. Once the supervisor is started, it will launch everything else. I usually like to keep a command window running `watch` so that I can observe services spin up:

```
Every 2.0s: kubectl get all  
NAME READY STATUS RESTARTS AGE
```

```
po/kubernetes-supervisor-l85h6 1/1 Running 0 1m
```

```
po/polyverse-etcd-1 1/1 Running 0 1m
```

```
po/polyverse-kubernetes-pod-manager-7w542 1/1 Running 0 1m
```

```
po/polyverse-nsq-1-d4kvm 1/1 Running 0 1m
```

```
po/polyverse-router-2x3dn 1/1 Running 0 1m
```

```
NAME DESIRED CURRENT READY AGE
```

```
rc/kubernetes-supervisor 1 1 1 1m
```

```
rc/polyverse-kubernetes-pod-manager 1 1 1 1m
```

```
rc/polyverse-nsq-1 1 1 1 1m
```

```
rc/polyverse-router 1 1 1 1m
```

```
NAME CLUSTER-IP EXTERNAL-IP PORT(S) AGE
```

```
svc/kubernetes 10.3.240.1 <none> 443/TCP 1m
```

```
svc/polyverse-etcd-1 10.3.244.248 <none> 2379/TCP,2380/TCP 1m
```

```
svc/polyverse-nsq-1 10.3.250.134 <none> 4150/TCP 1m
```

```
svc/polyverse-router 10.3.247.44 130.211.233.140 30080:30080/TCP 1m
```

We wait until the GCE gives the router service a public-facing IP (130.211.187.109). Let’s see what happens when we try to ping the cluster on port 30080:

```
curl 130.211.233.140:30080
```

```
<html>
```

```
<head>
```

```
<title>Hello world!</title>
```

```
<link href=’http://fonts.googleapis.com/css?family=Open+Sans:400,700' rel=’stylesheet’ type=’text/css’>
```

```
<style>
```

```
body {
```

```
background-color: white;
```

```
text-align: center;
```

```
padding: 50px;
```

```
font-family: “Open Sans”,”Helvetica Neue”,Helvetica,Arial,sans-serif;
```

```
}
```

```
#logo {
```

```
margin-bottom: 40px;
```

```
}
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
<img id=”logo” src=”logo.png” />
```

```
<h1>Hello world!</h1>
```

```
<h3>My hostname is blog-example — kube-route-default — 13093339989763551321</h3> <h3>Links found</h3>
```

```
<b>POLYVERSE_NSQ_1</b> listening in 4150 available at tcp://10.3.250.134:4150<br />
```

```
<b>POLYVERSE_ETCD_1</b> listening in 2380 available at tcp://10.3.244.248:2380<br />
```

```
<b>POLYVERSE_ETCD_1</b> listening in 2379 available at tcp://10.3.244.248:2379<br />
```

```
<b>KUBERNETES</b> listening in 443 available at tcp://10.3.240.1:443<br />
```

```
<b>POLYVERSE_ROUTER</b> listening in 30080 available at tcp://10.3.247.44:30080<br />
```

```
</body>
```

```
</html>
```

That seems about right: this is what `tutum/hello-world` returns. If we go back to our watcher, you’ll notice a large number of Pods has started, and as they cross the 30-second lifetime mark, they are quickly replaced by new instances to take their place:

```
NAME READY STATUS RESTARTS AGE  
po/blog-example — kube-route-default — 00896848433713446282 1/1 Running 0 2m
```

```
po/blog-example — kube-route-default — 01226209926893756499 1/1 Running 0 2m
```

```
po/blog-example — kube-route-default — 02158913934322092479 1/1 Running 0 11s
```

```
po/blog-example — kube-route-default — 02207642866019253657 0/1 Pending 0 21s
```

```
po/blog-example — kube-route-default — 02559157942065164791 1/1 Running 0 10s
```

```
po/blog-example — kube-route-default — 05695006230249559288 1/1 Running 0 56s
```

```
po/blog-example — kube-route-default — 07247819473528208499 1/1 Running 0 1m
```

```
po/blog-example — kube-route-default — 09060590674581895311 0/1 Pending 0 4s
```

```
po/blog-example — kube-route-default — 09511575105227313598 0/1 Pending 0 1s
```

```
po/blog-example — kube-route-default — 09764087338786205693 1/1 Running 0 2m
```

```
po/blog-example — kube-route-default — 11964680743026652223 0/1 Pending 0 7s
```

```
po/blog-example — kube-route-default — 12185658296840170903 1/1 Running 0 1m
```

```
po/blog-example — kube-route-default — 13018583032032941773 1/1 Running 0 1m
```

```
po/blog-example — kube-route-default — 13800303621935396225 1/1 Running 0 1m
```

```
po/blog-example — kube-route-default — 15192960994861293060 1/1 Running 0 22s
```

```
po/blog-example — kube-route-default — 15196143337265972567 1/1 Running 0 2m
```

```
po/blog-example — kube-route-default — 17710472477436212743 1/1 Running 0 1m
```

```
po/kubernetes-supervisor-nq54f 1/1 Running 0 4m
```

```
po/polyverse-etcd-1 1/1 Running 0 4m
```

```
po/polyverse-kubernetes-pod-manager-rrndj 1/1 Running 0 4m
```

```
po/polyverse-nsq-1–5s101 1/1 Running 0 4m
```

```
po/polyverse-router-dqw08 1/1 Running 0 4m
```

**What more can I do?**

You’ll notice all the way back in the AppDefinition, the function “Route” has two parameters, “r” and “c.” The parameter “r” is a golang [http.Request](https://golang.org/pkg/net/http/#Request) object for the incoming request.

```
Route: function(r,c) {  
  return default_goal;  
}
```

Given that each request flows through your Application Definition, and you have the power of JavaScript to dynamically generate goals per request, you can create highly entropized systems with very few lines of code.

An example might be to pull your session header/cookie out of the request, and generate a goal for that session. This one rule will enable Polyverse to launch a new Pod for every session that comes into your service, apply tight network policies, and even react dynamically to a changing landscape — such as throttling requests with very specific selectors, or sending them to different clusters, or even creating dynamic honeypots. The last one is my personal favorite…

The possibilities are endless.
