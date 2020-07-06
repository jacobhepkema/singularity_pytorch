Bootstrap: docker
From: nvidia/cuda:10.0-devel

# Inspired by https://github.com/natbutter/pytorch-singularity

# This file contains the dependencies
%files
  environment.yml
  
%labels
  Inspired by natbutter/pytorch-singularity
  Maintained by jacobhepkema

%post
  mkdir /project /scratch

  #Now install everything
  apt update && apt install -y apt-utils curl 

  curl -LO http://repo.continuum.io/miniconda/Miniconda3-4.5.12-Linux-x86_64.sh
  bash Miniconda3-4.5.12-Linux-x86_64.sh -p /miniconda -b
  rm Miniconda3-4.5.12-Linux-x86_64.sh
  PATH=/miniconda/bin:${PATH}
  
  apt-get install -y --no-install-recommends build-essential make zlib1g procps
  
  /miniconda/bin/conda update -y conda
  
  ENV_NAME=$(head -1 environment.yml | cut -d' ' -f2)
  echo ". /miniconda/etc/profile.d/conda.sh" >> $SINGULARITY_ENVIRONMENT
  echo "/miniconda/bin/conda activate $ENV_NAME" >> $SINGULARITY_ENVIRONMENT
  
  . /miniconda/etc/profile.d/conda.sh
  # create environment
  /miniconda/bin/conda env create -f environment.yml -p /miniconda/envs/$ENV_NAME
  /miniconda/bin/conda activate $ENV_NAME

%environment
  export PATH=/miniconda/bin:${PATH}

%runscript
  exec /miniconda/envs/$(head -n 1 environment.yml | cut -f 2 -d ' ')/bin/"$@"
