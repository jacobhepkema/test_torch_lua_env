Bootstrap: docker
From: ubuntu:14.04

%help
    cuda_torch_basset

%labels
    Maintainer @jacobhepkema
    Version v0.1

%post
    apt-get -qqy update && apt-get install -y --no-install-recommends -q git curl python-pip cython libxft-dev libblas-dev liblapack-dev libatlas-base-dev gfortran libhdf5-dev wget bedtools
    mkdir -p /home/workspace
    mkdir -p /home/workspace/cuda
    curl -s https://raw.githubusercontent.com/torch/ezinstall/master/install-deps | bash
    git clone https://github.com/torch/distro.git /home/workspace/torch --recursive
    cd /home/workspace/torch
    ./install.sh
    bash -c "source ~/.bashrc"
    
    pip install numpy matplotlib seaborn pandas h5py sklearn pysam
    
    git clone https://github.com/davek44/Basset.git /home/workspace/Basset
    cd /home/workspace/Basset
    
    export BASSETDIR=/home/workspace/Basset
    export PATH=$BASSETDIR/src:$PATH
    export PYTHONPATH=$BASSETDIR/src:$PYTHONPATH
    ./install_dependencies.py
    
    echo 'export LUA_PATH="$BASSETDIR/src/?.lua;${LUA_PATH}"' >> ~/.bashrc
    bash -c "source ~/.bashrc"
    
%environment
    export BASSETDIR=/home/workspace/Basset
    export PATH=$BASSETDIR/src:$PATH
    export PYTHONPATH=$BASSETDIR/src:$PYTHONPATH
    export LUA_PATH="$BASSETDIR/src/?.lua;$LUA_PATH"
