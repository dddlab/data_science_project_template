# Data Science Project Template

![Docker Build CI](https://github.com/dddlab/data_science_project_template/workflows/Docker%20Build%20CI/badge.svg) [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/dddlab/data_science_project_template/main)

This template repository simplifies setting up a reproducible data science project using Jupyter notebook.

The procedure outlined in this repository assumes you have a project that runs in a Jupyter notebook environment. Following the procedure in this notebook will 

* (Docker Hub) Build a docker image from your `Dockerfile`
    * From [Jupyter docker images](https://hub.docker.com/u/jupyter) ([Github repository](https://github.com/jupyter/docker-stacks), [Selecting an image](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html))
    * Or another Jupyter-notebook compatible image
    * And make the built image available on [Docker Hub](https://hub.docker.com)
* (Local/VM) Mange development environment by  
    * Automating any installation process with docker
    * Securing your Jupyter notebook server with self-signed keys and hashed password
    * Starting/stopping the server with simple commands
* (Binder) Allow project repository visitors to tryout your work without installing any software by 
    * Providing compatibility with [Binder](https://mybinder.org)
    * Having Binder load your docker image from Docker Hub
* (GitHub Actions) Continuously build your docker image for your project

## Repository structure

* `binder/Dockerfile` (Binder): [docker image for Binder](https://mybinder.readthedocs.io/en/latest/tutorials/dockerfile.html)
* `docker-compose.yml` (Local/VM): start/stop Jupyter notebook server process
* `Dockerfile` (Local/VM): defines Jupyter notebook environment
* `Makefile` (Local/VM): build and start/stop Jupyter notebook server
* `setup.sh` (Local/VM): prepares local directory to run a server instance

## Initial Setup

* Update Docker hub credentials for GitHub Actions: `.github/workflows/build-docker-image.yml`
   * Refer to [GitHub secrets documentation on encrypting credentials](https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets)
   * `DOCKER_USERNAME` variable for Docker Hub in GitHub secrets
   * `DOCKER_PASSWORD` variable for Docker Hub in GitHub secrets

## Local/VM Development Server

### Install Docker and build image

```bash
# install necessary packages on debian or ubuntu
sudo apt-get update && \
   sudo apt-get install -y wget git docker.io docker-compose && \
   sudo usermod -aG docker $USER

# re-login to to apply docker group setting

git clone https://github.com/dddlab/data_science_project_template.git
cd dddlab/data_science_project_template/
make setup
make build
```

### Start/Stop Jupyter notebook server

```bash
make start
make stop
```

## Running on Binder

* File `binder/Dockerfile` defines a docker image for Binder sessions
* Update base image to point to a [stable docker image](https://hub.docker.com/repository/docker/dddlab/ds-project-template/tags?page=1)
