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

## Setting Up Your Environment on macOS  

There are several ways to access the Magenta source code and to get started generating new musical material, but this article will focus on the simplest and most effective method, with _Docker_.

### WTF is Docker? 

The environment (of a host machine) can cause a great deal of frustration during software development. In particular, your operating system, or the version of Python you have installed, all impact what the source code expects to see on your machine. In order to solve this problem, developers have turned to solutions which contain and isolate their production and deployment environments. Enter Docker. You are likely familiar with Virtual Machines, which allow software developers to write and deploy their code to a stable environment. Docker takes the virtual machine concept, and fragments it into smaller pieces which do not need to be replicated. This allows many applications to run in a virtual environment which all share the same base OS kernel. Because of this, Docker is a cross-platform solution for developing mission critical software that will run exactly the same in all Docker environemnts. 
