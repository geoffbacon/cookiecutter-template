FROM ubuntu:18.04

# Install Ubuntu packages
RUN apt-get update && apt-get install -y \
    wget \
    bzip2 \
    nano \
    ca-certificates \
    curl \
    git \
    unzip \
    sudo \
    locales \
    build-essential

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Configure environment
SHELL ["/bin/bash", "-c"]
RUN locale-gen en_US.UTF-8  
ENV LC_ALL=en_US.UTF-8 \
	LANG=en_US.UTF-8 \
	LANGUAGE=en_US.UTF-8 \
    JUPYTER_TOKEN={{cookiecutter.project_slug}} \
	HOME=/root
RUN echo 'PS1="(docker) \W/ \$ "' >> /root/.bashrc
WORKDIR $HOME

# Install miniconda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
RUN chmod +x miniconda.sh && ./miniconda.sh -b -p miniconda && rm miniconda.sh
ENV PATH "$HOME/miniconda/bin:$PATH"
RUN conda update --yes conda && conda update --all --yes && conda config --add channels conda-forge

# Configure project
COPY . {{cookiecutter.project_slug}}
WORKDIR $HOME/{{cookiecutter.project_slug}}
#RUN make install-dev && make requirements && make install

EXPOSE {{cookiecutter.container_port}}
