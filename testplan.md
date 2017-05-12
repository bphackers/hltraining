# Test plan

## Synchronizing

1/ disconnect a peer, restart and check if synchronization can be done.
--> state update
check : https://jira.hyperledger.org/browse/FAB-707


## Deploy contract

1/ create and deploy a chaincode

2/ update chaincode

## Run contract

1/ several endorser executing same chcincode, without the same result (ex : random number generatin)
--> transaction cannot be validated on the state

