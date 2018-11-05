# OASIS Loss Modelling Platform

## Install
Reference:
https://github.com/OasisLMF/OasisPlatform

### Step 1: Prerequisites
- [Install Docker with Compose](linux-docker.md)

### Step 2: Local Build

```
mkdir oasis
cd oasis
git clone https://github.com/OasisLMF/OasisPlatform.git
cd OasisPlatform

docker build -f Dockerfile.oasis_api_server -t oasis_api_server .
docker build -f Dockerfile.model_execution_worker -t model_execution_worker .
docker build -f Dockerfile.api_runner -t api_runner .

docker-compose up -d
```

### Step 3: Intall KTOOLS
Reference: https://github.com/OasisLMF/ktools

```
cd ~
cd oasis
git clone https://github.com/OasisLMF/ktools.git

docker build -f Dockerfile.ktools -t ktools .
docker build --file Dockerfile.ktools.alpine -t alpine-ktools .
```
Example of usage:
```
docker run ktools coveragetocsv < ./examples/input/coverages.bin > tmp_coverages.csv
```
