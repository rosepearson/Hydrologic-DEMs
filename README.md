# GeoFabrics

## Build Status

![Linux](https://github.com/rosepearson/GeoFabrics/actions/workflows/python-test-package.yml/badge.svg)

## Introduction

The GeoFabrics and associated sub-packages include routines and classes for combining point (i.e. LiDAR), vector (i.e. catchment of interest, infrastructure) and raster (i.e. reference DEM) to generate a hydrologically conditioned raster. 

### Fetching geospatial data from web APIs
GeoFabrics also contains support for downloading all LiDAR tiles from OpenTopography, or a particular vector layer from the LINZ data service. In both cases this data is only downloaded within a spectified polygon (catchment region). This functionality is contained in the lidar_fetch module for LiDAR, and the vector_fetch model for vector data. It can be used independently of the other geofabrics modules (aside from a slight gepency on the geometry module) and will be separated into an independent package in issue [23](https://github.com/rosepearson/GeoFabrics/issues/23). Example of its usage can be found in tests/test_lidar_fetch and tests/test_vector_fetch.

## Requirements
### Required Software
See the `environment_windows.yml` or `environment_linux.yml` depending on your operating system under root for a list of package dependencies.

### Required Credentials
You will need a https://data.linz.govt.nz/ login and API key to pull vector data from the LINZ data service, or to run the tests locally on your machine. Create a login and navigate to https://data.linz.govt.nz/my/api/ to create an API.

## Import into a conda environment
You can use this package by using pip to install the package and dependencies using the following commands in a conda terminal to add it to that environment, where you must either specify `environment_windows.yml` or `environment_linux.yml` depending on your operating system. Each file is located in the root repository folder. Sorry there is no macOS support at this stage.

```bash
conda env create -f environment_[windows|linux].yml
conda activate geofabrics
pip install git+https://github.com/rosepearson/GeoFabrics
```

## Basic instructions for use
The GeoFabricsGenerator class is the main entry point. This can either be accessed by importing the package then using the class directly, or through the main.py script in the root src directory. 

### Importing GeoFabrics
Basic code stub looks like:
```python
from geofabrics import processor
import json
with open(r'path\to\file\\', 'r') as file_pointer:
            instructions = json.load(file_pointer)
runner = processor.GeoFabricsGenerator(instructions)
runner.run()
```

### Running main.py script
In the conda environment defined in the root\environment.yml, run the following:

`python src\main.py --instructions full\path\to\instruction.json`

## Tests
Tests exist for stand alone functionality (i.e. fetch lidar), and complete processing chain (i.e. creating a DEM from LiDAR files within a shapefile). A 'benchmark_dem.nc' is uploaded for each test when a DEM is generated. This is stored using git LTS as these file are not human readable. 

### Automated testing
[Github Actions](https://docs.github.com/en/actions) are used to run tests after each push to remote (i.e. github). [Miniconda](https://github.com/marketplace/actions/setup-miniconda) from the GitHub Actions marketplace is used to install the package dependencies. Linting with [Flake8](https://github.com/py-actions/flake8) and testing with [PyTest](https://docs.pytest.org/en/6.2.x/contents.html) is then performed. Several tests require a API key. This is stored as a GitHub secrete and accessed by the workflow.

Check the actions tab after you push to check if your tests run successfully.

### Running tests locally
Several tests (test_vector_fetch, and test_processor_remote_all_westport) require a LINZ API token. Create an API token using the instructions under the **Requirements** section then create an `.env` file under root and copy the key there as follows:
`LINZ_API="COPY_YOUR_API_KEY_HERE"`

In the conda environment defined in the root\environment_[windows|linux].yml, run the following in the repository root folder:

1. to run individual tests
`python -m tests.test_processor_local_files.test_processor_local_files` or `python -m tests.test_lidar_fetch.test_lidar_fetch` etc.

2. to run all tests
`python -m unittest`

## Spyder IDE setup
If you are using spyder you can make the following changes to run main and the tests using Run (F5)

#### Running main

Go to 'Run>Configuration per file...' and check the **Command line options** under **General settings**. Enter the following:

`--instructions full\path\to\your\instruction.json`

See image below: 

![image](https://user-images.githubusercontent.com/22883860/127434946-782f3a51-ea8c-485e-baea-3bd6b33ee39a.png)

### Running a test

Go to 'Run>Configuration per file...' and check the **The following directory** under **Working directory settings** and specify the root of the repository with no quotes. Do this for each Python test file (usually one per test folder)

`full\path\to\the\repository\root`

See image below: 

![image](https://user-images.githubusercontent.com/22883860/127434795-6d3848fb-6b2f-4ddf-ad4b-2450722f2f8d.png)

## Related material
Scripts used as part of the development of this project are included in a separate repo. See https://github.com/rosepearson/Hydrologic-DEMs-scripts for an example.

## Contributions
Please see our [Issue Tracker](https://github.com/rosepearson/GeoFabrics/issues) for details on coming features and additions to the software.

There is no current expectations of contributions to this project. We accept input in code reviews now. If you would like to be involved in the project, please contact the maintainer.

## Contacts
Maintainer: Rose Pearson @rosepearson rose.pearson@niwa.co.nz

## License
[GNU GPL](https://github.com/rosepearson/GeoFabrics/LICENSE)
