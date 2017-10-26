---
title: "Lesson 2: Annotate, harmonize, clean, and version data"
teaching: 60
exercises: 60
questions:
- "How to work with and preserve data of different types?"
objectives:
- Learn to annotate, harmonize, clean, and version data
- Become familiar with standards and technologies that preserve data
keypoints:
- What different file formats store and knowing where to find information
- Using standards to simplify harmonization
---

> ## You can skip this lesson if you can answer these questions? --->
>
>  - Do you know how to convert your DICOM data to BIDS?
>  - Can you create a data dictionary for your own data collection instrument?
>  - Do you know what coordinate system is used in your raw data?
>  - Do you know how to use DataLad to version data?
{: .challenge}

This lesson focuses on how you can preserve data for yourself and for 
your collaborators. In particular, understanding the technologies in this lesson
will help you maintain and communicate information about your dataset explicitly.

### Lesson outline

- [Information stored in different types of data used in brain imaging research](#datatypes)
- [Different data standards to capture information in structured form](#standards)
- How to clean data
- How to store and publish data using versioning

### Lesson requirements

Although not essential it is helpful to have an understanding of:
 
- How brain imaging data are exported from a scanner
- How to use a software tool (e.g., AFNI, FSL, SPM) to process data
- [Git](http://swcarpentry.github.io/git-novice/)
- [Git-Annex](https://git-annex.branchable.com/)

<a name="datatypes" />

### Information stored in different data types in brain imaging

Brain imaging covers a diverse set of data acquisition instruments including 
MRI scanners, physiological measurements, cognitive tests, and clinical 
interviews. Further, different analysis software often have their own data 
formats. Therefore, to ensure information is preserved for reproducible 
dataprocessing, it is important to understand the different types of information 
stored in each format.

#### Data
This section familiarizes you with the different types of data generated or used
in brain imaging research and the different formats used to store them.

1. **Participant data:** All imaging experiments store some information 
about participants. These include demographic data, data from 
neuropsychological and clinical assessments, and phsyiological measurements. 
Typically such data are stored in CSV, TSV, or [JSON](json.org) files. But some
types of measurements such as eye-tracking, respiration, GSR, may be stored in 
proprietary file formats. In many cases data are also stored in Excel 
spreadsheets or databases (e.g., RedCap, COINS, XNAT, LORIS, etc.,.). 

It is important to consider what information is stored in each file or database 
before converting or extracting the information. This includes understanding the 
variables that are represented, their datatypes (string, integer, float, double),
 their range  (e.g., age > 0) or set membership (e.g., diagnosis part of DSM-V 
or ICD-10), and their units (e.g., years, mm).

2. **MRI data:** Most scanners output data in [DICOM](http://dicom.nema.org/standard.html) 
format. However, DICOM supports any medical imaging device and can store 2D, 3D,
and 4D data and associated metadata. Given the diversity of devices, DICOM has 
to cover an extensive array of metadata. Since much of the metadata was not 
relevant to existing imaging analysis tools, most brain imagers typically 
convert data to the NifTi format. However, across brain imaging there are many
different data formats depending on the software being used. It is important to 
note that most of these formats store data in binary form and cannot be 
introspected like a text file. You will need to use specialized software to 
introspect these files.

   ```
   - DICOM, Multiframe DICOM
   - NifTi1/2 (Most imaging tools)
   - CIFTI2 (Connectome Workbench)
   - MGH, MGZ, ANNOT, CURV, *.pial (FreeSurfer) formats (mgz, annot, curv)
   - BRIK/HEAD (AFNI)
   - MINC2 (MNI Tools)
   - Nrrd (Slicer/ITK community)
   - Analyze (Mostly not in use any more)
   ```

All of these formats store metadata (e.g., coordinate space information, resolution, 
etc.,.) in the **header** component of the file and the 3D spatial intensity or 4D
time series data in the **binary** component of the file. Each format uses their
own definition for the header and store different types of information. This reduces
the ease of converting from one type to another and in harmonizing the metadata.
   
3. **Coordinate systems:** An MRI scan is an image of a physical object in 3D space. 
Therefore, brains of different participants even within the same scanner may be
in different physical positions. The DICOM files contain information describing
the orientation of the image slices or volumes with respect to the scanner. 
Similarly, EEG setups often use the [10-20 system](https://en.wikipedia.org/wiki/10%E2%80%9320_system_(EEG)).
These systems help relate physical positions of the participant to a common 
reference frame that can be understood by analysis software.

4. **Other types of data.** Brain imaging experiments may also include genetic data,
electrophysiology recordings (such as EEG, MEG, ECoG). Most of the raw data are
stored in proprietary formats depending on the device used to acquire the data.

> ## Metadata representation
> A common issue with many such files is that the column headers and keys are 
> often not described adequately. For example, if a column says Age, does it 
> mean age in years, weeks, or months. Was it precise age at enrollment or 
> during one of the data collection visits. To make data reusable, it is 
> important to create a data dictionary to describe each field in data file. 
> Examples of such data dictionaries can be found in the 
> [NIH Data Archive](https://ndar.nih.gov/data_dictionary.html?type=All&source=All&category=All).
> For those who use CSV files, this [metadata editor](http://data.wu.ac.at/csvengine/csvm/editor)
> may be a good starting point.
{: .callout}

> ## Coordinate systems and transformations
> Understanding coordinate systems and transformations is essential when using 
> the various software tools. Most brain imaging analysis requires transforming
> individuals into a common frame of reference that also creates correspondences
> between each brain.
{: .callout}

> ## Exercise:
>
>  List 3 examples of information lost when converting from DICOM to NifTi?
>
> > ## Solution
> >
> > The NifTi file keeps an extremely small subset of the DICOM metadata, unless 
> > specific converters are used that store DICOM metadata in extensions. While
> > this is often useful for anonymization of metadata, it loses most sequence 
> > related information, when that is available from dicoms. Three specific 
> > examples are: Number of channels in head coil, slice timing information, and
> > phase encoding direction. 
> > 
> {: .solution}
{: .challenge}

<a name="standards" />

### Data and metadata standards

#### NIMH Data Archive (NDA)

The NIMH data archive accepts data from several federally funded initiatives. To
submit data to the NDA, your data needs to be converted to specific types using
their data dictionaries. To learn how to submit data to the NDA view the 
[training videos](https://ndar.nih.gov/training.html). You can simplify the 
submission process by storing your data in [BIDS](bids.neuroimaging.io) format.

> ## Hands on Exercise:
>
> Use the [NDA api](https://ndar.nih.gov/swagger/#/) to search for the different
> sources and categories of information in the NDA.  
>
> > ## Solution
> >
> > Click on datadictionary and then use the two GET operations:
> >
> > 1. GET /datadictionary/v2/datastructure/categories
> > 2. GET /datadictionary/v2/datastructure/sources
> >
> > If you set application type to json, the results will be returned in JSON 
> > format
> > 
> {: .solution}
>
> Use the [NDA api](https://ndar.nih.gov/swagger/#/) to retrieve the data
> dictionary for mini mental state exam. What are some of the drawbacks of this
> dictionary. 
>
> > ## Solution
> >
> > The dictionary uses undescriptive keys to refer to different components of 
> > the exam and enforces encoding certain responses in a specific way.
> > 
> {: .solution}
{: .challenge}

#### RedCap 

RedCap allows investigators to electronically capture participant data using 
online surveys or through manual curation of information into a RedCap form. 
RedCap also allows reusing existing forms to already ensure that the data are
stored using consistent keys. Learn how to use RedCap using these [training 
videos](https://redcap.vanderbilt.edu/consortium/videos.php). 

#### Brain Imaging Data Structure (BIDS) 

[BIDS](bids.neuroimaging.io) is a community developed file organization and 
naming scheme that makes it easier to publish data and share it with others in a
consistent format. [This paper](http://www.nature.com/articles/sdata201644) 
provides an overview of the BIDS schema.

#### Neuro Imaging Data Model (NIDM)

The Neuro Imaging Data Model (NIDM) captures brain data, workflow, and results 
in a structured format using a derivative of the W3C PROV Data Model. Currently,
results of fMRI analyses conducted in FSL and SPM can be stored using 
NIDM-Results. These results can be uploaded to [NeuroVault](http://neurovault.org) 
for sharing with others. To understand how you can use NIDM you can start
[here](http://nidm.nidash.org/getting-started/).


### Data curation and publishing

The data formats and standards described earlier allow users to store and 
represent data in structured form. If everyone adopts a structured format 
standard, data will be much easier to aggregate, comprehend, and analyze. 
However, in the absence of complete community agreement and different efforts
targeted at different levels of comprehensiveness, there is a lot of data that
falls through the cracks in terms of clarity. In this section we focus on two
approaches that allow users to take existing data and convert them to one of the
structured forms described above.

#### Data Wrangling

[Data wrangling](https://en.wikipedia.org/wiki/Data_wrangling) "is loosely 
defined as the process of manually converting or mapping data from one "raw" 
form into another format that allows for more convenient consumption of the 
data with the help of semi-automated tools."

1. [Using OpenRefine to harmonize tables](http://www.slideshare.net/louislibraries/data-wrangling-with-open-refine)
2. [Converting DICOM data to BIDS](https://github.com/nipy/heudiconv)
3. [Validating BIDS](http://incf.github.io/bids-validator/)
4. [Converting BIDS to NDA](https://github.com/INCF/BIDS2NDA)
5. [RedCap to NDA](https://github.com/BANDA-connect/NDA-sprint/tree/master/REDCap2NDA)

#### Best practices for data publishing

[The best practices for sharing data on the Web](https://www.w3.org/TR/dwbp/) is
a proposed recommendation for the W3C. The [summary section](https://www.w3.org/TR/dwbp/#bp-summary)
provides a set of clear goals that make data more useful.

### Data versioning

Just like software and text can be versioned, data can be versioned as well. This
section highlights a few technologies that can be used to version data.

1. DropBox, Google drive, Box, AWS S3 (with versioning turned on). Each of these 
cloud providers have the ability to version your data. Unlike using filenames
for versioning (e.g., file_v1.txt, file_170129.txt), these systems use the 
content of the file to determine if there has been a change, and stores a new
version. These providers can do so for any type of file. However, just having 
versions does not tell you what has changed. This is easier for text type files
(e.g., csv) but harder for binary files. In general, no tool exists that 
describes differences across two versions of a brain imaging file. Some work is
being done with NIDM to describe differences between two binary files based on 
their metadata.

2. [Git Annex/DataLad](http://datalad.org). Another approach to versioning is to
use Git, the same versioning system that was created to track versions of the 
LINUX development effort and is now used across millions of software packages. 
GitHub provides a user interface to track to track text files, analysis scripts, 
and even small binary data files. Edits to analysis scripts should be versioned
and GitHub can be used to do so. To learn more about datalad see
[module Reproducible Basics:VCS:DataLad](http://www.reproducibleimaging.org/module-reproducible-basics/02-vcs/#datalad)
unit.

> ## Hands on Exercise:
>
> Numerous datasets are available from the [datalad index](http://datasets.datalad.org/). 
> How many Openfmri datasets are currently available and what file layout scheme
> is used to store the datasets?
>
> > ## Solution
> >
> > There are 80 datasets available as of Aug 7, 2017. These datasets are all
> > stored using the BIDS layout. 
> > 
> {: .solution}
{: .challenge}


[Learn how to use GitHub to track your analysis](https://guides.github.com/)

After learning how to us git and GitHub to track code, the next step is to use a 
very similar mechanism to track data. DataLad is an effort that makes tracking,
publishing and sharing brain imaging data easier. [Public datasets available using datalad](http://datasets.datalad.org/)
