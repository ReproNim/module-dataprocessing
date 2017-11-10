---
title: "Module overview"
teaching: 10
exercises: 0
questions:
- "What do we need to know to conduct reproducible analysis?"
objectives:
- Understand the conceptual pieces that make up reproducible research.
- Learn where to go for information
keypoints:
- Reproducible research requires understanding all pieces of the data flow
- You should be familiar with the necessary elements and tools for reproducible analysis.

---


> ## You can skip this module if you can answer these questions? --->
>
>  - What elements should be captured for repeatable analysis?
>  - How do you annotate a CSV file for others to understand?
>  - How do you convert a docker container to singularity?
>  - How can you recreate your analysis environment on any machine?
{: .challenge}

The typical brain imaging experiment uses data, software, and human
interaction to test hypotheses and/or explore relations in data. These
analyses can involve many different elements (data quality,
software environment, algorithms, human input) that can introduce errors.
It is therefore useful to capture the information necessary to repeat or
reproduce the analysis.

### An analysis workflow

<img src="{{site.root}}/fig/EDC-annot.png" width="100%">

Reproducing the analysis requires knowing descriptions of:

- data was used in the analysis
- steps to collect or find such data
- analysis steps that were needed (processing and statistics)
- how to redo the analysis steps
- what computing environment was used
- how to recreate the same or similar environment
- how did the researcher validate the output

These steps are needed for the researcher to preserve information for
future use, to document the methods for dissemination, and to repeat the
experiment.

### Prerequisites

For this module, we expect the reader to be familiar with unix computing
concepts and have a general idea of brain image analysis. It is highly
recommended that you go through the overview lectures of the
reproducible basics and FAIR data principles modules.

### What will you learn

You will learn how to perform reproducible analysis, how to preserve the
information, and how to share data and code with others.

### How long will it take

This module consists of 6 lessons, each comprising multiple units. Each
unit in this module will take you up to 10 hours of work.

### Lessons included in this module

1. [Lesson 1.]({{site.root}}/02-concepts) An example framework for reproducible analysis
2. [Lesson 2.]({{site.root}}/03-data) How to annotate, harmonize, clean, and version brain imaging data
3. [Lesson 3.]({{site.root}}/04-containers) How to create and maintain reproducible computational environments for
analysis
4. [Lesson 4.]({{site.root}}/05-dataflows) How to use dataflow tools to capture detailed provenance and perform
efficient analyses
5. [Lesson 5.]({{site.root}}/06-testing) How to setup a testing framework to revalidate analyses as data and
software change
6. [Lesson 6.]({{site.root}}/07-provenance) How to track complete provenance of the analyses from data to results
