# Moods
#### A POC Blockchain Application using Hyperledger Fabric v1.0

### The Application
The tasks in developing this Application can be divided as follows:
* Setting up the Blockchain Network
* Writing the Chaincode
* Using the NodeJS SDK to query/update the network

#### The Blockchain Network Setup
The simplest possible network was setup in a Ubuntu 16.04 LTS Local Environment
* One Orderer
* One Peer in an Organisation

A Channel was created and the single Peer was added to the channel. The Go Chaincode was installed on the Peer's filesystem.

Once [fabric-samples](https://hyperledger-fabric.readthedocs.io/en/latest/samples.html) is setup and the required prerequisites are installed as given [here](https://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html) we can start the network as follows.
```
cd $GOPATH/src/github.com/hyperledger/fabric-samples
git clone https://github.com/YashAgarwal/moodsapp.git
cd moodsapp
#Start the Blockchain Nodes
./startFabric
```

The script launches all the nodes(as docker containers) defined in our network configuration files
```shell
docker-compose -f docker-compose.yml down
Removing network net_basic

docker-compose -f docker-compose.yml up -d ca.example.com orderer.example.com peer0.org1.example.com couchdb
Creating network "net_basic" with the default driver
Creating orderer.example.com
Creating couchdb
Creating ca.example.com
Creating peer0.org1.example.com

# wait for Hyperledger Fabric to start
# incase of errors when running later commands, issue export FABRIC_START_TIMEOUT=<larger number>
export FABRIC_START_TIMEOUT=10
#echo ${FABRIC_START_TIMEOUT}
sleep ${FABRIC_START_TIMEOUT}

# Create the channel
docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@org1.example.com/msp" peer0.org1.example.com peer channel create -o orderer.example.com:7050 -c mychannel -f /etc/hyperledger/configtx/channel.tx
2017-07-15 08:06:12.077 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2017-07-15 08:06:12.145 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2017-07-15 08:06:12.163 UTC [channelCmd] InitCmdFactory -> INFO 003 Endorser and orderer connections initialized
2017-07-15 08:06:12.176 UTC [msp] GetLocalMSP -> DEBU 004 Returning existing local MSP
2017-07-15 08:06:12.176 UTC [msp] GetDefaultSigningIdentity -> DEBU 005 Obtaining default signing identity
2017-07-15 08:06:12.176 UTC [msp] GetLocalMSP -> DEBU 006 Returning existing local MSP
2017-07-15 08:06:12.176 UTC [msp] GetDefaultSigningIdentity -> DEBU 007 Obtaining default signing identity
2017-07-15 08:06:12.176 UTC [msp/identity] Sign -> DEBU 008 Sign: plaintext: 0A8C060A074F7267314D53501280062D...53616D706C65436F6E736F727469756D
2017-07-15 08:06:12.176 UTC [msp/identity] Sign -> DEBU 009 Sign: digest: E0D8DE2F08C3146C6A6F3FF237E2D190D54EF9AB7B5EC9866E777225F9A95197
2017-07-15 08:06:12.177 UTC [msp] GetLocalMSP -> DEBU 00a Returning existing local MSP
2017-07-15 08:06:12.177 UTC [msp] GetDefaultSigningIdentity -> DEBU 00b Obtaining default signing identity
2017-07-15 08:06:12.177 UTC [msp] GetLocalMSP -> DEBU 00c Returning existing local MSP
2017-07-15 08:06:12.177 UTC [msp] GetDefaultSigningIdentity -> DEBU 00d Obtaining default signing identity
2017-07-15 08:06:12.177 UTC [msp/identity] Sign -> DEBU 00e Sign: plaintext: 0AC3060A1508021A0608F499A7CB0522...F8A2713D0BA8F207AEFA560A2AEF4BE7
2017-07-15 08:06:12.177 UTC [msp/identity] Sign -> DEBU 00f Sign: digest: 96789701A8B3A41F819303EE4E66290473DC5B5D5786C89EEA441FDD91E2C29F
2017-07-15 08:06:12.250 UTC [msp] GetLocalMSP -> DEBU 010 Returning existing local MSP
2017-07-15 08:06:12.250 UTC [msp] GetDefaultSigningIdentity -> DEBU 011 Obtaining default signing identity
2017-07-15 08:06:12.251 UTC [msp] GetLocalMSP -> DEBU 012 Returning existing local MSP
2017-07-15 08:06:12.251 UTC [msp] GetDefaultSigningIdentity -> DEBU 013 Obtaining default signing identity
2017-07-15 08:06:12.251 UTC [msp/identity] Sign -> DEBU 014 Sign: plaintext: 0AC3060A1508021A0608F499A7CB0522...4ACABFF07BD912080A021A0012021A00
2017-07-15 08:06:12.251 UTC [msp/identity] Sign -> DEBU 015 Sign: digest: CB311A50B1AFAC9D6629BEE280F94F4E3EE04946DA0BC6513AD8ED5BACF978C5
2017-07-15 08:06:12.251 UTC [channelCmd] readBlock -> DEBU 016 Got status:*orderer.DeliverResponse_Status
2017-07-15 08:06:12.251 UTC [msp] GetLocalMSP -> DEBU 017 Returning existing local MSP
2017-07-15 08:06:12.251 UTC [msp] GetDefaultSigningIdentity -> DEBU 018 Obtaining default signing identity
2017-07-15 08:06:12.252 UTC [channelCmd] InitCmdFactory -> INFO 019 Endorser and orderer connections initialized
2017-07-15 08:06:12.452 UTC [msp] GetLocalMSP -> DEBU 01a Returning existing local MSP
2017-07-15 08:06:12.452 UTC [msp] GetDefaultSigningIdentity -> DEBU 01b Obtaining default signing identity
2017-07-15 08:06:12.452 UTC [msp] GetLocalMSP -> DEBU 01c Returning existing local MSP
2017-07-15 08:06:12.452 UTC [msp] GetDefaultSigningIdentity -> DEBU 01d Obtaining default signing identity
2017-07-15 08:06:12.452 UTC [msp/identity] Sign -> DEBU 01e Sign: plaintext: 0AC3060A1508021A0608F499A7CB0522...47C66009C47512080A021A0012021A00
2017-07-15 08:06:12.452 UTC [msp/identity] Sign -> DEBU 01f Sign: digest: 0BF4E0D5F8D2CA7DA6DE94C54DAFA101BDCC2CE789BC4B9680DA45D16F0B0CB6
2017-07-15 08:06:12.453 UTC [channelCmd] readBlock -> DEBU 020 Got status:*orderer.DeliverResponse_Status
2017-07-15 08:06:12.453 UTC [msp] GetLocalMSP -> DEBU 021 Returning existing local MSP
2017-07-15 08:06:12.453 UTC [msp] GetDefaultSigningIdentity -> DEBU 022 Obtaining default signing identity
2017-07-15 08:06:12.454 UTC [channelCmd] InitCmdFactory -> INFO 023 Endorser and orderer connections initialized
2017-07-15 08:06:12.654 UTC [msp] GetLocalMSP -> DEBU 024 Returning existing local MSP
2017-07-15 08:06:12.654 UTC [msp] GetDefaultSigningIdentity -> DEBU 025 Obtaining default signing identity
2017-07-15 08:06:12.654 UTC [msp] GetLocalMSP -> DEBU 026 Returning existing local MSP
2017-07-15 08:06:12.654 UTC [msp] GetDefaultSigningIdentity -> DEBU 027 Obtaining default signing identity
2017-07-15 08:06:12.654 UTC [msp/identity] Sign -> DEBU 028 Sign: plaintext: 0AC3060A1508021A0608F499A7CB0522...9320B04FACEC12080A021A0012021A00
2017-07-15 08:06:12.654 UTC [msp/identity] Sign -> DEBU 029 Sign: digest: AA2730FE32D9CF94F6BCC4C0BFF6ABA0692708153ABC3C4675DB46A02201575C
2017-07-15 08:06:12.658 UTC [channelCmd] readBlock -> DEBU 02a Received block:0
2017-07-15 08:06:12.658 UTC [main] main -> INFO 02b Exiting.....
# Join peer0.org1.example.com to the channel.
docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@org1.example.com/msp" peer0.org1.example.com peer channel join -b mychannel.block
2017-07-15 08:06:12.769 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2017-07-15 08:06:12.769 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2017-07-15 08:06:12.770 UTC [grpc] Printf -> DEBU 003 grpc: addrConn.resetTransport failed to create client transport: connection error: desc = "transport: Error while dialing dial tcp 172.22.0.5:7051: getsockopt: connection refused"; Reconnecting to {peer0.org1.example.com:7051 <nil>}
2017-07-15 08:06:13.770 UTC [grpc] Printf -> DEBU 004 grpc: addrConn.resetTransport failed to create client transport: connection error: desc = "transport: Error while dialing dial tcp 172.22.0.5:7051: getsockopt: connection refused"; Reconnecting to {peer0.org1.example.com:7051 <nil>}
2017-07-15 08:06:15.476 UTC [channelCmd] InitCmdFactory -> INFO 005 Endorser and orderer connections initialized
2017-07-15 08:06:15.477 UTC [msp/identity] Sign -> DEBU 006 Sign: plaintext: 0A8A070A5C08011A0C08F799A7CB0510...CCBE3220A1501A080A000A000A000A00
2017-07-15 08:06:15.477 UTC [msp/identity] Sign -> DEBU 007 Sign: digest: 2219CBB13F8CAEA8116E503F06E526ED272D8DB7CEB69AF6161FC95139E6A948
2017-07-15 08:06:16.085 UTC [channelCmd] executeJoin -> INFO 008 Peer joined the channel!
2017-07-15 08:06:16.085 UTC [main] main -> INFO 009 Exiting.....
Creating cli
2017-07-15 08:06:20.512 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2017-07-15 08:06:20.512 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2017-07-15 08:06:20.519 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2017-07-15 08:06:20.519 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2017-07-15 08:06:21.824 UTC [golang-platform] getCodeFromFS -> DEBU 005 getCodeFromFS github.com/moods
2017-07-15 08:06:22.823 UTC [golang-platform] func1 -> DEBU 006 Discarding GOROOT package bytes
2017-07-15 08:06:22.823 UTC [golang-platform] func1 -> DEBU 007 Discarding GOROOT package encoding/json
2017-07-15 08:06:22.824 UTC [golang-platform] func1 -> DEBU 008 Discarding GOROOT package fmt
2017-07-15 08:06:22.824 UTC [golang-platform] func1 -> DEBU 009 Discarding provided package github.com/hyperledger/fabric/core/chaincode/shim
2017-07-15 08:06:22.824 UTC [golang-platform] func1 -> DEBU 00a Discarding provided package github.com/hyperledger/fabric/protos/peer
2017-07-15 08:06:22.824 UTC [golang-platform] func1 -> DEBU 00b Discarding GOROOT package strconv
2017-07-15 08:06:22.824 UTC [golang-platform] GetDeploymentPayload -> DEBU 00c done
2017-07-15 08:06:22.842 UTC [msp/identity] Sign -> DEBU 00d Sign: plaintext: 0A8A070A5C08031A0C08FE99A7CB0510...A5CF7F030000FFFF79E79F8B001E0000
2017-07-15 08:06:22.842 UTC [msp/identity] Sign -> DEBU 00e Sign: digest: E32DA5B9499DFFF6D149BD48071DA4DF52F236EBA43A1B3C4F92897465CA083C
2017-07-15 08:06:22.850 UTC [chaincodeCmd] install -> DEBU 00f Installed remotely response:<status:200 payload:"OK" >
2017-07-15 08:06:22.850 UTC [main] main -> INFO 010 Exiting.....
2017-07-15 08:06:22.993 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2017-07-15 08:06:22.993 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2017-07-15 08:06:22.994 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2017-07-15 08:06:22.994 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2017-07-15 08:06:22.995 UTC [msp/identity] Sign -> DEBU 005 Sign: plaintext: 0A95070A6708031A0C08FE99A7CB0510...324D53500A04657363630A0476736363
2017-07-15 08:06:22.995 UTC [msp/identity] Sign -> DEBU 006 Sign: digest: 6346BCB5F87C663CB48EB57F7ACC306B4AC43165F911DD300F9E4A27C5DBD8BF
2017-07-15 08:06:24.707 UTC [msp/identity] Sign -> DEBU 007 Sign: plaintext: 0A95070A6708031A0C08FE99A7CB0510...3282FB01266A2229EB8132C1B78A7754
2017-07-15 08:06:24.707 UTC [msp/identity] Sign -> DEBU 008 Sign: digest: 158411EC122A597F31687F49448224D877484E1A9702918258971D41EF4EC07F
2017-07-15 08:06:24.712 UTC [main] main -> INFO 009 Exiting.....
2017-07-15 08:06:35.079 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2017-07-15 08:06:35.079 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2017-07-15 08:06:35.079 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2017-07-15 08:06:35.079 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2017-07-15 08:06:35.080 UTC [msp/identity] Sign -> DEBU 005 Sign: plaintext: 0A95070A6708031A0B088B9AA7CB0510...1A0E0A0A696E69744C65646765720A00
2017-07-15 08:06:35.080 UTC [msp/identity] Sign -> DEBU 006 Sign: digest: 77FEDCB5AEAB4843F5BE4CE60936D40A861C1821D6E466337275943BC52E2013
2017-07-15 08:06:35.111 UTC [msp/identity] Sign -> DEBU 007 Sign: plaintext: 0A95070A6708031A0B088B9AA7CB0510...1AD0E292A00E12F06E399DD3B9E0EE89
2017-07-15 08:06:35.111 UTC [msp/identity] Sign -> DEBU 008 Sign: digest: 857758918917863380387BA1F5FA0FF7A168A4472983F5906E07C38048C0D8CD
2017-07-15 08:06:35.114 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> DEBU 009 ESCC invoke result: version:1 response:<status:200 message:"OK" > payload:"\n \315\242\354\243\"\250#O\225x\225\nI\273\032\335HH]/\275\213*B\234tU\025]\362\214\253\022\301\001\n\253\001\022\025\n\004lscc\022\r\n\013\n\005moods\022\002\010\001\022\221\001\n\005moods\022\207\001\032+\n\005MOOD0\032\"{\"user\":\"1\",\"type\":\"2\",\"case\":\"1\"}\032+\n\005MOOD1\032\"{\"user\":\"2\",\"type\":\"2\",\"case\":\"1\"}\032+\n\005MOOD2\032\"{\"user\":\"3\",\"type\":\"2\",\"case\":\"1\"}\032\003\010\310\001\"\014\022\005moods\032\0031.0" endorsement:<endorser:"\n\007Org1MSP\022\374\005-----BEGIN -----\nMIICGDCCAb+gAwIBAgIQPcMFFEB/vq6mEL6vXV7aUTAKBggqhkjOPQQDAjBzMQsw\nCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNU2FuIEZy\nYW5jaXNjbzEZMBcGA1UEChMQb3JnMS5leGFtcGxlLmNvbTEcMBoGA1UEAxMTY2Eu\nb3JnMS5leGFtcGxlLmNvbTAeFw0xNzA2MjMxMjMzMTlaFw0yNzA2MjExMjMzMTla\nMFsxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1T\nYW4gRnJhbmNpc2NvMR8wHQYDVQQDExZwZWVyMC5vcmcxLmV4YW1wbGUuY29tMFkw\nEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEzS9k2gCKHcat8Wj4T2nB1uyC8R2zg3um\nxdTL7nmgFWp0uyCCbQQxD/VS+8R/3DNvEFkvzhcjc9NU/nRqMirpLqNNMEswDgYD\nVR0PAQH/BAQDAgeAMAwGA1UdEwEB/wQCMAAwKwYDVR0jBCQwIoAgDnKSJOiz8xeE\nyKk8W4729MHJHZ5uV3xFwzFjYJ/kABEwCgYIKoZIzj0EAwIDRwAwRAIgHBdxbHUG\nrFUzKPX9UmmN3SwigWcRUREUy/GTb3hDIAsCIEF1BxTqv8ilQYE8ql0wJL4mTber\nHE6DFYvvBCUnicUh\n-----END -----\n" signature:"0E\002!\000\313}\271I\340\227\343\3542\177\253C\251-j\353\010\2232\362p\241\243\002\312\203_\250\3543\204\223\002 4\026DF\031m\204\240\\l\317-\013\307\ne\032\320\342\222\240\016\022\360n9\235\323\271\340\356\211" >
2017-07-15 08:06:35.115 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> INFO 00a Chaincode invoke successful. result: status:200
2017-07-15 08:06:35.115 UTC [main] main -> INFO 00b Exiting.....

Total execution time : 63 secs ...
```
#### Data Storage
A state database is stored using CouchDB. A single entry in the state is defined as

```
type Mood struct {
  User  string `json:"user"`
  Type  string `json:"type"`
  Case  string `json:"case"`
}
```
where, <br />
Type 1 => Happy, Sad <br />
Type 2 => Excited, Bored <br />
Type 3 => Laughing, Crying <br />
for example <br />
{User "1", Type: "1", Case: "1"} denotes Happy send by user "1" <br />
{User "1", Type: "1", Case: "2"} denotes Sad send by user "1" <br />
{User "1", Type: "2", Case: "1"} denotes Excited send by user "1" <br />

#### The Chaincode
The chaincode written has the following functions
* initLedger: It initializes the state with some sample entries
* createMood: It adds a new entry to the state
* queryMood: It returns one specific entry from the state
* queryAllMoods: It displays all the entries listed in the state

#### The Node.js SDK is used as follows to issue query and update commands

```shell
#this will run "queryAllMoods" function from the chaincode
node query.js
```

```shell
Create a client and set the wallet location
Set wallet path, and associate user  PeerAdmin  with application
Check user is enrolled, and set a query URL in the network
Make query
Assigning transaction_id:  cb49e2a04f47af4b26fd705f26a12fbc8ddc182c5a9ed3e82dbde5f67403931c
returned from query
Query result count =  1
Response is  [{"Key":"MOOD0", "Record":{"case":"1","type":"2","user":"1"}},{"Key":"MOOD1", "Record":{"case":"1","type":"2","user":"2"}},{"Key":"MOOD2", "Record":{"case":"1","type":"2","user":"3"}}]
```

```shell
#this will run "createMood" function from the chaincode with the arguemnts ['MOOD4', '1', '1', '1'] denoting user 1 is Happy
node invoke.js
```

```shell
Create a client and set the wallet location
Set wallet path, and associate user  PeerAdmin  with application
Check user is enrolled, and set a query URL in the network
Assigning transaction_id:  8118f42e1df8ff69bdcb46fb54144b40c930f3ff978985c625213e3a742fc7a2
transaction proposal was good
Successfully sent Proposal and received ProposalResponse: Status - 200, message - "OK", metadata - "", endorsement signature: info: [EventHub.js]: _connect - options {}
The transaction has been committed on peer localhost:7053
 event promise all complete and testing complete
Successfully sent transaction to the orderer.
```

```shell
node query.js
```

```shell
Create a client and set the wallet location
Set wallet path, and associate user  PeerAdmin  with application
Check user is enrolled, and set a query URL in the network
Make query
Assigning transaction_id:  1f47adb8efb9c8451236460cdd2e691d84c59d957ce7a884469ee6b817d7792e
returned from query
Query result count =  1
Response is  [{"Key":"MOOD0", "Record":{"case":"1","type":"2","user":"1"}},{"Key":"MOOD1", "Record":{"case":"1","type":"2","user":"2"}},{"Key":"MOOD2", "Record":{"case":"1","type":"2","user":"3"}},{"Key":"MOOD4", "Record":{"case":"1","type":"1","user":"1"}}]
```

### Identity management - take three users: You, Me and Vandana
Indentity Management is not implemented for this version of the application. Currently it is understood Fabric CA has to be used to create a user registory and enrollment based user credentials.

### Lessons learned
* Basics of Docker Container
* Setting up the developement environment for hyperledger fabric
* Basic concepts in Hyperledger Fabric  
  - The Transaction Process Flow
  - The major components in a blockchain network
* Writing Chaincode in Go

### Sources of code
* This applications was built upon the codebase of [Fabcar Sample](https://github.com/hyperledger/fabric-samples/tree/release/fabcar) from the fabric-sample repo.
* The network configuration files used are from [Basic-Network Sample](https://github.com/hyperledger/fabric-samples/tree/release/basic-network) from the fabric-sample repo.

### Information sources
* Major Information Source: Hyperledger Fabric [Documentation](https://hyperledger-fabric.readthedocs.io/en/latest/blockchain.html)
* Many other tutorials were tried out unsuccessfully.
