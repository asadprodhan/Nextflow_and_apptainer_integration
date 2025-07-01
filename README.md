# Your Project Name

[![Nextflow](https://img.shields.io/badge/Nextflow-enabled-brightgreen.svg)](https://www.nextflow.io/)
[![Apptainer](https://img.shields.io/badge/Apptainer-container-blue.svg)](https://apptainer.org/)

---

## Overview

This project uses **Nextflow** for workflow management and **Apptainer** (formerly Singularity) for containerized execution to ensure reproducibility and portability of bioinformatics analyses.

The pipeline includes running MrBayes for phylogenetic analysis using containerized environments managed seamlessly by Nextflow.

---

## Table of Contents

- [Prerequisites](#prerequisites)  
- [Installation](#installation)  
  - [Java](#java)  
  - [Nextflow](#nextflow)  
  - [Apptainer](#apptainer)  
- [Configuration](#configuration)  
- [Running the Pipeline](#running-the-pipeline)  
- [Pipeline Structure](#pipeline-structure)  
- [License](#license)  
- [Contact](#contact)  

---

## Prerequisites

- Linux operating system (tested on Ubuntu 20.04+)
- Java 17 or later
- Bash shell
- Internet access for container downloads

---

## Installation

### **Java**

Ensure Java 17 or later is installed:

```
java -version
```


If not installed, on Ubuntu:

```
sudo apt update
sudo apt install openjdk-17-jdk
```


### **Nextflow**


Install Nextflow via official installer script:


```
curl -s https://get.nextflow.io | bash
```

```
mv nextflow ~/bin/
```

> or any directory in your $PATH

Verify:

```
nextflow -version
```


### **Apptainer**

Apptainer replaces Singularity with active support and fixes.

Remove any conflicting Singularity packages:

```
sudo apt-get remove --purge singularity-container
sudo apt --fix-broken install
```


Download Apptainer .deb packages

Get the latest version from Apptainer releases, for example:

apptainer_1.4.1_amd64.deb

apptainer-suid_1.4.1_amd64.deb

Install packages:


```
sudo dpkg -i apptainer_1.4.1_amd64.deb apptainer-suid_1.4.1_amd64.deb
```

Configure Apptainer

Create config directory and config files with proper permissions:

```
sudo mkdir -p /etc/apptainer
sudo tee /etc/apptainer/apptainer.conf > /dev/null <<EOF
allow setuid = yes
enable overlay = yes
mount proc = yes
mount sys = yes
mount dev = yes
mount devpts = yes
mount home = yes
mount tmp = yes
mount hostfs = yes
EOF

sudo touch /etc/apptainer/capability.json /etc/apptainer/ecl.toml
sudo chown root:root /etc/apptainer/{capability.json,ecl.toml}
sudo chmod 644 /etc/apptainer/{capability.json,ecl.toml}
```

Test Apptainer


```
apptainer exec docker://alpine echo "Hello Apptainer"
```


### **Configuration**

Configure Nextflow to enable Apptainer container support by editing nextflow.config:


```
singularity {
    enabled = true
    autoMounts = true
    cacheDir = "$PWD/singularity"
    // path = '/usr/bin/apptainer'  // uncomment if Apptainer not in PATH
}

process {
    withName: mrbayes {
        container = "$HOME/.singularity/images/mrbayes_3.2.7.sif"
        cpus = 2
        memory = '2 GB'
        time = '1h'
    }
}

```


Optional: Pre-pull container images for faster startup and fewer runtime errors:


```
mkdir -p ~/.singularity/images
apptainer pull ~/.singularity/images/mrbayes_3.2.7.sif docker://quay.io/biocontainers/mrbayes:3.2.7--h760cbc2_0
```


Running the Pipeline

Run your pipeline with Nextflow:


```
nextflow run main.nf -with-singularity
```


Or specify the container image manually if needed:

```
nextflow run main.nf -with-singularity $HOME/.singularity/images/mrbayes_3.2.7.sif
```


