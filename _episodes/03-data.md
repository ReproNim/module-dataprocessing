---
title: "Lesson 2: Annotate, harmonize, clean, and version data"
teaching: 15
exercises: 30
questions:
- "How to work with data of different types?"
objectives:
- Learn to annotate, harmonize, clean, and version data
- Become familiar with standards and technologies that preserve data
keypoints:
- What different file formats store and knowing where to find information
- Using standards to simplify harmonization
- buga
---

> ## You can skip this lesson if you can answer these questions? --->
>
>  - What are the different formats used to store structural imaging data?
>  - 
{: .challenge}

This lesson ...

### Lesson outline

- ... 

### Lesson requirements

Although not essential it is helpful to have an understanding of:
 
- ... 


### Data harmonization/standardization

- What are the different datatypes used in brain imaging studies
- What formats are used to store them.

- Behavioral/clinical datatypes
   - Neuropsychological and clinical assessments
       - CSV
       - TSV
       - JSON
   - Physiological measurements, behavioral responses
- MRI datatypes
   - 2D/3D/4D Images -  MRI/PET/CT scans
       - Formats
           - DICOM, Multiframe DICOM
           - NifTi1/2
               - CIFTI2
           - FreeSurfer formats (mgz, annot, curv)
           - BRIK/HEAD
           - MINC2
           - Nrrd
   - Spatial coordinate systems
       - MNI
   - Transforms
       - SPM/FSL/FreeSurfer mat files
       - Formats
          - HDF5
          - Text
          - Matlab
- GWAS datatypes
   - https://software.broadinstitute.org/software/igv/FileFormats
- EEG/MEG datatypes
   - Physiological measurements
   - Formats
       - vmdk
- Metadata dictionaries
   - Formats
       - json/ld

### Data and metadata standards

- NDA https://ndar.nih.gov/training.html
- RedCap https://redcap.vanderbilt.edu/consortium/videos.php 
- NIDM http://nidm.nidash.org/getting-started/
- BIDS http://www.nature.com/articles/sdata201644
- BDBAG http://bd2k.ini.usc.edu/tools/bdbag/

### Data wrangling and clean up

- What is data wrangling? https://en.wikipedia.org/wiki/Data_wrangling
- Using OpenRefine to harmonize tables http://www.slideshare.net/louislibraries/data-wrangling-with-open-refine
- Best practices for sharing data on the Web https://www.w3.org/TR/dwbp/ 
- Harmonizing imaging data: 
   - dicom data to BIDS: https://github.com/nipy/heudiconv

### Data versioning

- Git Annex/DataLad: http://datasets.datalad.org/?dir=/openfmri
