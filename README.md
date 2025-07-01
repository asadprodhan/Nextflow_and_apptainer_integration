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

### Java

Ensure Java 17 or later is installed:

```bash
java -version
