Bootstrap: docker
From: nvidia/cuda:10.0-devel

# Inspired by https://github.com/natbutter/pytorch-singularity

# This file contains the dependencies except for pytorch torchvision cudatoolkit=10.0
%files
  environment.yml
  
%labels
  Inspired by natbutter/pytorch-singularity
  Maintained by jacobhepkema

%post
  mkdir /project /scratch

  #Now install everything
  apt update && apt install -y curl 

  curl -LO http://repo.continuum.io/miniconda/Miniconda3-4.5.12-Linux-x86_64.sh
  bash Miniconda3-4.5.12-Linux-x86_64.sh -p /miniconda -b
  rm Miniconda3-4.5.12-Linux-x86_64.sh
  PATH=/miniconda/bin:${PATH}
  
  apt-get install -y --no-install-recommends apt-utils build-essential make zlib1g procps
  
  conda update -y conda
  
  ENV_NAME=$(head -1 environment.yml | cut -d' ' -f2)
  echo ". /opt/conda/etc/profile.d/conda.sh" >> $SINGULARITY_ENVIRONMENT
  echo "conda activate $ENV_NAME" >> $SINGULARITY_ENVIRONMENT
  
  . /opt/conda/etc/profile.d/conda.sh
  # create environment
  conda env create -f environment.yml -p /opt/conda/envs/$ENV_NAME
  
  conda activate $ENV_NAME
  conda install -y pytorch torchvision cudatoolkit=10.0 -c pytorch

%environment
  export PATH=/miniconda/bin:${PATH}

%runscript
  exec /opt/conda/envs/$(head -n 1 environment.yml | cut -f 2 -d ' ')/bin/"$@"