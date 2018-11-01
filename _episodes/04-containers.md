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
- Element 2: Types of containers
- Element 3: Using reproducible computational environments
- Element 4: Creating reproducible computational environments
- Element 5: Running analysis within a container
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
construction of virtual environments.

TODO: present 4-8


###  Element 2: Types of containers

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
 - [Gentle introduction to docker and other containers for scientists](http://nipy.org/workshops/2017-03-boston/lectures/lesson-container/#1)
 - [More extensive tutorial for docker (including building and deploying new webapps)](https://prakhar.me/docker-curriculum/)
 - [Features comparison between Docker and Singularity](https://tin6150.github.io/psg/blogger_container_hpc.html).
 - [More technical comparison of Docker and Singularity](http://www.admin-magazine.com/HPC/Articles/Singularity-A-Container-for-HPC)   


> ## Question:
>
>  Which container technologies can be used in HPC centers?
>
> > ## Answer
> >
> > VM
> > Singularity
> >
> {: .solution}
{: .challenge}



### Element 3: Using pre-built containers for brain imaging

If you are running a container on your laptop, it uses the same hardware, 
but user spaces and libraries are independent.

<img src="../fig/docker1in.jpeg" width="10%" />


<img src="../fig/docker2in.jpeg" width="25%" />


You can alway create additional bindings between the container and the host machine.

<img src="../fig/docker3in.jpeg" width="35%" />


There are many existing images available on [Docker Hub](https://hub.docker.com/) 
or [Singularity Hub](https://singularity-hub.org/).
You can find images for [Ubuntu](https://hub.docker.com/_/ubuntu/) as well as images that contain
more specific software, e.g. [Nipype](https://hub.docker.com/r/nipype/nipype/).
Simple examples of how to pull and run a Docker image can be found in this
[presentation](http://nipy.org/workshops/2017-03-boston/lectures/lesson-container/#29).

Information about pulling an existing image from Singularity Hub 
you can find [here](http://singularity.lbl.gov/docs-pull).
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


TODO - move it 
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


#### **Docker** image

In order to create a docker image we need to write a Dockerfile, that contains all the commands a user could call 
on the command line to assemble an image.  Dockerfile provide a “recipe” for an image.
A simple Dockerfile might look like this

  ```bash
  FROM ubuntu:latest
  RUN apt-get update -y && apt-get install -y git emacs
  ```

But Dockerfile can me much more complicate, a Dockerfile with FSL might look like this:

  ```bash
  FROM neurodebian:stretch-non-free
  ARG DEBIAN_FRONTEND="noninteractive"

  ENV LANG="en_US.UTF-8" \
      LC_ALL="en_US.UTF-8" \
      ND_ENTRYPOINT="/neurodocker/startup.sh"
  RUN export ND_ENTRYPOINT="/neurodocker/startup.sh" \
      && apt-get update -qq \
      && apt-get install -y -q --no-install-recommends \
             apt-utils bzip2 ca-certificates \
             curl locales unzip \
      && apt-get clean \
      && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
      && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
      && dpkg-reconfigure --frontend=noninteractive locales \
      && update-locale LANG="en_US.UTF-8" \
      && chmod 777 /opt && chmod a+s /opt \
      && mkdir -p /neurodocker \
      && if [ ! -f "$ND_ENTRYPOINT" ]; then \
           echo '#!/usr/bin/env bash' >> "$ND_ENTRYPOINT" \
      &&   echo 'set -e' >> "$ND_ENTRYPOINT" \
      &&   echo 'if [ -n "$1" ]; then "$@"; else /usr/bin/env bash; fi' >> "$ND_ENTRYPOINT"; \
      fi \
      && chmod -R 777 /neurodocker && chmod a+s /neurodocker
  ENTRYPOINT ["/neurodocker/startup.sh"]
  RUN apt-get update -qq \
      && apt-get install -y -q --no-install-recommends \
             fsl-5.0-core \
             fsl-mni152-templates \
      && apt-get clean \
      && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
  RUN sed -i '$isource /etc/fsl/5.0/fsl.sh' $ND_ENTRYPOINT

  ```

> ## Question:
>
> What is a meaning of commands `FROM`, `ENV`, `ENTRYPOINT` and `RUN` in the FSL Dockerfile?
>
> > ## Answer
> >
> > Check the official 
> > [Dockerfile Best Practice](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/).
> >
> {: .solution}
{: .challenge}


#### **Neurodocker** image

If you want to create a new image for neuroimaging, you should check
[Nuerodocker project](https://github.com/kaczmarj/neurodocker) that allows you
to  generate custom Dockerfiles and minifies existing Docker images.
Neurodocker not only simplifies writing a new Dockerfile, but also incorporates
the best practice for installing software.
It supports popular neuroimaging software: AFNI, ANTs, Convert3D, dcm2niix, FreeSurfer, 
FSL, MINC, Miniconda (Python), MRtrix3, PETPVC, SPM.
You can find full explanation of argument for `generate docker` command 
[here](https://github.com/kaczmarj/neurodocker#generate-dockerfile).  

Let's create an FSL Dockerfile using neurodocker. 
We will start from creating a new directory:

```bash
mkdir my_docker
cd my_docker
```

And now we can create the Dockerfile (we are using docker container with neurodocker already installed): 

  ```bash
  docker run --rm kaczmarj/neurodocker:master generate docker \
  --base neurodebian:stretch-non-free \
  --pkg-manager apt \
  --install fsl-5.0-core fsl-mni152-templates \
  --add-to-entrypoint "source /etc/fsl/5.0/fsl.sh"

  ```

Creating a Dockerfile is only the first step, we have the "receipe", now we can ask 
Docker to create an image using `docker build` command:

```bash
docker build -t my_fsl .
```

We can check if the image is created:

```bash
docker images
```

> ## Question:
>
> How to create an image with a tag?
> You can search Docker website or check help: `docker build --help`
>
> > ## Answer
> >
> > 
> > docker build -t my_fsl:sfn18
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





#### **Singularity** image

You can use Neurodocker also to create a Singularity file by using `generate singularity` command:

```bash
docker run --rm kaczmarj/neurodocker:master generate singularity \
--base neurodebian:stretch-non-free \
--pkg-manager apt \
--install fsl-5.0-core fsl-mni152-templates \
--add-to-entrypoint "source /etc/fsl/5.0/fsl.sh" > Singularity_fsl 
```

In order to build a Singularity image, you need to have root privileges on a machine:
```bash
sudo singularity build my_fsl.simg Singularity_fsl 
```


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


### Element 5: Running analysis within a container

Once we have a Docker image with FSL we can try to run an FSL command, e.g. `bet`:

The simplest way of executing the command:

```bash
docker run my_fsl bet
```

<img src="../fig/docker2.jpeg" width="50%" />


We can see bet output on our screen and we are back in our host system.
We have to have an input to run bet command, and we will download a T1w file using datalad 
(please go to section TODO to learn about datalad if you are not familiar). 
We will install `ds000114` dataset and download one file only:

```bash
mkdir data
cd data
datalad install -r ///workshops/nih-2017/ds000114
datalad get ds000114/sub-01/ses-test/anat/sub-01_ses-test_T1w.nii.gz
cd ..
```

In order to use the T1w file that is in the filesystem on the host machine, 
we  should mount a local directory with data and run *bet* on the downloaded file:

```bash
docker run -v ~/ds000114:/data my_fsl bet \
/data/sub-01/ses-test/anat/sub-01_ses-test_T1w.nii.gz sub-01_output
```


Once it's done, we can check the output:

```bash
ls -l
```

As we can see there is no bet output in our directory.
This is because the output file `sub-01_output` was not created in a directory that was shared 
with the host system.

<img src="../fig/docker3.jpeg" width="50%" />

We can fix it by creating a new directory for output:

```bash
mkdir output
```

and  mounting the output directory in addition to data directory:

```bash
docker run -v ~/ds000114:/data -v ~/output:/output my_fsl bet \
/data/sub-01/ses-test/anat/sub-01_ses-test_T1w.nii.gz /output/sub-01_output
```

Now we should be able to see the output:
```bash
ls -l output
```

<img src="../fig/docker4.jpeg" width="50%" />


> ## Hands on exercise:
>
> Repeat the analysis using the Singularity container
>
> {: .solution}
> > Home directory is automatically mounted, so we don't have to specify (`-B` can be used to add more mounting points).
> >
> > ```bash
> > singularity run images/fsl.simg bet \
> > ~/ds000114/sub-01/ses-test/anat/sub-01_ses-test_T1w.nii.gz \
> > ~/output/sub-01_output_sing
> > ```
> >
> > Checking the output
> > ```bash
> > ls -l ~/output
> > ```
{: .challenge}


We can also run an interactive session with Docker:

```bash
docker run -it my_fsl
```

Now you are with docker container and you can type commands directly.

<img src="../fig/docker5.jpeg" width="40%" />

> ## Question:
>
> Which Singularity commands is used to open an interactive session?
>
> > ## Answer
> >
> > ```bash
> > singularity shell image
> > ```
> > Note: `shell` doesn't run automatically the file specified in `%runscript` or `ENTRYPOINT`.
> {: .solution}
{: .challenge}



### Element 6: Capturing the essential pieces needed for an analysis

Many packages, such as FSL, FreeSurfer, AFNI, do many different kinds of
computation. Not all of the components are necessary for a specific analysis,
and definitely not for repeating the exact analysis steps. [ReproZip](https://reprozip.readthedocs.io/en/1.0.x/)
allows users to package the essential pieces necessary for an analysis. ReproZip
can be used inside all of the above container technologies to create a minimal
package for repeating an analysis. As such the reprozip bundle contains the
necessary software components for recreating the exact software environment used
in an analysis.
