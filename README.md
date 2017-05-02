# hltraining

# Purpose
1. Understand principles
2. Setup basic environement (2 servers : 2 endorsers, 2 ordering, 1 ca).
3. Deploy basic application
4. Stress test
5. Hacking


# Documentation

First of all, you need a Linux Fundation account (LFID)

## General
http://hyperledger-fabric.readthedocs.io/en/latest/getting_started.html

## Community 
Chat : https://chat.hyperledger.org

Jira : https://jira.hyperledger.org

Gerit : https://gerrit.hyperledger.org

## Installation
Download and install GO.
Important : set the following environment variables : 

GOROOT is for compiler/tools that comes from go installation.

GOPATH is for your own go projects / 3rd party libraries (downloaded with "go get").

setup you GOPATH env, in ~/.bashrc :

    export GOPATH=/usr/lib/go-1.6/
    export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

clone git repo :

    git clone https://github.com/hyperledger/fabric.git

then launch : 

    fabric/examples/e2e_cli/download-dockerimages.sh

## Tips
May be you'll need to allow non-root users to access the docker service :

https://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo
