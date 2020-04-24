Bootstrap: docker
From: nvidia/cuda:9.1-cudnn7-devel-ubuntu16.04

# Highly based on https://github.com/NIH-HPC/singularity-examples/blob/master/torch/Singularity.torch

%help
    cuda_torch_basset

%labels
    Maintainer @jacobhepkema
    Version v0.1

%post
    apt-get -qqy update && apt-get install -y -q dialog apt-utils git wget vim curl software-properties-common cython libssl-dev libzmq3-dev libxft-dev libblas-dev liblapack-dev libatlas-base-dev gfortran libhdf5-dev python-dev python-zmq python-pip bedtools
    mkdir -p /opt
    cd /opt
    git clone https://github.com/torch/distro.git --recursive torch
    cd torch
    sed 's/sudo//g' install-deps > install-deps.nosudo
    bash install-deps.nosudo
    
    # ran into https://github.com/torch/cutorch/issues/797
    export TORCH_NVCC_FLAGS="-D__CUDA_NO_HALF_OPERATORS__"
    
    ./install.sh -sb
    
    # this installs a cudnn lua package that is not compatible with cuDNN7
    # # so install that separately again
    git clone https://github.com/soumith/cudnn.torch -b R7
    cd cudnn.torch
    /opt/torch/install/bin/luarocks make cudnn-scm-1.rockspec
    
    # create some generic mount points
    mkdir /mnt/data /mnt/input /mnt/output /mnt/ref /mnt/code /mnt/work
    
    pip install numpy matplotlib seaborn pandas h5py sklearn pysam
    
    git clone https://github.com/davek44/Basset.git /mnt/Basset
    cd /mnt/Basset
    
    export BASSETDIR=/mnt/Basset
    export PATH=$BASSETDIR/src:$PATH
    export PYTHONPATH=$BASSETDIR/src:$PYTHONPATH
    
    ./install_dependencies.py
    
    echo 'export LUA_PATH="$BASSETDIR/src/?.lua;${LUA_PATH}"' >> ~/.bashrc
    bash -c "source ~/.bashrc"
    
    # cleanup
    apt-get clean
    
%environment
    export LC_ALL=C
    LUA_PATH='\
    /opt/torch/install/share/lua/5.1/?.lua;\
    /opt/torch/install/share/lua/5.1/?/init.lua;\
    ./?.lua;\
    /opt/torch/install/share/luajit-2.1.0-beta1/?.lua;\
    /usr/local/share/lua/5.1/?.lua;\
    /usr/local/share/lua/5.1/?/init.lua'
    export LUA_PATH
    
    export BASSETDIR=/mnt/Basset
    export PATH=$BASSETDIR/src:$PATH
    export PYTHONPATH=$BASSETDIR/src:$PYTHONPATH
    export LUA_PATH="$BASSETDIR/src/?.lua;$LUA_PATH"
    
    LUA_CPATH='\
    /opt/torch/install/lib/lua/5.1/?.so;./?.so;\
    /usr/local/lib/lua/5.1/?.so;\
    /usr/local/lib/lua/5.1/loadall.so;\
    /opt/torch/install/lib/?.so;'
    export LUA_CPATH
    
    export PATH=/opt/torch/install/bin:$PATH
    export LD_LIBRARY_PATH=/opt/torch/install/lib:$LD_LIBRARY_PATH
    export DYLD_LIBRARY_PATH=/opt/torch/install/lib:$DYLD_LIBRARY_PATH

%runscript
