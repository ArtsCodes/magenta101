# Magenta 101
by [Timothy Vallier](http://www.timothyvallier.com/)

Magenta is a project from the Google Brain team that asks: Can we use machine learning to create compelling art and music? If so, how? If not, why not?  

This article will walk you through the steps of setting up a Magenta environment, generating some musical material, and then arranging the material into a traditional song form. Thankfully, the Magenta team has made this process relatively straightforward, but fair warning: _this tutorial will require the use of the command line_.  

## From Tensorflow to Magenta

Google Brain is a deep learning research project that began in 2011 at Google. The team had been interested in using deep learning techniques to crack the problem of artificial intelligence since 2006, and in 2011 began collaborating to build a large-scale deep learning software system on top of Google's cloud computing infrastructure.  

The second generation 'Deep Learning' outcome from this team was _TensorFlow_.  

### TensorFlow

Today, TensorFlow is an open source software library for machine learning in various kinds of perceptual and language understanding tasks. It is currently used for both research and production by many different teams in several commercial Google products, such as speech recognition, Gmail, Google Photos, and search.

### Magenta 

Magenta is a specific implementation of the TensorFlow software library with the goal of being able to generate art and music. Understanding a little bit about the powerful tool which Magenta is built upon (TensorFlow) will help steer your creative goals for working with this software.

## Setting Up Your Environment  

There are several ways to access the Magenta source code and to get started generating new musical material, but this article will focus on the simplest and most effective method, with _Docker_.

### WTF is Docker? 

The environment (of a host machine) can cause a great deal of frustration during software development. In particular, your operating system, or the version of Python you have installed, all impact what the source code expects to see on your machine. In order to solve this problem, developers have turned to solutions which contain and isolate their production and deployment environments. Enter Docker. You are likely familiar with Virtual Machines, which allow software developers to write and deploy their code to a stable environment. Docker takes the virtual machine concept and fragments it into smaller pieces which do not need to be replicated. This allows many applications to run in a virtual environment which all share the same base OS kernel. Because of this, Docker is a cross-platform solution for developing mission critical software that will run exactly the same in all Docker environemnts.  

### Installing Docker 

Docker Engine is supported on Linux, Cloud, Windows, and macOS. In this tutorial I'll be using macOS, so please refer to the following guide [here](https://docs.docker.com/engine/installation/) for installing Docker on your host OS.  

### Command Line: Acquiring the Magenta Docker Image

Once docker is installed, and you've rebooted your machine, it should be running on your system. You can hover or click on the Docker icon in your tray and you should see a green light indicating Docker is running. It may take a few minutes upon installing in order to get the green light. 

First, let's make a folder on our machine to hold our Magenta files. I'm going to make my folder on the desktop and name it 'magenta'. Note: macOS format below.

```
sudo mkdir ~/Desktop/magenta
```

Next, open the terminal (or PowerShell on Windows) and execute the following command in order to download the Magenta Docker Image and run it for the first time. 

```
docker run -it -p 6006:6006 -v ~/Desktop/magenta:/magenta-data tensorflow/magenta
```

Once you submit the previous line, the Magenta Docker image will begin downloading. This is a 3.7gb~ image, so grab a cup of coffee and wait for the command line to come to a rest. While this is happening, let's break down the previous line we just submitted to the terminal. 

The `docker run` command instructs docker to run the image which is passed in as argument, in this case `tensorflow/magenta`. Docker maintains a registry of images, so when `tensorflow/magenta` is passed as an argumant, the registry looks for the latest image and begins downloading it to your system. The `-it` property instructs docker to run this image with an `i`nteractive `t`erminal. This will place you inside the shell of the image, allowing you to pass the magenta software commands. The `-p` property requests docker to open ports, and the argument `6006:6006` is passed, which requests a 1:1 mapping of the image port and the host port. This allows the docker image and the host machine to communicate with one another seamlessly. Lastly, the `-v` property instructs docker to mount a volume, and the argument we passed, `~/Desktop/magenta:/magenta-data` mounts a folder from the host at `~/Desktop/magenta` to the docker image at `/magenta-data`. 