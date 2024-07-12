# 🚅 Sync

**Snapshort:**

```
sudo systemctl stop Cardchaind

cp $HOME/.cardchaind/data/priv_validator_state.json $HOME/.cardchaind/priv_validator_state.json.backup

rm -rf $HOME/.cardchaind/data 
curl https://node39.top/testnet/crowdcontrol/snap-crowdcontrol.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.cardchaind

mv $HOME/.cardchaind/priv_validator_state.json.backup $HOME/.cardchaind/data/priv_validator_state.json

sudo systemctl restart Cardchaind
sudo journalctl -u Cardchaind -f
```

**State sync:**

```
sudo systemctl stop Cardchaind

cp $HOME/.cardchaind/data/priv_validator_state.json $HOME/.cardchaind/priv_validator_state.json.backup
Cardchaind tendermint unsafe-reset-all --home $HOME/.cardchaind

peers="fd6cf9438cfafe4a1fc35bb20456a856328febaa@37.27.47.29:39656,35c8779026ceb17659b722b6a768e5a7f070c770@84.247.161.158:31656,86fe149f801ac75213179be5b56fbd1a1e545c43@202.61.225.157:20656"  
SNAP_RPC="https://crowdcontrol-testnet-rpc.node39.top:443"

sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.cardchaind/config/config.toml 

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) 

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH && sleep 2

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ;
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.cardchaind/config/config.toml

mv $HOME/.cardchaind/priv_validator_state.json.backup $HOME/.cardchaind/data/priv_validator_state.json

sudo systemctl restart Cardchaind && sudo journalctl -u Cardchaind -f
```
