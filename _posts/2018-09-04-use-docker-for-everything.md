---
layout: post
title: Use Docker for everything, seriously
category: Technical Notes
---

While my favorite technology pick of 2017 has been [Spacemacs](http://spacemacs.org/), which then later led me to discovering Emacs on my own, and 2018 has gone by under the sign of [NixOS](https://nixos.org/), the purely functional Linux distribution, I must admit both of these things weren't the biggest game changers for me.

[Docker](https://www.docker.com/) has been the one.

## Use it everywhere

I no longer install programming languages on my machine. When some friend asks me how I made X programming language or Y development environment to work on NixOS, I just shrug – because I haven't. It's in the Docker.

Am I trying out a new language, like, let's say, Clojure? I just need to run a set of koans... no worries, that shouldn't be too hard:

~~~ bash
docker run --rm -it -v $(pwd):/app -w /app clojure lein koan run
~~~

And we're done.

Or maybe you want to run your [LambdaCD](https://www.lambda.cd/), which is a nice CI/CD pipeline as a code written in Clojure, or any other CI/CD for that matter, to run in a Docker container so you can deploy it easily with Kubernatees?

Sure, you can do that.

And then you can still build and test your dockerized app as a container inside your dockerized CI/CD pipeline... well, actually _next to it_ as a sibling would be a much better explanation [as redirection of the Docker socket](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/) will do just that.

## One Makefile to rule them all

In each of my projects I want to have a simple Makefile. No matter the language or framework, I'd like to have the similar interface everywhere. Here are main points:

* `make start` should start a container specified in a local `docker-compose.yml` with a `-d/--detached` option and thus return to my shell once applications starts to run
* `make stop` will stop the container
* `make test` will execute the test and, preferably, run them in watch mode (like a [guard](https://github.com/guard/guard) gem in Ruby does for example) unless `CI=true` environment variable is set, thus running tests only once on continous integration systems

With this setup I can grab logs using `docker logs -f <container_id>` where I can grab ID from `docker ps` output.

For languages which heavily rely on a REPL, I might consider either exposing yet another port for the `make serve` command and then connecting to it from the host, or running a seperate `make repl` command if required.

Last but not least is the shell, usally accessed with a `make ash` command for Dockerfiles based on [Alpine Linux](https://alpinelinux.org/), though I can see how in certain projects I might be in a need of bigger distros and thus `make bash` would be more convinient.

If you're wondering why that's not `make shell` instead, the answer is simple: I want to be pretty explicit in what shell I will land after firing up the command so I can link my make outputs with other UNIX commands with confidence.

## Ready starter templates

I recently started to write down dockerized starter templates for some languages I use. So far you can grab my [Ruby starter](https://github.com/naliwajek/ruby-starter) and, not yet completed as I have started to play with [Julia](https://julialang.org/) in the meantime, [Clojure starter](https://github.com/naliwajek/clojure-starter) template.

Happy dockerizing.
