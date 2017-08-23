
# R Docker Centos #

## Overview ##

This repository contains a set of alternative Dockerfiles for slightly different than what is found at [rocker](https://hub.docker.com/r/rocker).
In particular, this includes images based on Centos 6 and 7; images based on OpenSUSE and Clear Linux are also in development.


## How to Use  ##

### Centos-EPEL ###

Centos EPEL Dockerfiles use the current version of R found in EPEL, therefore no versioning is performed.
You may be able to get older versions of R by setting the *from* line in the docker file to an older Centos version (this has not been tested).
Centos EPEL is not compiled with R-SHLIB's, but is compiled with OpenBLAS.  Therfore, it is not possible to exchange BLAS versions.

### Centos-MRO ###

Centos MRO Dockerfiles can use any MRO version >= 3.3.1.
Earlier of MRO are not supported due to changes in the installation script.
In addition only Centos 7 is supported at this time due to issues with getopt on Centos 6.

To install a specific version of MRO use the following command:

`
docker build -t centos-7-mro --build-arg R_VER=3.4.0  https://raw.githubusercontent.com/jlisic/R-docker-centos/master/centos-7-mro/Dockerfile
`

The default version is set to 3.3.1. 


 

