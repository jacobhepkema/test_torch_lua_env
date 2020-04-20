Bootstrap: docker
From: kaixhin/cuda-torch

%help
    cuda_torch_basset

%labels
    Maintainer @jacobhepkema
    Version v0.1

%post
    apt-get update && apt-get install -y --no-install-recommends procps lbzip2 libhdf4-alt-dev libhdf5-dev libxml-parser-perl
    
    wget https://luarocks.org/releases/luarocks-3.3.0.tar.gz
    tar -zxpf luarocks-3.3.0.tar.gz
    cd luarocks-3.3.0
    ./configure && make && sudo make install
        
    cd ~
    git clone https://github.com/davek44/Basset --recursive && cd Basset && ./install_dependencies.py
    
    luarocks install luafilesystem
    luarocks install dpnn
    luarocks install inn
    luarocks install dp
    
    git clone https://github.com/davek44/torch-hdf5.git && cd torch-hdf5
    
    luarocks make
    
%environment
    export BASSETDIR=~/Basset
    export PATH=$BASSETDIR/src:$PATH
    export PYTHONPATH=$BASSETDIR/src:$PYTHONPATH
    export LUA_PATH="$BASSETDIR/src/?.lua;$LUA_PATH"
