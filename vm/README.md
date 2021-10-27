# VictoriaMetrics
## Setup
### Create google cloud project

Create Service Account and download json key

activate compute engine

### Allow VictoriaMetrics traffic

Open ports 8428 and 8429 in the firewall

### Install google cloud sdk

See [installation docs](https://cloud.google.com/sdk/docs/install)

`gcloud auth activate-service-account --key-file=microbenchmarkevaluation-275929759504.json`

### Install golang and graphviz

`sudo apt-get install golang`

`sudo apt-get install graphviz`

### Clone this project
git clone ...

### Install dependencies for notebooks
https://pygraphviz.github.io/documentation/stable/install.html
https://pip.pypa.io/en/stable/installing/
run: `pip install -r requirements.txt`

## Find microbenchmark suite(s)

### Generate application benchmark call graph
adjust 0_main.sh in /findSuites and enter the base commit

run 0_main.sh three times and save the generated .pprof and .dot file for each run manually
