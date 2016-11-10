---
title: "Lesson 1: Core concepts using an analysis example"
teaching: 15
exercises: 30
questions:
- "What are the different considerations for reproducible analysis?"
objectives:
- Learn core elements of reproducible analysis
- Familiarize with various technology terms
keypoints:
- Reproducible analysis is technologically possible
- Learning these technologies can help produce more reliable research output
- Using such frameworks provide a better way to communicate information to colleagues and collaborators
---

> ## You can skip this lesson if you can answer these questions? --->
>
>  - What are the four core elements of a reproducible analysis?
>  - Why should you annotate your data in a machine accessible manner?
>  - Why should you use version control for data and code?
>  - What are some ways to share your analysis environment with others?
>  - What does continuous integration help you with?
{: .challenge}

This lesson uses the [Simple Workflow](https://github.com/repronim/simple_workflow)
Github repository to illustrate core concepts of reproducible analysis
and pitfalls associated with complex data, software, and computing
environments.

### Lesson outline

- Overview of the workflow
- <a href="#element1">Element 1: Storing data and metadata</a>
   - CSV, JSON/LD, BIDS, NIDM-E 
- Element 2: Creating a reproducible execution environment
   - Virtual machines, Containers, Scripts
- Element 3: Running analysis and storing expected results and provenance
   - Determine which execution outputs constitute test 
- Element 4: Checking output consistency using continuous integration
   - Couple with testing services such as circle or travis
    (or setup your own jenkins)
- Results from running the Workflow

### Lesson requirements

Although not essential it is helpful to have an understanding of:
 
- Version control (See git/github lesson in module X)

### Overview of workflow
This basic workflow extracts a collection of brain images and associated 
phenotypic (e.g., age) information from a spreadsheet, and runs a Nipype
workflow that takes the anatomical brain images and performs some simple
anatomical image processing, 

### Element 1: Storing data and metadata
To ensure reproducibility all data and metadata must be accessible and 
preferably machine accessible. 

> ## Machine accessibility
> A typical approach to describing research is to write a document that
> one shares with colleagues and collaborators. However, such 
> information requires significant human resources to interpret and 
> translate into code. An alternate approach is to encode the metadata
> using structured markup (RDF, JSON, XML). Often such markup can be 
> standardized to provide machine accessibility.
{: .callout}

In this example the data and metdata are stored in a [google spreadsheet]().
TODO - The column headers are described in detail in a JSON document 
using JSONLD a format that supports semantic annotation. The annotation 
provides information about the data contained in the column and allows 
harmonizing the information with other similar tables. The phenotypic 
information are stored as literals. The imaging data are stored as 
pointers to files in the NITRC XNAT repository. For example, the JSONLD 
metadata key tells us that the URLs correspond to anatomical T1-weighted 
images of the human brain and that the age of participants is in years. 
Note, that the same information can be transformed into existing 
standards:
 
1. A filesystem-based BIDS dataset with limited metadata
2. A Semantic Web-based NIDM experiment object with all metadata

By providing consistent structures these standards make the data more 
accessible. 

Lesson 2 covers many different aspects of data annotation, 
harmonization, cleaning, storage, and sharing.

### Element 2: Creating a reproducible execution environment
The second component of this example is a setup script that creates the
necessary computational environment for analysis. 

> ## Problems with creating environments
> 1. The default script assumes that you have access to certain software, 
>   such as bash and FSL, on your system. This means you have to run 
>   this on a unix-like system such as Linux or MacOS. 
> 2. All other software, python libraries and their dependencies are 
>   installed by the script. The script itself does not care if these 
>   installs conflict with your existing software environment.
{: .callout}

Alternatives that reproduce environments with minimal software 
dependencies are technologies like virtual machines (virtualbox, vmwre, 
NITRC CE), containers (docker, singularity) and installers (e.g., 
vagrant, ansible). These can be very useful to replicate existing 
environments and therefore simplify the installation problem 
significantly. However, at present some of these technologies are not 
installed by default on computing clusters you may have access to, 
although that is changing.

Lesson 3 covers container technologies and how to create, use, modify, 
and reuse containers.

### Element 3: Running analysis and storing expected results and provenance

Once the environment is setup, one can execute the analysis. Each time
the analysis is run the provenance of the workflow is captured and 
stored using a PROV model for workflows. All of this happens inside a 
single executable script. The script itself uses the Nipype dataflow 
framework to ensure a consistent representation of the execution graph
using Python as the dataflow language, and therefore benefits from all
the advantages a dataflow framework brings to analyses.

> ## Using dataflow technologies for analysis instead of shell scripts
> There are many dataflow platforms out there. These typically enable a
> compact, abstract graph based representation of a dataflow, allowing
> reuse and consistent of execution. They also enable running the same
> dataflow in different computing environments and not requiring the 
> user to keep track of complex data dependencies across nodes. While 
> Nipype was used in this example, other brain imaging data flow systems
> include Automated Analysis, PSOM, FASTR.
{: .callout}

Running the analysis is one part of reproducibility. It is important to 
also capture the output necessary for scientific hypothesis testing or
exploration. In this example, the volumes of subcortical structures and 
of the different brain tissue classes are extracted and stored in a JSON
document. A specific run of this workflow on a specific platform was 
used to create the provnenace document and the expected outputs data. 
When another user runs this workflow, their output can be compared to 
the expected to output.

Lesson 4 covers data flow technologies, specifically how to create
analysis pipelines and applications and capture provenance when running 
these pipelines. 

### Element 4: Checking output consistency using continuous integration

Once the data and environment are setup appropriately and the analysis 
is run, it would be good to know if the same results are obtained when
a different dataset containing the same data or a slightly different 
workflow is used. These can be carried out using continuous integration
services, such as Travis, CircleCI, Jenkins, which allow executing 
an analysis and performing a test comparison automatically as versions 
of data or sofware change.

> ## Continuous integration testing
> In typical brain imaging analyses there is a complex interaction 
> between data, software, and scientific hypothesis testing results. 
> Continuous integration services ensure that such results can be 
> obtained consistently and provide a framework to evaluate when results
> diverge. While typically used for software testing, continuous 
> integration has become essential for process management by creating a 
> complete test cycle.
{: .callout}

In brain imaging, most published results rarely come with data and code 
to allow retesting the outcome when either the software versions change
or when a new dataset is available. The intent of this simple workflow
framework is to move the community towards such comprehensive data 
preservation and testing integration.

Lesson 5 covers how to use continuous integration services like Travis 
and CircleCI, but also how container technologies can be used to run 
your own integration testing.

### Results from running the Workflow

It turns out that this Workflow is not reproducible across different 
versions of software and operating system. The observed inconsistencies 
point to issues of randomization within the algorithms that are run. 
While its easy to detect deviation of execution in different 
environments it is harder to determine the cause of the deviation. This
is where rich provenance capture can help establish where along an
execution graph an analysis diverged and help zero in on the possible 
culprits.

Lesson 6 covers details of how provenance can be captured.
