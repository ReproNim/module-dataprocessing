---
title: "Lesson 3: Create and maintain reproducible computational environments"
teaching: 30
exercises: 30
questions:
- "Why and how to use containers and Virtual Machines?"
objectives:
- "Determine the requirements of your analysis software for reproducibility"
- "Use and create containers for reproducible research"
keypoints:
- Hardware and software comprise an analysis environment.
- Software and hardware components interact through command options and environment variables. 
---

> ## You can skip this lesson if you can answer these questions? --->
>
> - Can you use an AWS image or docker application to analyze your data?
> - Can you recreate your analysis environment on a new computer?
> - Can you **re-generate** your analysis environment using container technologies?
> - Can you publish your analysis environment for others to reuse?
{: .challenge}

This lesson is an introduction to reusable computational environments. This will
focus on components relevant to brain imaging analyses.

### Lesson outline

- Understanding container technologies
- Using reproducible computational environments
- Creating reproducible computational environments

### Lesson requirements

It is essential to have a basic understanding of:
- Unix operating system, command line, and shell
- Package managers (e.g., apt, yum, brew, linuxbrew, conda, npm)
- Git/Github

###  Overview

Carrying out reproducible data processing requires understanding details of the
software environment used in the analysis. An analysis is the outcome of data,
analysis scripts, and the analysis environment (software and hardware). Modern
operating systems are complex and every analysis software may have dependencies
on many different components of the system. These include:
 
 - Software packages and libraries that come with the operating system
 - Software packages and libraries installed by a user or administrator
 - Environment variables and settings in a user's operating environment
 - Interactions between software and available hardware (e.g., multicore, GPU)
 - Schedulers and clusters
 
Given the complexity of the environment, it is imperative to get a clear 
understanding of:
 1. How to capture the details of the software and hardware requirements of an analysis
 2. How to determine which of these components can have a direct impact on results
 3. How to recreate the environment in which the analysis was done

There are many ways to re-create an

#### Understanding container technologies

Container technologies provide a mechanism to encapsulate analysis environments
for redistribution. Many of these technologies also allow creating an executable
environment based on a script, thus allowing ease of reproducing analysis 
environments. Some popular virtual machine and container technologies are:

  1. Virtual Machines and [Vagrant](https://www.vagrantup.com/)
  2. [Amazon AMIs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html#creating-an-ami)
     a. [NITRC Computational Environment: NITRC-CE](http://www.nitrc.org/plugins/mwiki/index.php/nitrc:User_Guide_-_NITRC_Computational_Environment)
  3. [Docker](https://www.docker.com/)
  4. [Singularity](http://singularity.lbl.gov/)

There are [technical differences](https://medium.freecodecamp.com/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b#.kchrpokfz) 
between these technologies.

You can use each of the technologies above to setup analysis environments for 
brain imaging.

#### Using pre-built containers for brain imaging

1. **Vagrant:** A neuroimaging environment based on [Neurodebian](http://neuro.debian.net/) can be initialized quickly 
using a single command: 
   
   ```
   vagrant init hlaubish/NeuroDebian_64; vagrant up --provider virtualbox
   ```
   
   Another example of reusable environment is this [virtual machine](https://s3.amazonaws.com/openfmri/virtual-machines/precise64_neuro.box).
This virtual machine can be used to reproduce the analyses from [this paper](http://www.nature.com/articles/ncomms9885).

   A general introduction to Vagrant is [available here](https://www.vagrantup.com/docs/getting-started/)
and a video tutorial is below. 
<script type="text/javascript" src="https://asciinema.org/a/11428.js" id="asciicast-11428" async></script>

2. **NITRC-CE:** TODO

3. **Docker**: An set of example brain imaging docker containers can be found as
part of the [BIDS-Apps](http://bids-apps.neuroimaging.io/) project. The project
provides a basic tutorial to get started with the app.

4. **Singularity**: Singularity is useful in HPC centers where docker is not 
allowed. Any docker image can be pulled in as a singularity container. Therefore,
you can retrieve any of the bids-apps above as a singularity image using the 
command:

   ```
   singularity shell docker://bids/freesurfer
   ```
   
For more details see the following [tutorial](http://singularity.lbl.gov/singularity-tutorial)

#### Creating a Vagrant VM for distribution
Vagrant supports VirtualBox and VMWare virtual machines. [Using Vagrant with 
VirtualBox](https://www.vagrantup.com/docs/getting-started/) is a matter of 
creating a Vagrantfile and using it download and configure an execution 
environment. As an example, one can consider how to create an image with 
Neurodebian and install FSL tools into it:
   
```
vagrant init ubuntu/trusty64
vagrant up

vagrant ssh -c /bin/sh <<EOF
   wget -O- http://neuro.debian.net/lists/trusty.us-nh.full | sudo tee /etc/apt/sources.list.d/neurodebian.sources.list
   sudo apt-key adv --recv-keys --keyserver hkp://pgp.mit.edu:80 0xA5D32F012649A5A9    sudo apt-get update
   sudo apt-get -y install fsl-complete
EOF
```

#### Docker
Docker provides a slew of services to make building and distributing containers
easier. This includes integration with GitHub and the ability to **`pull`**
pre-built containers from Docker hub. In addition docker containers can 
orchestrated together with **`docker-compose`** to generate interacting services.

1. [Gentle introduction to docker for scientists](https://neurohackweek.github.io/docker-for-scientists/)
2. [More extensive tutorial for docker](https://prakhar.me/docker-curriculum/) 

#### Singularity
Singularity offers an alternative to docker on HPC clusters. Creating singularity
containers requires root privileges on a machine virtual or physical. However,
using singularity does not. The [Singularity User Guide](http://singularity.lbl.gov/user-guide)
describes how to use the different components of singularity. One of the big 
advantages of Singularity is support for native drivers and libraries (e.g., GPU,
MPI, etc.,.). One can also use vagrant to create a virtual machine with singularity that can 
in turn be used to create another singularity container.

```
vagrant init ubuntu/trusty64
vagrant up

vagrant ssh -c /bin/sh <<EOF
   sudo apt-get update
   sudo apt-get -y install build-essential curl git sudo man vim autoconf libtool
   git clone https://github.com/singularityware/singularity.git
   cd singularity
   ./autogen.sh
   ./configure --prefix=/usr/local
   make
   sudo make install
EOF
```

Here is a tutorial that uses such a virtual machine to create a Tensorflow 
container with GPU support. <script type="text/javascript" src="https://asciinema.org/a/100998.js" id="asciicast-100998" async></script>

### Capturing the essential pieces needed for an analysis

Many packages, such as FSL, FreeSurfer, AFNI, do many different kinds of 
computation. Not all of the components are necessary for a specific analysis, 
and definitely not for repeating the exact analysis steps. [ReproZip](https://reprozip.readthedocs.io/en/1.0.x/) 
allows users to package the essential pieces necessary for an analysis. ReproZip
can be used inside all of the above container technologies to create a minimal
package for repeating an analysis. As such the reprozip bundle contains the 
necessary software components for recreating the exact software environment used
in an analysis.
