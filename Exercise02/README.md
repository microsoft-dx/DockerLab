[Back to Menu](../README.md)

# Exercise 2 - Docker Images

Docker images are the basis of containers. Each time you've used ``docker run`` you told it which image you wanted. In the previous section of the guide you used Docker images that already exist, for example the ubuntu image.

You also discovered that Docker stores downloaded images on the Docker host. If an image isn't already present on the host then it'll be downloaded from a registry: by default the [Docker Hub](https://hub.docker.com/) Registry.

In this section you're going to explore Docker images a bit more including:

* Managing and working with images locally on your Docker host.

* Creating basic images.

* Uploading images to Docker Hub Registry.


## Listing images on the host

Let's start with listing the images you have locally on our host. You can do this using the docker images command like so:

```bash
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              14.04               1d073211c498        3 days ago          187.9 MB
```


You can see the images you've previously used in the user guide. Each has been downloaded from Docker Hub when you launched a container using that image. When you list images, you get three crucial pieces of information in the listing.

* What repository they came from, for example ``ubuntu``.

* The tags for each image, for example 14.04.

* The image ID of each image.


> Tip: You can use a [third-party dockviz tool](https://github.com/justone/dockviz) or the [Image layers site](https://imagelayers.io/) to display
 visualizations of image data.

A repository potentially holds multiple variants of an image. In the case of our ``ubuntu`` image you can see multiple variants covering Ubuntu 10.04, 12.04, 12.10, 13.04, 13.10 and 14.04. Each variant is identified by a tag and you can refer to a tagged image like so:
```bash
ubuntu:14.04
```

So when you run a container you refer to a tagged image like so:
```bash
$ docker run -t -i ubuntu:14.04 /bin/bash
```

If instead you wanted to run an Ubuntu 12.04 image you'd use:
```bash
$ docker run -t -i ubuntu:12.04 /bin/bash
```

If you don't specify a variant, for example you just use ``ubuntu``, then Docker will default to using the ``ubuntu:latest`` image.


> Tip: You should always specify an image tag, for example ``ubuntu:14.04``. That way, you always know exactly what variant of an image you are using. This is useful for troubleshooting and debugging.

## Getting a new image

So how do you get new images? Well Docker will automatically download any image you use that isn't already present on the Docker host. But this can potentially add some time to the launch of a container. If you want to pre-load an image you can download it using the docker pull command. Suppose you'd like to download the centos image.

```bash
$ docker pull centos
Pulling repository centos
b7de3133ff98: Pulling dependent layers
5cc9e91966f7: Pulling fs layer
511136ea3c5a: Download complete
ef52fb1fe610: Download complete
. . .

Status: Downloaded newer image for centos
```

You can see that each layer of the image has been pulled down and now you can run a container from this image and you won't have to wait to download the image.

```bash
$ docker run -t -i centos /bin/bash
bash-4.1#
```

## Finding images

One of the features of Docker is that a lot of people have created Docker images for a variety of purposes. Many of these have been uploaded to [Docker Hub](https://hub.docker.com/). You can search these images on the [Docker Hub](https://hub.docker.com/) website.

![](images/search.png)

You can also search for images on the command line using the ``docker search`` command. Suppose your team wants an image with Ruby and Sinatra installed on which to do our web application development. You can search for a suitable image by using the ``docker search`` command to find all the images that contain the term ``sinatra``.

```bash
$ docker search sinatra
NAME                                   DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
training/sinatra                       Sinatra training image                          0                    [OK]
marceldegraaf/sinatra                  Sinatra test app                                0
mattwarren/docker-sinatra-demo                                                         0                    [OK]
luisbebop/docker-sinatra-hello-world                                                   0                    [OK]
bmorearty/handson-sinatra              handson-ruby + Sinatra for Hands on with D...   0
subwiz/sinatra                                                                         0
bmorearty/sinatra                                                                      0
. . .
```

You've reviewed the images available to use and you decided to use the ``training/sinatra`` image. So far you've seen two types of images repositories, images like ubuntu, which are called base or root images. These base images are provided by Docker Inc and are built, validated and supported. These can be identified by their single word names.

You've also seen user images, for example the ``training/sinatra`` image you've chosen. A user image belongs to a member of the Docker community and is built and maintained by them. You can identify user images as they are always prefixed with the user name, here training, of the user that created them.

## Pulling our image

You've identified a suitable image, ``training/sinatra``, and now you can download it using the docker pull command.
```bash
$ docker pull training/sinatra
```

The team can now use this image by running their own containers.
```bash
$ docker run -t -i training/sinatra /bin/bash
root@a8cb6ce02d85:/#
```

## Creating our own images

The team has found the ``training/sinatra`` image pretty useful but it's not quite what they need and you need to make some changes to it. There are two ways you can update and create images.

1. You can update a container created from an image and commit the results to an image.

2. You can use a ``Dockerfile`` to specify instructions to create an image.

## Updating and committing an image

To update an image you first need to create a container from the image you'd like to update.
```bash
$ docker run -t -i training/sinatra /bin/bash
root@0b2616b0e5a8:/#
```


> Note: Take note of the container ID that has been created, ``0b2616b0e5a8``, as you'll need it in a moment.

Inside our running container let's add the ``json`` gem.
```bash
root@0b2616b0e5a8:/# gem install json
```

Once this has completed let's exit our container using the ``exit`` command.

Now you have a container with the change you want to make. You can then commit a copy of this container to an image using the docker commit command.
```bash
$ docker commit -m "Added json gem" -a "Kate Smith" \
0b2616b0e5a8 ouruser/sinatra:v2
4f177bd27a9ff0f6dc2a830403925b5360bfe0b93d476f7fc3231110e7f71b1c
```

Here you've used the ``docker commit`` command. You've specified two flags: ``-m`` and ``-a``. The ``-m`` flag allows us to specify a commit message, much like you would with a commit on a version control system. The ``-a`` flag allows us to specify an author for our update.

You've also specified the container you want to create this new image from, ``0b2616b0e5a8`` (the ID you recorded earlier) and you've specified a target for the image:
``ouruser/sinatra:v2``


Break this target down. It consists of a new user, ``ouruser``, that you're writing this image to. You've also specified the name of the image, here you're keeping the original image name ``sinatra``. Finally you're specifying a tag for the image: ``v2``.

You can then look at our new ``ouruser/sinatra`` image using the ``docker images`` command.
```bash
$ docker images
REPOSITORY          TAG     IMAGE ID       CREATED       SIZE
training/sinatra    latest  5bc342fa0b91   10 hours ago  446.7 MB
ouruser/sinatra     v2      3c59e02ddd1a   10 hours ago  446.7 MB
ouruser/sinatra     latest  5db5f8471261   10 hours ago  446.7 MB
```

To use our new image to create a container you can then:
```bash
$ docker run -t -i ouruser/sinatra:v2 /bin/bash
root@78e82f680994:/#
```

## Building an image from a Dockerfile

Using the ``docker commit`` command is a pretty simple way of extending an image but it's a bit cumbersome and it's not easy to share a development process for images amongst a team. Instead you can use a new command, ``docker build``, to build new images from scratch.

To do this you create a ``Dockerfile`` that contains a set of instructions that tell Docker how to build our image.

First, create a directory and a ``Dockerfile``.
```bash
$ mkdir sinatra
$ cd sinatra
$ touch Dockerfile
```

Each instruction creates a new layer of the image. Try a simple example now for building your own Sinatra image for your fictitious development team.

Open the `Dockerfile` with your favorite text editor - for example, `nano`:

```bash
$ nano Dockerfile
```

Type or paste the following lines inside the `Dockerfile`:

```bash
# This is a comment
FROM ubuntu:14.04
MAINTAINER Kate Smith <ksmith@example.com>
RUN apt-get update && apt-get install -y ruby ruby-dev
RUN gem install sinatra
```

Now save the file and close the text editor. If you're using `nano`, the key combination is `CTRL + O` to save, and `CTRL + X` to exit, respectively.

Let's examine what your ``Dockerfile`` does. Each instruction prefixes a statement and is capitalized.
```bash
INSTRUCTION statement
```


> Note: You use # to indicate a comment

The first instruction ``FROM`` tells Docker what the source of our image is, in this case you're basing our new image on an Ubuntu 14.04 image. The instruction uses the ``MAINTAINER`` instruction to specify who maintains the new image.

Lastly, you've specified two ``RUN`` instructions. A RUN instruction executes a command inside the image, for example installing a package. Here you're updating our APT cache, installing Ruby and RubyGems and then installing the Sinatra gem.

Now let's take our ``Dockerfile`` and use the ``docker build`` command to build an image.

> NOTE: There is a . (dot) at the end of the `docker build` command, which tells Docker to build from the current folder.

```bash
$ docker build -t ouruser/sinatra:v2 .
Sending build context to Docker daemon 2.048 kB
Sending build context to Docker daemon
Step 1 : FROM ubuntu:14.04
 ---> e54ca5efa2e9
Step 2 : MAINTAINER Kate Smith <ksmith@example.com>
 ---> Using cache
 ---> 851baf55332b
Step 3 : RUN apt-get update && apt-get install -y ruby ruby-dev
 ---> Running in 3a2558904e9b
. . . 
 ---> c55c31703134
Removing intermediate container 3a2558904e9b
Step 4 : RUN gem install sinatra
 ---> Running in 6b81cb6313e5
. . . 
 ---> 97feabe5d2ed
Removing intermediate container 6b81cb6313e5
Successfully built 97feabe5d2ed
```

You've specified our ``docker build`` command and used the ``-t`` flag to identify our new image as belonging to the user ``ouruser``, the repository name ``sinatra`` and given it the tag ``v2``.

You've also specified the location of our ``Dockerfile`` using the . to indicate a ``Dockerfile`` in the current directory.


> Note: You can also specify a path to a Dockerfile.

Now you can see the build process at work. The first thing Docker does is upload the build context: basically the contents of the directory you're building in. This is done because the Docker daemon does the actual build of the image and it needs the local context to do it.

Next you can see each instruction in the ``Dockerfile`` being executed step-by-step. You can see that each step creates a new container, runs the instruction inside that container and then commits that change - just like the ``docker commit`` work flow you saw earlier. When all the instructions have executed you're left with the ``97feabe5d2ed`` image (also helpfully tagged as ``ouruser/sinatra:v2``) and all intermediate containers will get removed to clean things up.

You can then create a container from our new image.
```bash
$ docker run -t -i ouruser/sinatra:v2 /bin/bash
root@8196968dac35:/#
```

## Setting tags on an image

You can also add a tag to an existing image after you commit or build it. We can do this using the ``docker tag`` command. Now, add a new tag to your ``ouruser/sinatra`` image.
```bash
$ docker tag 5db5f8471261 ouruser/sinatra:devel
```

The ``docker tag`` command takes the ID of the image, here ``5db5f8471261``, and our user name, the repository name and the new tag.

Now, see your new tag using the ``docker images`` command.

```bash
$ docker images ouruser/sinatra
REPOSITORY          TAG     IMAGE ID      CREATED        SIZE
ouruser/sinatra     latest  5db5f8471261  11 hours ago   446.7 MB
ouruser/sinatra     devel   5db5f8471261  11 hours ago   446.7 MB
ouruser/sinatra     v2      5db5f8471261  11 hours ago   446.7 MB
```


## Push an image to Docker Hub

Once you've built or created a new image you can push it to Docker Hub using the ``docker push`` command. This allows you to share it with others, either publicly, or push it into a private repository.
```bash
$ docker push ouruser/sinatra
The push refers to a repository [ouruser/sinatra] (len: 1)
Sending image list
Pushing repository ouruser/sinatra (3 tags)
. . .
```

## Remove an image from the host

You can also remove images on your Docker host in a way similar to containers using the ``docker `` command.

Delete the ``training/sinatra`` image as you don't need it anymore.
```bash
$ docker rmi training/sinatra
Untagged: training/sinatra:latest
Deleted: 5bc342fa0b91cabf65246837015197eecfa24b2213ed6a51a8974ae250fedd8d
Deleted: ed0fffdcdae5eb2c3a55549857a8be7fc8bc4241fb19ad714364cbfd7a56b22f
Deleted: 5c58979d73ae448df5af1d8142436d81116187a7633082650549c52c3a2418f0
```


> Note: To remove an image from the host, please make sure that there are no containers actively based on it.

### References

This exercise borrows and adapts content from the following sources:

1. [The official Docker documentation](https://docs.docker.com/)

[Back to Menu](../README.md)