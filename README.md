# hltraining

# Purpose
1. Understand principles
2. Setup basic environement (2 servers : 2 endorsers, 2 ordering, 1 ca).
3. Deploy basic application
4. Stress test
5. Hacking


# Documentation
## General
http://hyperledger-fabric.readthedocs.io/en/latest/getting_started.html

## Community 
Rocket : https://chat.hyperledger.org

Jira : https://jira.hyperledger.org

## Installation

setup you GOPATH env, in ~/.bashrc :

    export GOPATH=/usr/lib/go-1.6/
    export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

clone git repo :

    git clone https://github.com/hyperledger/fabric.git

then launch : 

    fabric/examples/e2e_cli/download-dockerimages.sh
