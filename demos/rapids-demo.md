---
layout: default
nav_order: 1
parent: Demos
title: RAPIDS Demo Container
permalink: rapids-demo
redirect_from: "containers/rapids-demo" # redirect from old page to ensure existing links still work
---

# RAPIDS Demo Container

Get started with our preconfigured RAPIDS demo container, featuring several demo notebooks using cuDF, cuML, cuGraph, Dask, and XGBoost
{: .fs-6 .fw-300 }

1. TOC
{:toc}

### Current Version

#### RAPIDS {{ site.data.releases.stable.version }} - {{ site.data.releases.stable.date }}

Versions of libraries included in the `{{ site.data.releases.stable.version }}` images:
- `cuDF` [v{{ site.data.releases.stable.version }}](https://github.com/rapidsai/cudf/tree/v{{ site.data.releases.stable.version }}.0), `cuML` [v{{ site.data.releases.stable.version }}](https://github.com/rapidsai/cuml/tree/v{{ site.data.releases.stable.version }}.0), `cuGraph` [v{{ site.data.releases.stable.version }}](https://github.com/rapidsai/cugraph/tree/v{{ site.data.releases.stable.version }}.0), `RMM` [v{{ site.data.releases.stable.version }}](https://github.com/rapidsai/RMM/tree/v{{ site.data.releases.stable.version }}.0), `cuSpatial` [v{{ site.data.releases.stable.version }}](https://github.com/rapidsai/cuspatial/tree/v{{ site.data.releases.stable.version }}.0), `cuSignal` [v{{ site.data.releases.stable.version }}](https://github.com/rapidsai/cusignal/tree/v{{ site.data.releases.stable.version }}.0)
  - **IMPORTANT:** CUDA 10.0 & Python 3.6 support ended in v0.14; v0.15 includes CUDA 11.0 & Python 3.8 support
  - **NOTE:** See [RAPIDS Notices](https://docs.rapids.ai/notices) for release changes for `clx` & `cuxfilter` as well as other recent changes

### Former Version

#### RAPIDS {{ site.data.releases.legacy.version }} - {{ site.data.releases.legacy.date }}

Versions of libraries included in the `{{ site.data.releases.legacy.version }}` images:
- `cuDF` [v{{ site.data.releases.legacy.version }}](https://github.com/rapidsai/cudf/tree/v{{ site.data.releases.legacy.version }}.0), `cuML` [v{{ site.data.releases.legacy.version }}](https://github.com/rapidsai/cuml/tree/v{{ site.data.releases.legacy.version }}.0), `cuGraph` [v{{ site.data.releases.legacy.version }}](https://github.com/rapidsai/cugraph/tree/v{{ site.data.releases.legacy.version }}.0), `RMM` [v{{ site.data.releases.legacy.version }}](https://github.com/rapidsai/RMM/tree/v{{ site.data.releases.legacy.version }}.0), `cuSpatial` [v{{ site.data.releases.legacy.version }}](https://github.com/rapidsai/cuspatial/tree/v{{ site.data.releases.legacy.version }}.0), `cuxfilter` [v{{ site.data.releases.legacy.version }}](https://github.com/rapidsai/cuxfilter/tree/v{{ site.data.releases.legacy.version }}.0), `cuSignal` [v{{ site.data.releases.legacy.version }}](https://github.com/rapidsai/cusignal/tree/v{{ site.data.releases.legacy.version }}.0)
  - **NOTE:** `cuxfilter` is only available in `runtime` containers
- `xgboost` [branch](https://github.com/rapidsai/xgboost/tree/rapids-{{ site.data.releases.legacy.version }}-release), `dask-xgboost` [branch](https://github.com/rapidsai/dask-xgboost/tree/dask-cudf) `dask-cuda` [branch](https://github.com/rapidsai/dask-cuda/tree/branch-{{ site.data.releases.legacy.version }})

### Image Types

The RAPIDS images are based on [nvidia/cuda](https://hub.docker.com/r/nvidia/cuda), and are intended to be drop-in replacements for the corresponding CUDA
images in order to make it easy to add RAPIDS libraries while maintaining support for existing CUDA applications.

RAPIDS images come in three types, distributed in two different repos:

The [rapidsai/rapidsai](https://hub.docker.com/r/rapidsai/rapidsai/tags) repo contains the following:
- `base` - contains a RAPIDS environment ready for use.
  - **TIP: Use this image if you want to use RAPIDS as a part of your pipeline.**
- `runtime` - extends the `base` image by adding a notebook server and example notebooks.
  - **TIP: Use this image if you want to explore RAPIDS through notebooks and examples.**

The [rapidsai/rapidsai-dev](https://hub.docker.com/r/rapidsai/rapidsai-dev/tags) repo adds the following:
- `devel` - contains the full RAPIDS source tree, pre-built with all artifacts in place, and the compiler toolchain, the debugging tools, the headers and the static libraries for RAPIDS development.
  - **TIP: Use this image to develop RAPIDS from source.**

### Image Tag Naming Scheme

The tag naming scheme for RAPIDS images incorporates key platform details into the tag as shown below:
```
{{ site.data.releases.stable.version }}-cuda{{ site.data.versions.CUDA_VER }}-runtime-ubuntu18.04-py{{ site.data.versions.PYTHON_VER }}
 ^       ^    ^        ^         ^
 |       |    type     |         python version
 |       |             |
 |       cuda version  |
 |                     |
 RAPIDS version        linux version
```

To get the latest RAPIDS version of a specific platform combination, simply exclude the RAPIDS version.  For example, to pull the latest version of RAPIDS for the `runtime` image with support for CUDA {{ site.data.versions.CUDA_VER }}, Python {{ site.data.versions.PYTHON_VER }}, and Ubuntu 18.04, use the following tag:
```
cuda{{ site.data.versions.CUDA_VER }}-runtime-ubuntu18.04-py{{ site.data.versions.PYTHON_VER }}
```

Many users do not need a specific platform combination but would like to ensure they're getting the latest version of RAPIDS, so as an additional convenience, a tag named simply `latest` is also provided which is equivalent to `cuda{{ site.data.versions.CUDA_VER }}-runtime-ubuntu16.04-py{{ site.data.versions.PYTHON_VER }}`.

## Prerequisites

* NVIDIA Pascal™ GPU architecture or better
* CUDA [10.1/10.2/11.0](https://developer.nvidia.com/cuda-downloads) with a compatible NVIDIA driver
* Ubuntu 16.04/18.04 or CentOS 7
* Docker CE v18+
* [nvidia-docker](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0)) v2+

## Usage

### Start Container and Notebook Server

#### Preferred - Docker CE v19+ and `nvidia-container-toolkit`
```bash
$ docker pull rapidsai/rapidsai:cuda{{ site.data.versions.CUDA_VER }}-runtime-ubuntu18.04-py{{ site.data.versions.PYTHON_VER }}
$ docker run --gpus all --rm -it -p 8888:8888 -p 8787:8787 -p 8786:8786 \
         rapidsai/rapidsai:cuda{{ site.data.versions.CUDA_VER }}-runtime-ubuntu18.04-py{{ site.data.versions.PYTHON_VER }}
```
**NOTE:** This will open a shell with [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) running in the background on port 8888 on your host machine.

#### Legacy - Docker CE v18 and `nvidia-docker2`
```bash
$ docker pull rapidsai/rapidsai:cuda{{ site.data.versions.CUDA_VER }}-runtime-ubuntu18.04-py{{ site.data.versions.PYTHON_VER }}
$ docker run --runtime=nvidia --rm -it -p 8888:8888 -p 8787:8787 -p 8786:8786 \
         rapidsai/rapidsai:cuda{{ site.data.versions.CUDA_VER }}-runtime-ubuntu18.04-py{{ site.data.versions.PYTHON_VER }}
```
**NOTE:** This will open a shell with [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) running in the background on port 8888 on your host machine.

### Use JupyterLab to Explore the Notebooks

Notebooks can be found in the following directories within the {{ site.data.releases.stable.version }} container:

* `/rapids/notebooks/clx` - CLX demo notebooks
* `/rapids/notebooks/cugraph` - cuGraph demo notebooks
* `/rapids/notebooks/cuml` - cuML demo notebooks
* `/rapids/notebooks/cusignal` - cuSignal demo notebooks
* `/rapids/notebooks/cuxfilter` - cuXfilter demo notebooks
* `/rapids/notebooks/xgboost` - XGBoost demo notebooks

For a full description of each notebook, see the [README](https://github.com/rapidsai/notebooks/blob/branch-{{ site.data.releases.stable.version }}/README.md) in the notebooks repository.

### Custom Data and Advanced Usage

You are free to modify the above steps. For example, you can launch an interactive session with your own data:

#### Preferred - Docker CE v19+ and `nvidia-container-toolkit`
```bash
$ docker run --gpus all --rm -it -p 8888:8888 -p 8787:8787 -p 8786:8786 \
         -v /path/to/host/data:/rapids/my_data \
                  rapidsai/rapidsai:cuda{{ site.data.versions.CUDA_VER }}-runtime-ubuntu18.04-py{{ site.data.versions.PYTHON_VER }}
```

#### Legacy - Docker CE v18 and `nvidia-docker2`
```bash
$ docker run --runtime=nvidia --rm -it -p 8888:8888 -p 8787:8787 -p 8786:8786 \
         -v /path/to/host/data:/rapids/my_data \
                  rapidsai/rapidsai:cuda{{ site.data.versions.CUDA_VER }}-runtime-ubuntu18.04-py{{ site.data.versions.PYTHON_VER }}
```
This will map data from your host operating system to the container OS in the `/rapids/my_data` directory. You may need to modify the provided notebooks for the new data paths.

### Access Documentation within Notebooks

You can check the documentation for RAPIDS APIs inside the JupyterLab notebook using a `?` command, like this:
```
[1] ?cudf.read_csv
```
This prints the function signature and its usage documentation. If this is not enough, you can see the full code for the function using `??`:
```
[1] ??pygdf.read_csv
```
Check out the RAPIDS [documentation](http://rapids.ai/start.html) for more detailed information and a RAPIDS [cheat sheet](https://rapids.ai/files/cheatsheet.pdf).

## More Information

Check out the [RAPIDS](https://docs.rapids.ai/api) and [XGBoost](https://xgboost.readthedocs.io/en/latest/) API docs.

Learn how to setup a mult-node cuDF and XGBoost data preparation and distributed training environment by following the [mortgage data example notebook and scripts](https://github.com/rapidsai/notebooks).

## Where can I get help or file bugs/requests?

Please submit issues with the container to this GitHub repository: [https://github.com/rapidsai/docs](https://github.com/rapidsai/docs/issues/new)

For issues with RAPIDS libraries like cuDF, cuML, RMM, or others file an issue in the related GitHub project.

Additional help can be found on [Stack Overflow](https://stackoverflow.com/tags/rapids) or [Google Groups](https://groups.google.com/forum/#!forum/rapidsai).