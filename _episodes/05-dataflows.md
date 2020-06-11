---
title: "Lesson 4: Create reusable and composable dataflow tools"
teaching: 15
exercises: 0
questions:
- "How to use dataflow tools?"
objectives:
- Understand the conceptual components of dataflow tools
- Learn to use Nipype as an example dataflow tool
keypoints:
- Dataflow tools create abstraction of process from data.
- Dataflow tools allow reuse and composition of tools.
---

> ## You can skip this lesson if you can answer these questions? --->
>
> - Do you you know how to separate data from analysis scripts?
> - Can you write a Nipype workflow?
{: .challenge}

This lesson provides an approach to capturing analysis steps using dataflow
tools that decouple specification of analysis from execution, and often from
data, across different computation platforms, whether a local environment or an
HPC cluster.

### Lesson outline

- Overview: Why you should create reusable analysis components
- Nipype: As an example of reusable and composable components
- Other brain imaging dataflow frameworks

### Lesson requirements

Although not essential it is helpful to have an understanding of:

- Neuroimaging analysis algorithms
- The lessons on [data](/03-data) and [containers](/04-containers) in this module

### Overview

Most brain imaging analyses comprise common steps: preprocessing, quality control,
registration, normalization, statistical estimation. However, there are many
software and tools that implement many of these of algorithms. The performance
of these algorithms often vary as a function of participant age, data quality,
and computational environment. Each software brings with a set of strengths and
weaknesses and as many analyses move towards multimodal data, it becomes
essential to be able to combine elements of different packages.

However, such combination comes at a cost. It increases the complexity of
software environments, the additional learning necessary to combine software
packages, and often the ability to script and program one's analyses. But, once
one masters the ability to perform these kinds of analyses, one can essentially
create any analyses. Further, by thinking about pieces of analyses, one can
optimally combine the most appropriate (e.g., least error, most robust)
algorithms for a task rather than being restricted to what is available in a
single package.

The essence of dataflows are:

1. Separation of data, scripts, and execution.
2. The scripts themselves should not be intricately tied to a particular data
set.
3. The data should be abstracted into a consistent structured form that can be
queried.

Most dataflow frameworks rely on language abstractions to support the flow of
data. Algorithms or dataflows written using such abstractions can be reused on
different datasets.

### A shell script example

### Nipype: an example dataflow tool
[Nipype](nipy.org/nipype) is a Python-based framework for writing dataflows. It
provides access to many brain imaging algorithms and workflows using a
consistent API. It also provides isolation of data being worked on. Here is an
example Nipype workflow.

```python
from nipype import Node, Workflow

in_file = "/path/to/data/T1.nii.gz"

skullstrip = Node(fsl.BET(in_file=in_file, mask=True), name="skullstrip")
smooth = Node(fsl.IsotropicSmooth(in_file=in_file, fwhm=4), name="smooth")
mask = Node(fsl.ApplyMask(), name="mask")

wf = Workflow(name="smoothflow")
wf.connect(skullstrip, "mask_file", mask, "mask_file")
wf.connect([(smooth, mask, [("out_file", "in_file")])])

wf.run()
```

- [Nipype tutorial](https://github.com/miykael/nipype_tutorial)

An example general purpose workflow for functional preprocessing is available as
a Dockerized application that works on BIDS datasets. You can work through the tutorials online using [Binder service](https://mybinder.org/v2/gh/miykael/nipype_tutorial/master?filepath=nipype_tutorial%2Findex.ipynb).

- [Nipypelines for BIDS datasets](https://github.com/BIDS-Apps/nipypelines)

Being a Nipype workflow, any user can modify it to replace one section of the
code with another or retrieve the details of any processing step.

### Other dataflow tools
There are many other dataflow frameworks in use in brain imaging. Each one
provides their own semantics of creating such dataflows and how to execute them.

- [AA](https://github.com/rhodricusack/automaticanalysis)
- [CPAC](https://fcp-indi.github.io/)
- [SPMbatch](https://en.wikibooks.org/wiki/SPM/Batch)
- [NIAK/PSOM](http://psom.simexp-lab.org/)
- [FASTR](http://fastr.readthedocs.io/en/stable/index.html)
- [LONI Pipeline](http://pipeline.loni.usc.edu/)

### CWL and NIDM
There are global efforts to represent computational workflows in more abstract
form. Two such efforts are the [Common Workflow Language](http://www.commonwl.org/)
and [NIDM](http://nidm.nidash.org/). While they may not support direct analyses
at this point for brain imaging, they represent a way to capture the semantics
of computation in an implementation agnostic manner.
