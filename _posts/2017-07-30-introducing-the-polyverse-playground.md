---
layout: post
title: "Introducing the Polyverse Playground"
description: "Visit http://play.polyverse.io and go wild!"
date: 2017-07-30 21:00:50 +0000
original_url: https://medium.com/@archisgore/introducing-the-polyverse-playground-d9d7ff84e093
---

Visit <http://play.polyverse.io> and go wild!

For some time now, I’ve been meaning to write various posts about what Polyverse does, how it works, how it integrates into your machines/platforms/processes/etc., and most important of all, how it scales and functions.

We are always a bit behind on documenting all the cool products we keep coming up with. While that is now being remedied quickly, and we have plenty of materials to read, the challenge has always been the lack of being able to touch, feel, use and test the products you keep hearing about.

Well, wait no more. Let’s go through the Polyverse Playground.

*NOTE: The playground itself is not secure, or particularly resilient. It was hacked up from play-with-docker (see Appendix I.) Do not put credentials or really anything very useful there. Remember the Polyverse philosophy: Assume the hack already exists!*

### Using the playground

Skip this entire section if you’re familiar with [Play With Docker](http://play-with-docker.com).

#### Sessions

Let’s explore what the playground offers. When you hit <http://play.polyverse.io>, you should first be dropped into a *session*.

It would look something like this:

![](/assets/images/introducing-the-polyverse-playground/1_nVG347SzLN8wrN07HMeCeQ.png)

Session

The cool part about this session, is that anyone with a link to this session, can see the same instances, and can interactively type into them.

So you can easily start something up, and share it with friends.

#### Instances

Once you have a session, let’s add an instance, by clicking the quite conspicuous area that says “ADD NEW INSTANCE”.

![](/assets/images/introducing-the-polyverse-playground/1_26o1hoKs2s4nOpTSBjGK6w.png)

Since Instance

This jumps you into a simple Docker-provisioned container that has some Polyverse images loaded into it (so you don’t need credentials to our subscribed docker registry.)

You may play with this instance as an online in-the-cloud docker instance.

#### Swarms

However, the REAL fun starts when you can run examples or tests on a Swarm of Docker Engines. This is how you can test whether your solution/stack scales correctly across nodes.

To get a Swarm, you have to use templates. Begin by clicking on that Wrench Icon on the control panel:

![](/assets/images/introducing-the-polyverse-playground/1_Afnn-tD9WAs3uuxfbbv5gA.png)

Swarm Templates

You’ll see a list of templates:

![](/assets/images/introducing-the-polyverse-playground/1_BRNgGrMzEK7k_lXEtLDttQ.png)

You’ll notice that there are plenty of available templates with five Docker Engines each. You can get between 1, 3 and 5 managers, with the remaining instances being Workers.

The very first template is special in that it gives you five managers, but three are on a scrambled instance, while two are unscrambled. This configuration is intended to test the benefits and limitations of scrambling (more on this coming in future blogs…)

#### Accessing exposed services

One of my absolute favorite features of Play With Docker, which is also in Play with Polyverse, is the ability to access services exposed from any instance, available over the web.

Let’s check it out. On any instance you have, run `/polyverse/hello-world/hello-world-container`.

![](/assets/images/introducing-the-polyverse-playground/1_h3g34__7K4JERbGZ9pLhQg.png)

See those two ports up there? 80 and 8080? They’re services exposed from this node. Since the hello-world-container script binds `tutum/hello-world` to port `80`, we’ll click on that one:

![](/assets/images/introducing-the-polyverse-playground/1_4Gl0poKcXasCb439dEWotA.png)

It will pop up a new tab that hits the hello-world container hosted right in the cloud instance:

![](/assets/images/introducing-the-polyverse-playground/1_ocWzTFrdPGCsDhuQBjfT6Q.png)

You can go back to your host instance, and run `docker ps -a` to verify that the hostname *e03e8a6ed3a8* is indeed the container id of the `tutum/hello-world` container. Of course the name will be different for your own session, depending on the id assigned to your container.

### Polyverse cycled, self-healing Hello World!

Let’s use the playground to run our Hello World program the Polyverse way. We’re going to use my favorite container: [tutum/hello-world](https://hub.docker.com/r/tutum/hello-world/). I love this container because it was built by someone else (and I’m famously lazy), it exposes a single HTTP port, and it doesn’t do very many clever things (so if something goes wrong, we can assume we screwed up.)

#### Start the swarm

Let’s start with a Scrambled Swarm with three managers and two workers:

![](/assets/images/introducing-the-polyverse-playground/1_HBxRCmGbblwNQNkux8Fhxw.png)

3 Managers and 2 Workers

Give it a minute to stabilize….

And then run `docker info` on one of the managers. We should see a five-node swarm with three managers:

![](/assets/images/introducing-the-polyverse-playground/1_It5bAQ8BRwn6OmK2XlSYPA.png)

Swarm active with five nodes, three of them managers

#### Run Hello World, Polyversed!

Go to any of the manager instances, and go to the directory `/polyverse/hello-world`, and run `./hello-world-cycled`

![](/assets/images/introducing-the-polyverse-playground/1_lYpQh85D0WuL0TQNL_27rA.png)

Running hello world, polyversed

You’ll see it start a swarm service… it updates some config files (do feel free to explore the script.)

![](/assets/images/introducing-the-polyverse-playground/1_M_ijJlrRiA8tENFcLjC3sQ.png)

Since Polyverse is built on a stateless, message-passing, microservices, actor-based model (a major influence from Erlang), the first thing that script started was a supervisor. The supervisor will proceed to start the entire Polyverse stack.

I like to run `watch docker service ls` to see the services slowly spin up one by one…

![](/assets/images/introducing-the-polyverse-playground/1_XQ9at8uO6--JryzgRMaVUQ.png)

After a couple of minutes, you’ll notice that a port `8080` has been bound to the host. Let’s click on that port!

You should see a hello-world container show up!

![](/assets/images/introducing-the-polyverse-playground/1_PVKIoRBxm6cVbfrDFIU_9Q.png)

So what’s the big deal here? For one, if you keep refreshing the page, you should see the hostname of the container change.

But to dig further, let’s go back to the service list and see what’s going on….

![](/assets/images/introducing-the-polyverse-playground/1_102edvpqf2_hBpw7eOqmRg.png)

Things are quite active on this side! You’ll notice two new services just launched that contain hello-world. And if you continue to watch this view, in about a minute, you’ll notice two new services taking the place of the old ones.

You’ve just cycled your very first hello-world container! I’ll write a blog in the coming week on how all this works, why those containers started, why they have two instances, and why they rotate every minute — and what you can do to change all that!

### Appendices

#### Appendix I: Source Code for the forked Play-With-Docker

Play with Polyverse is a fork of Play-with-Docker. It was created at nights and between talks/meetings during my stay at BlackHat, so it’s not the most beautifully written code there is.

The source code is available at: <https://github.com/polyverse-security/play-with-polyverse>

Major changes I made:

1. Containerized the PWD code (which in the compose file, was compiling go code life.) This was painful because I didn’t want my target machine to necessarily have golang.
2. Vendored dependencies using glide. This was necessary for my sanity. Using go 1.8, you should be able to do “go build api.go” to get a binary without worrying about the state of $GOPATH.
3. Added some CPU/resource limitations around each container.
4. Updated the Google Analytics code.

Things I’d like to do:

1. I’d love for this to work over a swarm itself and using **docker stack**, so I can scale it out as demand increases.
