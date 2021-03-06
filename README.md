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

Once docker is installed and you've rebooted your machine it should be running on your system. You can hover or click on the Docker icon in your tray and you should see a green light indicating Docker is running. It may take a few minutes upon installing in order to get the green light. 

First, let's make a folder on our machine to hold our Magenta files. I'm going to make my folder on the desktop and name it 'magenta'. Note: macOS format below.

```
mkdir ~/Desktop/magenta
```

Next, open the terminal (or PowerShell on Windows) and execute the following command in order to download the Magenta Docker Image and run it for the first time. 

```
docker run -it -p 6006:6006 -v ~/Desktop/magenta:/magenta-data tensorflow/magenta
```

Once you submit the previous line, the Magenta Docker image will begin downloading. This is a 3.7gb~ image, so grab a cup of coffee and wait for the command line to come to a rest. While this is happening, let's break down the previous line we just submitted to the terminal. To exit a docker shell session type `exit` and hit return. 

The `docker run` command instructs docker to run the image which is passed in as argument, in this case `tensorflow/magenta`. Docker maintains a registry of images, so when `tensorflow/magenta` is passed as an argument, the registry looks for the latest image and begins downloading it to your system. The `-it` property instructs docker to run this image with an `i`nteractive `t`erminal. This will place you inside the command line (shell) of the image, which will allow you to pass commands to magenta. The `-p` property requests docker to open ports, and the argument `6006:6006` is passed, which requests a 1:1 mapping of the image port and the host port. This allows the docker image and the host machine to communicate with one another seamlessly. Lastly, the `-v` property instructs docker to mount a volume, and the argument we passed, `~/Desktop/magenta:/magenta-data` mounts a folder from the host at `~/Desktop/magenta` to the docker image at `/magenta-data`. Note: Windows users can change `~/Desktop/magenta` to a path such as `C:/magenta`.  

**WARNING**: only data saved in `/magenta-data` will persist across Docker
sessions.  

### Generating Melodies   

Once the docker image has been downloaded it will start a shell in a directory with all the Magenta components compiled, installed, and ready to run. This will allow us to tap into the pre-trained models and generate melodies without any further configuration. Let's try that now!

Enter the following command in your Docker `tensorflow/magenta` shell:  

```
melody_rnn_generate \
  --config=lookback_rnn \
  --bundle_file=/magenta-models/lookback_rnn.mag \
  --output_dir=/magenta-data/lookback_rnn/generated \
  --num_outputs=10 \
  --num_steps=128 \
  --primer_melody="[60]"
```

Let's discuss the previous command. `melody_rnn_generate` is a package that generates midi files from a recurrent neural network (rnn) model. In this example, we indicated the configuration as `lookback_rnn`, pointed it to an existing model as the bundle_file `/magenta-models/lookback_rnn.mag`, specified the number of files to generate with `num_outputs=10`, specified the number of notes each melody will have with `num_steps=128` and gave it a primer melody of `"[60]"`, which is the midi note for middle 'C'. Lastly, we specified an output directory of `/magenta-data/lookback_rnn/generated`. 

Once this command is executed successfully, you should see the following message in your shell:  

```
INFO:tensorflow:Wrote 10 MIDI files to /magenta-data/lookback_rnn/generated
```

Now we can navigate to our `~/Desktop/Magenta` directory and we should see the output directory generated from the previous command. Your folder structure should resemble the following:  

```
magenta
└───lookback_rnn
│   └───generated
│       │   2017-01-17_153656_01.mid
│       │   2017-01-17_153656_02.mid
│       │   ...
│       │   2017-01-17_153656_10.mid
```

### MIDI Playback  

If you have QuickTime 7 (old version) or Logic Pro X (or a similar DAW) you can open these files and take a listen. If not, I recommend downloading [Aria Maestosa](http://ariamaestosa.sourceforge.net/download.html) which is a cross-platform midi sequencer/editor. 

# Coercing Magenta To Generate Song Materials 

Now that you've got your environment setup to generate melodies with Magenta, we can think about how to break this material down into the individual segments which make up a song. 

## Song structure

_Song structure_ or the _musical forms of songs_ in traditional music are typically sectional with repeated material. Other common forms include thirty-two-bar form, verse-chorus form, and the twelve-bar blues. Popular music songs traditionally use the same music for each verse or stanza of lyrics (as opposed to songs that are "through-composed", an approach used in classical music).  

The most common format is intro, verse, pre-chorus, chorus (or refrain), verse, pre-chorus, chorus, bridge ("middle eight"), verse, chorus and outro. Let's break down each of these components to strategize about how to generate materials for each. 

### Intro

The introduction is a section that comes at the beginning of the piece. It usually builds up suspense for the listener so when the downbeat drops in, it creates a release or surprise. In some cases, an introduction contains only drums or percussion parts which set the rhythm and "groove" for the song. Alternately the introduction may consist of a solo sung by the lead singer (or a group of backup singers), or played by an instrumentalist.  

### Verse  

In popular music, a verse roughly corresponds to a poetic stanza because it consists of rhyming lyrics most often with an AABB or ABAB rhyme scheme. When two or more sections of the song have almost identical music and different lyrics, each section is considered one verse. 


### Pre-chorus 

An optional section that may occur after the verse is the "pre-chorus." Also referred to as a "build", "channel," or "transitional bridge," the pre-chorus functions to connect the verse to the chorus with intermediary material, typically using subdominant or similar transitional harmonies. 

### Chorus   

The chorus contains the main idea, or big picture, of what is being expressed lyrically and/or musically. It is repeated throughout the song, and the melody and lyric rarely vary. It is almost always of greater musical and emotional intensity than the verse. 

### Bridge   

A bridge may be "a transition", but more often in popular music is "a section that contrasts with the verse. In music theory, "middle eight" (a common type of bridge) refers to the section of a song which has a significantly different melody and lyrics, which helps the song develop itself in a natural way by creating a contrast to the previously played, usually placed after the second chorus in a song. 

### Outro  

The conclusion or outro of a song is a way of ending or completing the song. For example, through a fade-out or instrumental tag.  

## Song Structure - Numbers Breakdown

We'll be following a common _song structure_ guideline to help us establish the length of each section.  

You may recall reviewing the earlier generated melodic material in your software or DAW of choice. You may have noticed that the `num_steps=128` argument resulted in a melody that was 8 measures (or bars) in duration, with a common time (4/4) subdivision. We are going to take that knowledge to map out our song structure.  

First, let's create some folders in our `~/Desktop/magenta/lookback_rnn/` directory. We'll create a folder for each section of musical material. Note: since the pre-chorus is optional, we won't be using it in this article.  

```
mkdir ~/Desktop/magenta/lookback_rnn/intro \
mkdir ~/Desktop/magenta/lookback_rnn/verse \
mkdir ~/Desktop/magenta/lookback_rnn/chorus \
mkdir ~/Desktop/magenta/lookback_rnn/bridge \
mkdir ~/Desktop/magenta/lookback_rnn/outro \
```
Now that we have these directories, we can start generating material to fill them.  Our song will have the following structure.  

| Intro  | 2 Bars |
|--------|--------|
| Verse  | 4 Bars |
| Chorus | 4 Bars |
| Verse  | 4 Bars |
| Chorus | 4 Bars |
| Bridge | 8 Bars |
| Chorus | 4 Bars |
| Chorus | 4 Bars |
| Outro  | 2 Bars | 

### Intro - 2 bars - 4 voices
The intro will be 2 bars long, which means that it will need 1/4 of the steps (`32`) we indicated in our original melody(`128`).  

We'll also need to adjust the output_dir to indicate our `~/Desktop/magenta/lookback_rnn/intro` folder. 

Lastly, we'll adjust the `num_outputs` to be `4`, one for each voice of our four part orchestration. At this point we can begin loosly thinking about these outputs as Melody, Harmony 1, Harmony 2, and Bass. How you assign the output will be up to you, but beginning to listen with ears that are seeking the various characteristics of these voices is important. 

Let's generate our Intro with the following command (inside of the Docker shell):  

 ```
melody_rnn_generate \
  --config=lookback_rnn \
  --bundle_file=/magenta-models/lookback_rnn.mag \
  --output_dir=/magenta-data/lookback_rnn/intro \
  --num_outputs=4 \
  --num_steps=32 \
  --primer_melody="[60]"
```
Head on over to your `.../intro` folder to examine your output. You should see 4 midi files each with 2 bars of materials. At this point, I like to listen to each one. You'll notice that some are far more active then others. I tend to take the most active line and tag it as the melody in my DAW. I'll then take the second most active line with the widest range, transpose it down 2 octaves, and tag it as the bass. Lastly, I will label the least active lines as harmony and decide if they need any transposition. If too many of the lines are occupying the same pitch space, consider transposing them 4 or 7 half steps to create a triad. If your melody is still not standing on it's own, you may want to transpose it up an octave. 

Once you have a 4 voice intro you like, we can proceed to the next section. For fun, [here's](https://www.dropbox.com/s/9dyodmc6mk1p3fz/01_Intro.mp3?dl=0) an example of the intro I just assembled while writing this article.  

Let's proceed generating the material for the remain sections in a similar fashion. 

### Verse - 4 bars - 4 voices 
Once again, we need to adjust our Magenta command to reflect the duration and voice of this section, along with the output directory. We'll be doing this for each section. Use the following to generate your verse materials:  

 ```
melody_rnn_generate \
  --config=lookback_rnn \
  --bundle_file=/magenta-models/lookback_rnn.mag \
  --output_dir=/magenta-data/lookback_rnn/verse \
  --num_outputs=4 \
  --num_steps=64 \
  --primer_melody="[60]"
```
Feel free to arrange the materials of your verse like we did with the intro, or skip ahead and generate your materials for each section and then assemble at the end. 

Let's proceed with the code to generate the remainder of our materials.

### Chorus - 4 bars - 4 voices 

 ```
melody_rnn_generate \
  --config=lookback_rnn \
  --bundle_file=/magenta-models/lookback_rnn.mag \
  --output_dir=/magenta-data/lookback_rnn/chorus \
  --num_outputs=4 \
  --num_steps=64 \
  --primer_melody="[60]"
```

### Bridge - 8 bars - 4 voices 

 ```
melody_rnn_generate \
  --config=lookback_rnn \
  --bundle_file=/magenta-models/lookback_rnn.mag \
  --output_dir=/magenta-data/lookback_rnn/bridge \
  --num_outputs=4 \
  --num_steps=128 \
  --primer_melody="[60]"
```

### Outro - 2 bars - 4 voices 

 ```
melody_rnn_generate \
  --config=lookback_rnn \
  --bundle_file=/magenta-models/lookback_rnn.mag \
  --output_dir=/magenta-data/lookback_rnn/outro \
  --num_outputs=4 \
  --num_steps=32 \
  --primer_melody="[60]"
```

## Assembling Your Song

Now that we have all of our materials generated we can begin assembling our song. 

### Step 1: Adjust the Voices of Each section
Just as we stepped through our materials for the intro to tag and transpose each voice to more closely resemble a typical four part ensemble, we need to do the same for each of our remaining sections. 

### Step 2: Organize and Assemble 
Now that you have your materials you simply must lay them out in the song format structure. 

### Step 3: Tweak and Share!
You may have finished assembling your song and you think it's still missing something. Now's your chance to modify the materials you've assembled, adding or subtracting instruments, adding percussion, adjusting voices, etc. This is the fun part. 

[Here's](https://www.dropbox.com/s/hzf8vtsqkwrpf4u/Magenta_Song.mp3?dl=0) an example of a finished track that I've made following this guide. 
