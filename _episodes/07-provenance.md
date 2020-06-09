---
title: "Lesson 6: Track provenance from data to results"
teaching: 15
exercises: 30
questions:
- "Can we represent the history of an entire analysis?"
- "Can we use this history to repeat the analysis?"
objectives:
- Learn to capture analysis in structured form
- Learn how to query the information
keypoints:
- Analysis can be captured in a way to repeat it
- Understanding points of human interaction and decision making are essential for reproducibility

---

> ## You can skip this lesson if you can answer these questions? --->
>
> - Can you capture the details of all your analysis steps in a form that can be repeated?
> - Do you know how to represent an analysis using the W3C PROV model?
{: .challenge}

This lesson provides an approach to capturing the details of an analysis in a
structured form that can be reused to repeat **and** introspect the analysis.

### Lesson outline

- Overview: What is provenance?
- Capturing provenance
- Reusing provenance

### Lesson requirements

Although not essential it is helpful to have an understanding of:

- [W3C PROV data model](https://www.w3.org/TR/prov-dm/)
- Basic neuroimaging analysis steps

### Overview

_"Provenance is information about entities, activities, and people involved in
producing a piece of data or thing, which can be used to form assessments about
its quality, reliability or trustworthiness."_ (source: w3c)

Most current publications cannot be used to assess "quality, reliability or
 trustworthiness" of the results. Publications do not capture every detail of
the analysis steps, the data sources, and the metadata necessary to make such
assessments. The goal of provenance capture is to make such assessments
feasible and practical. Thus far we have focused on how to describe data,
computation environments, and analysis scripts. This lesson extends this to
relating this information in a form that allows capturing details of execution
steps, thus moving from specification to provenance.

### Capturing provenance
There are many ways in which one can record what steps are being carried out. A
general neuroimaging analysis involves many connected steps as outlined in our
[overview to this module](/01-overview). There are different tools that can
capture pieces of these steps. Like most research, not all analysis attempts end
in the results used for publication. Analysis often involves understanding the
data from different view points, using different routines to figure out what
works well and what does not, and changes in questions that are being asked of
the data. Thus capturing the provenance is a fluid process and will likely
require mastery of different tools.

#### Command line tools
The Unix shell has several tools for capturing the history of commands and
details of execution.

1. **`script`**. Here is [a basic introduction](http://www.livefirelabs.com/unix_tip_trick_shell_script/aug_2003/08252003.htm)
2. **strace, ptrace, dtrace**. Here is an introduction to [strace](http://www.brendangregg.com/blog/2014-05-11/strace-wow-much-syscall.html).
For a comparison of tools for tracing the system, see [this blog post](http://www.brendangregg.com/blog/2015-07-08/choosing-a-linux-tracer.html)

#### External tools for recording history
There are also more complex history recording tools that export information in
a structured form or deposit into a database.

- [Reprozip](https://vida-nyu.github.io/reprozip/)
- [Sumatra](http://neuralensemble.org/sumatra/)
- [asciinema](https://asciinema.org/)

### Reusing provenance
The previously mentioned tools capture history in different forms and therefore
support different aspects of reproducible research. The command line tools like
`script`, `strace`, and `asciinema` capture information that needs to be looked
at by a user to determine which commands to re-execute. However, these tools do
not capture the environment or data files. Reprozip can capture the entire
environment, data, and software necessary for repeating a previous analysis, and
therefore provides more complete reproducibility. However, reprozip only runs on
Linux systems, while some of the other tools can be used on any Unix-like system
 (e.g. Mac OS, GNU/Linux)




