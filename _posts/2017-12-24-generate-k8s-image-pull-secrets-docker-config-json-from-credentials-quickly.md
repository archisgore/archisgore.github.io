---
layout: post
title: "Convert between Docker Registry Credentials, K8s Image Pull Secrets, and config.json live"
description: "Generating registry secrets for Kubernetes is cumbersome. Extracting creds or updating the secret is annoying. Generating config.json is…"
date: 2017-12-24 02:51:45 +0000
original_url: https://medium.com/@archisgore/generate-k8s-image-pull-secrets-docker-config-json-from-credentials-quickly-a27e05812ea5
---

<https://polyverse.github.io/docker-creds-converter/>

![](/assets/images/generate-k8s-image-pull-secrets-docker-config-json-from-credentials-quickly/1_B3zfQh4ohkuStXp0HragbQ.png)

I frequently generate service accounts in our private image hub for various tests. Generating config.json is cumbersome. Injecting that into a kubernetes cluster as a registry secret (for use as a ImagePullSecrets on a Pod) is hard. Single-character errors can lead to a cryptic ImagePullErr. On a swarm, it’s worse. The tasks just won’t spin up mysteriously.

It is even more painful when working with customers, and you are behind an email-wall at worst, or a slack-wall at best. Both terrible for preserving formatting.

So I whipped up this tri-directional live converter. You can add/edit credentials on the left-most side, and a config.json as well as Kubernetes Secret is generated live (which you can save to a file, and inject using:

`kubectl create -f <savedfile.yaml>`

However, you may also post a secret you obtained out of Kubernetes by running:

`kubectl get secret <secret> --output=yaml`

Or you can edit config.json directly. This is incredibly useful when you have a secret that contains authorizations to say 10 registries, but you want to revoke 3 of those.

You can remove those registry entries out of config.json, and the kubernetes yaml will be updated automatically to reflect that.

Spread the word!
