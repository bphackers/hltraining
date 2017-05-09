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

### Provisioning

Read povisioning.MD

### Manual

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

## Notes (getting started)

cryptogen - generates the x509 certificates used to identify and authenticate the various components in the network.
configtxgen - generates the requisite configuration artifacts for orderer bootstrap and channel creation.

    os_arch=$(echo "$(uname -s)-amd64" | awk '{print tolower($0)}')    #linux-amd64
    cd fabric-sample/release/samples/e2e
    export FABRIC_CFG_PATH=$PWD

Generate certs in the new folder fabric-sample/release/samples/e2e/crypto-config

    ./../../$os_arch/bin/cryptogen generate --config=./crypto-config.yaml

Then create the ordering service genesis block and a channel configuration artifact, from the previously generated certs:

    ./../../$os_arch/bin/configtxgen -profile TwoOrgs -outputBlock orderer.block

Ordered genesis block is created: /fabric-sample/release/samples/e2e/ordered.block

Create the channel.tx artifact which will be used to create channels in the future. Here the name of the channel is 'mychannel'. Don't put underscore _ in the name of your channel

    ./../../$os_arch/bin/configtxgen -profile TwoOrgs -outputCreateChannelTx channel.tx -channelID mychannel

Comment this line in /fabric-sample/release/samples/e2e/docker-compose.yaml to go through the commands manually:

    # command: /bin/bash -c './scripts/script.sh ${CHANNEL_NAME}; '

Then start the network

    export ARCH_TAG=$(uname -m)
    CHANNEL_NAME=mychannel docker-compose -f docker-compose-no-tls.yaml up
