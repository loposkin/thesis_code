# Using Optimized MicrobenchmarkSuites and Application Benchmarks to Detect Performance Regressions in CI/CD Processes

## Prerequisites

### Create google cloud project

Create Service Account and download json key 

activate compute engine

### Allow VictoriaMetrics traffic

Open ports 8428, 8429, 80, and 81 in the firewall


### Install google cloud sdk

`echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list`

`sudo apt-get install apt-transport-https ca-certificates gnupg`

`curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -`

`sudo apt-get update && sudo apt-get install google-cloud-sdk`

`gcloud auth activate-service-account --key-file=your-gcloud-key-name.json`

### Install additional software
`sudo apt-get install golang graphviz nmap`

### Setup python and install python requirements (to use jupyter notebooks)

`sudo apt-get install python pip`

`python3 -m venv env`

`source env/bin/activate`

`pip3 install -r requirements.txt`

## Experiment
Ensure all the scripts in the subdirectories are executable before proceeding

### Create instance templates
Create a template named `microbench-standard-2` with machine type `e2-standard-2 (2 vCPUs, 8 GB Memory)` with Ubuntu 20.04 and 30 GB boot disk.

Create a template named `microbench-standard-disk` with machine type `e2-standard-2 (2 vCPUs, 8 GB Memory)` with Ubuntu 20.04, a 30 GB boot disk, and an additional 20GB disk.

### Create data image

`cd createDataImage`

`./0_main.sh`

### Create an instance template for application benchmark

Create a template named `microbench-standard-2` with machine type `e2-standard-2 (2 vCPUs, 8 GB Memory)` with Ubuntu 20.04, 30 GB boot disk, and an additional 20GB disk created with an image from previous step(`tsbs-inserts`)

### Run application benchmark

`cd appBenchmark`

`./0_main.sh <start_commit_included> <end_commit_excluded> <run_number>`

### Run microbenchmarks

`cd microBenchmark`

`./0_main.sh <start_commit_included> <end_commit_excluded> <run_number>`

### Create call graph for the application benchmark

`cd cgApp`

`./0_main.sh <start_commit_included> <end_commit_excluded> <run_number>`

### Optimize the call graphs

Note: you need gocg tool for this step

`bitbucket.org/sealuzh/gocg/cmd/transform_profiles ../resultsVM/cgMicro/profiles ../resultsVM/cgMicro/dots dot:100000:0.000:0.000`

`bitbucket.org/sealuzh/gocg/cmd/overlap github.com/VictoriaMetrics/VictoriaMetrics ../resultsVM/cgApp/A ../resultsVM/cgMicro/dots ../resultsVM/cgOverlap`

`bitbucket.org/sealuzh/gocg/cmd/minimization github.com/VictoriaMetrics/VictoriaMetrics ../resultsVM/cgApp/A ../resultsVM/cgMicro/dots ../resultsVM/cgOverlap`

### Run optimized microbenchmark suite

`cd optimizedMicroBenchmark`

`./0_main.sh <start_commit_included> <end_commit_excluded> <run_number>`
