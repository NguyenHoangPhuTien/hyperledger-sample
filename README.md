# hyperledger-sample
export PATH=$PATH:/usr/local/go/bin:/home/tiennguyen/projects/hyperledger/fabric-samples/bin
export GOPATH=$HOME/fabric
export FABRIC_CFG_PATH=/home/tiennguyen/projects/hyperledger/fabric-samples/config


# change to directory
cd home/tiennguyen/projects/hyperledger/fabric-samples/test-network
# start peer, oderer without channel
./network.sh up
# create channel
./network.sh up createChannel -c channel1
# create basis chaincode
./network.sh deployCC -c channel1 -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go
# connect peer0.org1 for doing
source ./org1.sh
# connect peer0.org2 for doing
source ./org2.sh
# Read the last state of all assets
peer chaincode query -C $CHANNEL_NAME -n basic -c '{"Args":["GetAllAssets"]}' | jq .
# Read an asset 
peer chaincode query -C $CHANNEL_NAME -n basic -c '{"Args":["ReadAsset","asset1"]}' | jq .
$ create an asset (data)
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C $CHANNEL_NAME -n basic --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"CreateAsset","Args":["asset1","green","10","TienNguyen","500"]}'
