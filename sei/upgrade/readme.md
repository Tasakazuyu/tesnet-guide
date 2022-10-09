<p align="center">
<img height="180" height="auto" src="https://user-images.githubusercontent.com/109075185/194769876-e347730b-1935-40c3-b73f-551b2537142b.png"
 </p>

# Chain upgrade to commit 1.2.2beta!

## (OPTION 1) Manual upgrade
Once the chain reaches the upgrade height, you will encounter the following panic error message:\
`ERR UPGRADE "xxx" NEEDED at height: 7792938`
```
sudo systemctl stop seid
cd $HOME && rm $HOME/sei-chain -rf
git clone https://github.com/sei-protocol/sei-chain.git && cd $HOME/sei-chain
git checkout 1.2.2beta
make install
mv $HOME/go/bin/seid    $HOME/.sei/cosmovisor/upgrades/1.2.2beta/bin/seid
sudo systemctl restart seid && journalctl -fu seid -o cat
```

!!! DO NOT UPGRADE BEFORE CHAIN RECHES THE BLOCK `7792938`!!!

### (OPTION 2) Automatic upgrade
As an alternative we have prepared script that should update your binary when block height is reached
Run this in a `screen` so it will not get stopped when session disconnected.
```
wget -O upgrade.sh https://raw.githubusercontent.com/Tasakazuyu/tesnet-guide/main/sei/upgrade/upgrade.sh && chmod +x upgrade.sh && ./upgrade.sh
```
