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

## Installation - provisioning or manual

### Simple install with Provisioning

Read provisioning.MD (no yet finished, prefer manual install)

### Manual install (getting started)

Setup tested on Ubuntu 16.04 64bits, as root, on a test server. Copy paste is supposed to work
Root user has been chosen to fix permission issues, to quicly try Fabric

###### Install prerequisites

Note: GOROOT is for compiler/tools that comes from go installation. GOPATH is for your own go projects / 3rd party libraries (downloaded with "go get").

    #sudo su -
    apt-get install git docker.io python-pip curl docker-compose curl wget
    cd /tmp
    wget https://storage.googleapis.com/golang/go1.7.5.linux-amd64.tar.gz
    tar -zxvf go1.7.5.linux-amd64.tar.gz
    mv go /opt
    echo "export GOROOT=/opt/go" >> ~/.bashrc
    echo "export GOPATH=$GOROOT/bin" >> ~/.bashrc
    echo "export PATH=$PATH:$GOROOT/bin:$GOPATH/bin" >> ~/.bashrc
    source ~/.bashrc

    systemctl enable docker.service
    service docker start
    reboot

    #sudo su -
    mkdir fabric-sample
    cd fabric-sample
    curl -L https://nexus.hyperledger.org/content/repositories/snapshots/sandbox/vex-yul-hyp-jenkins-2/fabric-binaries/release.tar.gz -o release.tar.gz 2> /dev/null;  tar -xvf release.tar.gz
    cd release/linux-amd64/install
    ./get-docker-images.sh


###### Generate certs, genesis and channel tx artifact

cryptogen - generates the x509 certificates used to identify and authenticate the various components in the network.

configtxgen - generates the requisite configuration artifacts for orderer bootstrap and channel creation.

    os_arch=$(echo "$(uname -s)-amd64" | awk '{print tolower($0)}')
    cd ~/fabric-sample/release/samples/e2e


Generate certs with cryptogen command(new folder wil be created fabric-sample/release/samples/e2e/crypto-config). Then create the ordering service genesis block and a channel configuration artifact with configtxgen command, from the previously generated certs:

    ~/fabric-sample/release/$os_arch/bin/cryptogen generate --config=./crypto-config.yaml
    export FABRIC_CFG_PATH=$PWD
    ~/fabric-sample/release/linux-amd64/bin/configtxgen -profile TwoOrgs -outputBlock orderer.block

Ordered genesis block is created: /fabric-sample/release/samples/e2e/ordered.block

Create the channel.tx artifact which will be used to create channels in the future. Here the name of the channel is 'mychannel'. Note: don't put underscore _ in the name of your channel.

    ~/fabric-sample/release/linux-amd64/bin/configtxgen -profile TwoOrgs -outputCreateChannelTx channel.tx -channelID mychannel

Comment this line in ~/fabric-sample/release/samples/e2e/docker-compose.yaml to go through the commands manually with vim or nano (TODO: automate with sed later):

    # command: /bin/bash -c './scripts/script.sh ${CHANNEL_NAME}; '

Then start the network

    export ARCH_TAG=$(uname -m)
    CHANNEL_NAME=mychannel docker-compose -f docker-compose-no-tls.yaml up

In another shell

    # sudo su -
    docker exec -it cli bash

## Tips and issues

###### Docker-compose errors

Sometimes these errors appear. Especially when permission problem or when restarting docker-compose 

    Error: channel create configuration tx file not found read channel.tx: is a directory
and

    !!!!!!!!!!!!!!! Channel creation failed !!!!!!!!!!!!!!!!
    cli                       | ================== ERROR !!! FAILED to execute End-2-End Scenario ==================

In another terminal:

    cd ~/fabric-sample/release/samples/e2e
    docker exec -it cli bash

I get this error (probably due to the previous errors):

    Error response from daemon: Container 05539bef54617615021b3778a952ba644d6a9ba2063ac9726ea6b3323320a005 is not running


clone git repo :

    git clone https://github.com/hyperledger/fabric.git

then launch :

    fabric/examples/e2e_cli/download-dockerimages.sh

###### Docker tip

May be you'll need to allow non-root users to access the docker service:

https://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo
