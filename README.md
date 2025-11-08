# AZTEC-UPDATE-FOR-DOCKER V2.1.2

## 1.DOCKER COMPOSE.YML VERSION CHANGE
```
cd ~/aztec
```
```
docker compose down 
```
```
rm -rf /root/.aztec/testnet/data
```
```
sed -i 's|^ *image: aztecprotocol/aztec:.*|    image: aztecprotocol/aztec:2.1.2|' docker-compose.yml
```

```
sed -i 's|--network alpha-testnet|--network testnet|g' docker-compose.yml 
```
```
docker compose pull
```
## 2. INSTALL AZTEC CLI (FOR WALLET GENERATION AND ADD VALIDATOR COMMAND)
```
bash -i <(curl -s https://install.aztec.network)
```
```
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc

source ~/.bashrc
```
## 3. GENERATE NEW WALLET  
```
aztec validator-keys new   --fee-recipient 0x0000000000000000000000000000000000000000000000000000000000000000
```
You will see eth public adress and bls adress

<img width="507" height="96" alt="image" src="https://github.com/user-attachments/assets/797ca104-4532-4cef-bb39-40f3aa5f59ab" />


## 4.CREATE KEYS FOLDER AND MOVE KEYSTORE.JSON TO DOCKER ENVIRONMENT
```
mkdir /root/aztec/keys
```
```
cp ~/.aztec/keystore/key1.json /root/aztec/keys/keystore.json
```
### Save your keystore.json 

## 5. APPROVE 200K SPENDING TO JOIN THE NETWORK
```
cast send 0x139d2a7a0881e16332d7D1F8DB383A4507E1Ea7A "approve(address,uint256)" 0xebd99ff0ff6677205509ae73f93d0ca52ac85d67 200000ether --private-key "$PRIVATE_KEY_OF_OLD_SEQUENCER" --rpc-url YOUR ETH RPC URL
```

## 6.  JOIN THE NETWORK WITH AZTEC CLI
```
aztec \
  add-l1-validator \
  --l1-rpc-urls $ETH_RPC \
  --network testnet \
  --private-key $PRIVATE_KEY_OF_OLD_SEQUENCER \
  --attester $ETH_ATTESTER_ADDRESS \
  --withdrawer $ANY_ETH_ADDRESS \
  --bls-secret-key $BLS_ATTESTER_PRIV_KEY \
  --rollup 0xebd99ff0ff6677205509ae73f93d0ca52ac85d67

```

 ##### $ETH_RPC = Your Eth Rpc URL
 ##### $PRIVATE_KEY_OF_OLD_SEQUENCER= Your old private key of sequencer(which have 200k)
 ##### $ETH_ATTESTER_ADDRESS = Newly generated eth public adress which generated in step 3
 ##### $ANY_ETH_ADDRESS = You can use either your old eth adress or new adress(any adress)
 ##### $BLS_ATTESTER_PRIV_KEY = Newly generated wallet porivate keys which defined in step 4.You can see your private key via below command
 ```
 nano /root/aztec/keys/keystore.json
```

## START SEQUENCER AND CHECK THE LOGS
```
docker compose up -d
```
```
docker compose logs -fn 1000
```
 
