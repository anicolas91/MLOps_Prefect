# MLOps Zoomcamp 2023 Week 3

![Prefect logo](./images/logo.svg)

---

This repo contains Python code to accompany the videos that show how to use Prefect for MLOps. We will create workflows that you can orchestrate and observe..

# Setup

## Clone the repo

Clone the repo locally.

## Install packages

In a conda environment with Python 3.10.12 or similar, install all package dependencies with 

```bash
pip install -r requirements.txt
```
## Start the Prefect server locally

Create another window and activate your conda environment. Start the Prefect API server locally with 

```bash
prefect server start
```



## Optional: use Prefect Cloud for added capabilties
Signup and use for free at https://app.prefect.cloud


## NOTES:
As of june 2024 the working Prefect version is 2.19.4, which has led to the following changes on this repository:
- requirements.txt has been updated to install the latest prefect version
- Since the concept of 'projects' has been deprecated since june of 2023 (see [here](https://github.com/PrefectHQ/prefect/issues/9860)), we are adapting the process to follow the latest tutorial [here](https://docs.prefect.io/latest/tutorial/)