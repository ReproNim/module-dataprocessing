---
theme: gaia
_class: lead
paginate: true
_paginate: false
---
<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

## How would Repronim do data processing

---
<!--
footer: 'How would Repronim do data processing'
-->

![bg](white)
![width:1100px center](https://www.frontiersin.org/files/Articles/17185/fninf-06-00009-HTML/image_m/fninf-06-00009-g001.jpg)

---
### Outline

1. Heudiconv/ReproIn/BIDS
2. DataLad
3. Containerized apps

---
![bg](black)
![](white)

### BIDS

![width:1000px center](https://bids.neuroimaging.io/assets/img/dicom-reorganization-transparent-white_1000x477.png)

---

### Heudiconv/ReproIn

Convert dicoms

```shell
$ heudiconv --files phantom-1 -f reproin -o output --bids 
```

- What is HeuDIConv?
  - heuristic dicom converter

- What is ReproIn?
  - a protocol naming scheme

---
### ReproIn

![width:1100px](https://github.com/ReproNim/reproin/raw/master/docs/source/images/dbic-conversions.png)

---
### ReproIn

```shell
$ heudiconv --files phantom-1 \
-f reproin -o output2 \
--bids --datalad
```

![bg right](https://github.com/ReproNim/reproin/raw/master/docs/source/images/dbic-conversions.png)

---
### DataLad: finding data

[Datalad indexed datasets](https://datasets.datalad.org)

```shell
$ cd ~
$ datalad install https://datasets.datalad.org/openneuro/ds002311
```
what did we get?

```shell
$ ls 
$ cd ds002311
$ ls
$ cat dataset_description.json
$ mri_info sub-01/anat/sub-01_T1w.nii.gz 
```
---
### Datalad: let's play with some data

let's get some data

```shell
$ datalad get sub-01/anat/sub-01_T1w.nii.gz 
$ mri_info sub-01/anat/sub-01_T1w.nii.gz 
```

let's remove the data

```shell
$ datalad drop sub-01/anat/sub-01_T1w.nii.gz 
$ mri_info sub-01/anat/sub-01_T1w.nii.gz 
```

___
### Datalad - the master index

We have installed 
```shell
$ ls ~/datasets.datalad.org/abide/RawDataBIDS/

$ ls ~/datasets.datalad.org/openneuro
```

Now let's get some data

```shell
$ cd ~/datasets.datalad.org/
$ datalad install openneuro
$ ls

```

---

### DataLad: basics

| git | datalad |
| --- | --- |
| mkdir project; cd project; git init | datalad create project |
|git add file; git commit -am "my message" | datalad add file/folder |
|git manages a single git repo|datalad manages multiple git repos|

Where to learn: http://handbook.datalad.org/en/latest/

---

### DataLad for Processing and Provenance

Setting up a dataset

```shell
$ datalad create reprokwyk
$ cd reprokwyk
```

Adding an existing dataset to it

```shell
$ datalad clone --dataset . ///openneuro/ds000003
$ datalad get ds000003/sub-01/anat/sub-01_T1w.nii.gz
```

---

### Processing: Reusing software

Kwyk is a deep neural network based anatomical segmentation software. Let's add a Docker container for kwyk to the dataset

```shell
$ datalad containers-add -i kwyk-img \
-u dhub://neuronets/kwyk:latest-cpu kwyk
```
Now we can execute it

```shell
$ datalad containers-run -n kwyk \
--input ds000003/sub-01/anat/sub-01_T1w.nii.gz \
--output kwyk-output.nii.gz '{inputs}' '{outputs}'
```

---


### Datalad concepts


![width:600px center](https://handbook.datalad.org/en/latest/_images/dataset.svg)

---
### Datalad concepts

![width:600px center](https://handbook.datalad.org/en/latest/_images/local_wf_simple.png)

---
### Datalad concepts

![width:1024px center](https://handbook.datalad.org/en/latest/_images/collaboration.svg)

---

![height:640px center](https://raw.githubusercontent.com/ReproNim/containers-artwork/master/repronim-containers-yoda_30dpi.png)

---

<!--
_class: lead
-->
# Break

---
### Viewing the output

```shell
$ fsleyes ds000003/sub-01/anat/sub-01_T1w.nii.gz kwyk-output.nii.gz_means_orig.nii.gz
```

![width:925px center](https://dl.dropbox.com/s/5rdym7boij48xkd/Screenshot%202020-02-25%2013.52.09.png?dl=0)

---
### What have we done

```shell
$ git log
```

---

### What concepts have we covered:

- Finding and retrieving public data
   - datalad
- Reusing containers
   - containers
- Tracking the computations
   - datalad containers-run

---

### Where to go from here

- Collect data using ReproIn
- Reuse containers
  - https://bids-apps.neuroimaging.io/apps/
  - https://github.com/ReproNim/containers
- Learn more about [datalad](http://handbook.datalad.org/en/latest/)
- Publish your scripts
- Use dataflow technologies to create your scripts (Nipype, Boutiques, scikit-learn, tensorflow)
- Use neurostars.org to ask questions