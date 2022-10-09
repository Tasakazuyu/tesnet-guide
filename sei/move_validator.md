<p align="center">
<img height="180" height="auto" src="https://user-images.githubusercontent.com/109075185/194767660-667da0ee-1bf5-409e-a884-3cd4245bd109.png">

# Move your validator to another machine

### 1. Run a new full node on a new machine
To setup full node you can follow my guide [Sei node setup for testnet](https://github.com/Tasakazuyu/tesnet-guide/blob/main/sei/readme.md)

### 2. Before shutdown your machine, it's mandatory to backup your mnemonic

```
seid keys mnemonic
```
> _Or_
```
seid keys export --unarmored-hex --unsafe <walletname>
```
#### To get list of keys
```
seid keys list
```

### 3. Backup your private key validator
```
cat $HOME/.sei/config/priv_validator_key.json
```
> _copy your keys into notepad_


### 4. Recover your wallet of the old machine on the new machine

#### This can be done with the mnemonics
```
seid keys add wallet --recover
```
> _Paste your mnemonic_

### 5. Wait for the new full node on the new machine to finish catching-up

#### To check synchronization status
```
seid status 2>&1 | jq .SyncInfo
```
> _`catching_up` should be equal to `false`_
### 6. After the new node has caught-up, stop the validator node

> _To prevent double signing, you should stop the validator node before stopping the new full node to ensure the new node is at a greater block height than the validator node_
> _If the new node is behind the old validator node, then you may double-sign blocks_
#### Stop and disable service on old machine
```
sudo systemctl stop seid
sudo systemctl disable seid
```
> _The validator should start missing blocks at this point_
### 7. Stop service on new machine
```
sudo systemctl stop seid
```

### 8. Move the private key validator from the old machine to the new machine
#### In the new machine, copy your private key validator into: `~/.sei/config/priv_validator_key.json`

> _After being copied, the key `priv_validator_key.json` should then be removed from the old node's config directory to prevent double-signing if the node were to start back up_
```
sudo mv ~/.seid/config/priv_validator_key.json ~/.seid/bak_priv_validator_key.json
```
### 9. Start service on a new validator node
```
sudo systemctl start seid
```
> _The new node should start signing blocks once caught-up_
### 10. Make sure your validator is not jailed
#### To unjail your validator
```
seid tx slashing unjail --chain-id $SEI_CHAIN_ID --from mykey --gas=auto -y
```

### 11. After you ensure your validator is producing blocks and is healthy you can shut down old validator server
