# This is a CENTOS container for Microsoft Open R with MKL 
# Using this container requires compliance with the Terms of Service
# of Microsoft R (Microsoft) and MKL (Intel)

# When building specify the version of R that you want
# Due to changes in the install script of MRO it is not advised
# to try versions much earlier than 3.3.1.

FROM centos:centos7
MAINTAINER jlisic@gmail.com 

# Build instructions: 
# docker build -t centos7-mro . --build-arg R_VER=3.3.2
ARG R_VER
ENV R_VER=${R_VER:-3.3.1} 


# Setup EPEL
RUN yum -y install epel-release 
RUN yum -y update 

# set locales
RUN echo "LANG=en_US.utf8" >> /etc/locale.conf
RUN localedef -c -f UTF-8 -i en_US en_US.UTF-8
RUN export LC_ALL=en_US.UTF-8

# Install R dependencies
# this is a bit lazy right now, and should be thinned out a bit -jjl
RUN yum -y install wget make bzip2-devel gcc-c++ gcc-gfortran libX11-devel libicu-devel libxml2 \
  libxml2-devel openssl-devel pcre-devel pkgconfig tcl-devel texinfo-tex tk-devel tre-devel \
  xz-devel zlib-devel bzip2-libs cpp expat-devel fontconfig-devel freetype-devel \
  gcc glib2 glibc-devel glibc-headers kernel-headers libX11 libXau libXau-devel libXft-devel \
  libXrender-devel libffi libgcc libicu libmpc libquadmath-devel libselinux libsepol libstdc++ \
  libstdc++-devel libxcb libxcb-devel mpfr pcre perl perl-Data-Dumper perl-Text-Unidecode \
  perl-libintl texinfo texlive-epsf xorg-x11-proto-devel xz-libs zlib which \
  && yum clean all


# get MRO
RUN wget https://mran.blob.core.windows.net/install/mro/${R_VER}/microsoft-r-open-${R_VER}.tar.gz  
RUN tar -xf microsoft-r-open-${R_VER}.tar.gz
RUN ./microsoft-r-open/install.sh -u -a

# install need packages
RUN R -e "install.packages(c('littler','stringr'))"

# using a symlink to get around the lack of dynamic variables in docker
# this is a slightly better way to handle setting up littler than editing 
# the text files through sed.
ENV LIBLOC /usr/lib64/R/library
RUN mkdir -p /usr/lib64/R/library

# handle litter
RUN echo ln -s /usr/lib64/microsoft-r/`ls /usr/lib64/microsoft-r/`/lib64/R/library/littler/examples/install.r /usr/local/bin/install.r > /install-littler.sh
RUN echo ln -s /usr/lib64/microsoft-r/`ls /usr/lib64/microsoft-r/`/lib64/R/library/littler/examples/install2.r /usr/local/bin/install2.r >> /install-littler.sh
RUN echo ln -s /usr/lib64/microsoft-r/`ls /usr/lib64/microsoft-r/`/lib64/R/library/littler/examples/installGithub.r /usr/local/bin/installGithub.r >> /install-littler.sh
RUN echo ln -s /usr/lib64/microsoft-r/`ls /usr/lib64/microsoft-r/`/lib64/R/library/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r >> /install-littler.sh
RUN echo ln -s /usr/lib64/microsoft-r/`ls /usr/lib64/microsoft-r/`/lib64/R/library/littler/bin/r /usr/local/bin/r >> /install-littler.sh
RUN echo ln -s /usr/lib64/microsoft-r/`ls /usr/lib64/microsoft-r/`/lib64/R/library /usr/lib64/R/library  >> /install-littler.sh

RUN  bash /install-littler.sh && install.r docopt 

# Clean UP
RUN rm -rf /tmp/*
RUN rm -rf microsoft-r-open-${R_VER}.tar.gz
RUN rm -rf microsoft-r-open
RUN rm -rf install-littler.sh 

CMD ["R"] 
