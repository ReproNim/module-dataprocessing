---
title: "Lesson 3: Create and maintain reproducible computational environments"
teaching: 60
exercises: 120
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

- Element 1: Overview
- Element 2: Understanding container technologies
- Element 3: Using reproducible computational environments
- Element 4: Creating reproducible computational environments
- Element 5: Capturing the essential pieces needed for an analysis

### Lesson requirements

It is essential to have a basic understanding of:
- Unix operating system, command line, and shell
- Package managers (e.g., apt, yum, brew, linuxbrew, conda, npm)
- Git/Github

###  Element 1: Overview

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

There are many ways to re-create a complete environment. Usually it is done via
construction of environment "images" or "containers".

### Element 2: Understanding container technologies

Container technologies provide a mechanism to encapsulate analysis environments
for redistribution. Many of these technologies also allow creating an executable
environment based on a script, thus allowing ease of reproducing analysis
environments.

There are two main types of container technology -- [virtual machines](https://en.wikipedia.org/wiki/Virtual_machine) 
and "Docker-type" containers. 
VirtualBox, VMware, AWS, or Google Compute Engine are examples of Virtual Machines.
The most common technology within the second type is Docker, but it is not 
the only example.
Among scientist that use High Performance Centers (HPCs) Singularity becomes more popular.

The main idea behind these two types is the same -- isolate the computing environment.
Isolating the environment subsequently allows regenerating 
and sharing the computing environments.

However, there are important differences between the two types, 
that is pictured below:

<img src="../fig/Containers-vs-Virtual-Machines.jpg" width="50%" />

Virtual Machines emulate whole computer system (software+hardware).
They use hypervisor to share and manage hardware of the host, and execute 
the guest operating system.
All guest machines are completely isolated and have dedicated resources.
On the other hand, Docker containers share the host system’s kernel with other containers,
only bins and libs are created from scratch.
Each container gets its own isolated user space.
Comparing to Virtual Machine, containers are very lightweight and fast to start up.

There is no one solution that always works, your choice should depend on:
 - which hardware is available to you (also do you require GPU),
 - where is your data stored.

Docker might me the most portable technology right now, but most HPC
do not support Docker. 
However, the system administrator should agree to install Singularity.

So what are the differences between Docker and Singularity?
Docker is an open-source project and right now it's a leading software container platform.
There are various versions of Docker depending on the system you’re using, 
including Docker for Mac (for OSX users) and Docker for Windows (for Windows Pro users). 
If you have the Home edition of Windows, you still can use Docker, but have to install VM first.
Unfortunately, Docker can not be easily used on traditional HPC resources. 
One of the main reason is privilege escalation via Docker, i.e. users can get root access to the host system.


Singularity offers an alternative to docker on HPC clusters. 
A user inside a Singularity container is the same user as outside the container, so you can be a root
only if you are root on the host system.
Creating singularity containers requires root privileges on a machine virtual or physical. 
However, using singularity does not. The Singularity User Guide describes how to use the different components of singularity. One of the big advantages of Singularity is support for native drivers and libraries (e.g., GPU, MPI, etc.,.).
If you have Linux you can directly install and run on your OS. 
For other systems, you should use Vagrant and VM, follow the instruction for Mac or Windows.
If you are a Docker user, the good news is that a Singularity image can be created
from an existing Docker image.

You can use each of the technologies above to setup analysis environments for brain imaging, but there are important technical differences between them.

 - [Nice introduction to containers and technical differences between VM and Docker](https://medium.freecodecamp.com/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b#.kchrpokfz)
between these technologies.
 - [Features comparison between Docker and Singularity](https://tin6150.github.io/psg/blogger_container_hpc.html).
 - [More technical comparison of Docker and Singularity](http://www.admin-magazine.com/HPC/Articles/Singularity-A-Container-for-HPC)   


> ## Question:
>
>  Which container technologies can be used in HPC centers?
>
> > ## Answer
> >
> > VM with Vagrant
> > Singularity
> >
> {: .solution}
{: .challenge}



### Element 3: Using pre-built containers for brain imaging

#### 1. Vagrant: A neuroimaging environment based on [NeuroDebian](http://neuro.debian.net/) can be initialized quickly
using a single command:

   ```
   vagrant init hlaubish/NeuroDebian_64; vagrant up --provider virtualbox
   ```

   You can read more about NeuroDebian VM [here](http://neuro.debian.net/vm.html)

   A general introduction to Vagrant is [available here](https://www.vagrantup.com/docs/getting-started/)
and a video tutorial is below.
<script type="text/javascript" src="https://asciinema.org/a/11428.js" id="asciicast-11428" async></script>

   Vagrant is based on VirtualBox, and another example of reusable environment is this [virtual machine](https://s3.amazonaws.com/openfmri/virtual-machines/precise64_neuro.box).
This virtual machine can be used to reproduce the analyses from [this paper](http://www.nature.com/articles/ncomms9885).

#### 2. NITRC-CE: TODO

#### 3. Docker:

There are many existing images available on [Docker Hub](https://hub.docker.com/).
You can find images for [Ubuntu](https://hub.docker.com/_/ubuntu/) as well as images that contain
more specific software, e.g. [Nipype](https://hub.docker.com/r/nipype/nipype/).
Simple examples of how to pull and run an image can be found in this
[presentation](http://nipy.org/workshops/2017-03-boston/lectures/lesson-container/#29).

A set of example brain imaging docker images can be also found as
part of the [BIDS-Apps](http://bids-apps.neuroimaging.io/apps/) project.
The project provides a basic [tutorial](http://bids-apps.neuroimaging.io/tutorial/)
to get started with the app.

#### 4. Singularity:

Singularity is useful in HPC centers where docker is not
allowed. Any docker image can be pulled in as a singularity container. Therefore,
you can retrieve any of the bids-apps above as a singularity image using the
command:

   ```
   singularity shell docker://bids/freesurfer
   ```

Singularity also has an online registry for images -- [Singularity Hub](https://singularity-hub.org/).
You can pull images directly from either Docker Hub or Singularity Hub, more about `pull`
command you can find [here](http://singularity.lbl.gov/docs-pull).

More details on how to run an image you can find
[here](http://singularity.lbl.gov/singularity-tutorial#make-and-run-containers).

> ## Question:
>
>  Can you run a Docker image in HPC centers?
>
> > ## Answer
> >
> > Yes, if you have Singularity.
> > Singularity can run both Singularity and Docker images.
> >
> {: .solution}
{: .challenge}

> ## Hands on exercise:
>
> Pull satra/nih-workshop-2017 Docker image and check which python packages are installed.
>
> {: .solution}
{: .challenge}

> ## Hands on exercise:
>
> Repeat the previous exercise using Singularity.
>
> {: .solution}
{: .challenge}


> ## Hands on exercise:
>
> Follow  [Simple Workflow README] (https://github.com/ReproNim/simple_workflow)
> and run `run_demo_workflow.py` for one subject.
> Be sure to mount the directory to save your output.
>
> {: .solution}
{: .challenge}

> ## Hands on exercise:
>
> Repeat the previous exercise using Singularity.
>
> {: .solution}
{: .challenge}



### Element 4: Creating reproducible computational environments

#### 1. Creating a **Vagrant VM** for distribution
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

#### 2. Create a **Docker** image

In order to create a Docker Image, you should write a
[Dockerfile](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/).
A simple example of writing Dockerfile and build an image you can find
[here](http://nipy.org/workshops/2017-03-boston/lectures/lesson-container/#31).

If you want to create a new image for neuroimaging, you should check
[Nuerodocker project](https://github.com/kaczmarj/neurodocker) that allows you
to  generate custom Dockerfiles and minifies existing Docker images.
Neurodocker not only simplifies writing a new Dockerfile, but also incorporates
the best practice for installing software.
You can compare [a simple script to create a Docker image with `FSL`](https://github.com/djarecka/neurodocker/blob/examples/examples/fsl/create_dockerfile.sh)
to [a Dockerfile itself](https://github.com/djarecka/neurodocker/blob/examples/examples/fsl/Dockerfile)
that contains much more details of proper installation and cleaning.
Neurodocker can be also easily used to include Python and all Python libraries that can
be installed using `conda` or `pip`.
This is a simple example of [neurodocker command](https://github.com/djarecka/neurodocker/blob/examples/examples/conda_python/create_dockerfile.sh)
and [Dockerfile](https://github.com/djarecka/neurodocker/blob/examples/examples/conda_python/Dockerfile).
More examples can be found [here](https://github.com/djarecka/neurodocker/tree/examples/examples).


#### 3. Create a **Singularity** image

In order to create an empty image or import layers from Docker image, you don't
need root privileges. You can do it using
[`create` and `import` commands](http://singularity.lbl.gov/quickstart#command-quick-start).

However, if you want to install additional software or create an image from scratch,
you need to have root privileges on a machine.
It doesn't have to be a physical machine, if you're using HPC account, you can use Vagrant
to create a Virtual Machine with Singularity  that can in turn be used to create a new image.

Creating a Dockerfile is only the first step, we have the "receipe", now we can ask 
Docker to create an image using `docker build` command:

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

If you want to create a new image you're going to share, the recommended practice is to
create a bootstrap file.
You can still start from a Docker image, but you can easily add environmental variables,
additional software etc.
A short recipe for bootstrap files you can find here
[here](http://singularity.lbl.gov/quickstart#bootstrap-recipes),
for best practices and more details check [this](http://singularity.lbl.gov/bootstrap-image#best-practices-for-bootstrapping).

For more information about creating a Singularity image, changing size, etc., you should
check [this Singularity website that contains nice short videos](http://singularity.lbl.gov/create-image).


If you want to change already existing image (e.g. for testing purpose),
you can mount the image using
[`--writable` option](http://singularity.lbl.gov/docs-changing-containers)
(yes, you need to have root privileges).


> ## Question:
>
> Which Singularity commands require root privileges?
>
> > ## Answer
> >
> > bootstrap
> > most commands with `--writable` options
> >
> {: .solution}
{: .challenge}

> ## Hands on exercise:
>
> Create a Docker image using Neurodocker with a specific version of FSL
> and a Python 3.6 conda environment.
>
> {: .solution}
{: .challenge}


We can also run an interactive session with Docker:

```bash
docker run -it my_fsl
```

Now you are with docker container and you can type commands directly.

<img src="../fig/docker5.jpeg" width="40%" />

> ## Question:
>
> Create a Singularity image by importing previously created Docker image.
>
> {: .solution}
{: .challenge}


> ## Hands on exercise:
>
> Create a bootsrap file that starts from your Docker image.
> In addition to FSL install git and your favourite text editor (e.g. emacs).
> Create directories for your data and output.
>
> {: .solution}
{: .challenge}

### Element 5: Capturing the essential pieces needed for an analysis

Many packages, such as FSL, FreeSurfer, AFNI, do many different kinds of
computation. Not all of the components are necessary for a specific analysis,
and definitely not for repeating the exact analysis steps. [ReproZip](https://reprozip.readthedocs.io/en/1.0.x/)
allows users to package the essential pieces necessary for an analysis. ReproZip
can be used inside all of the above container technologies to create a minimal
package for repeating an analysis. As such the reprozip bundle contains the
necessary software components for recreating the exact software environment used
in an analysis.
