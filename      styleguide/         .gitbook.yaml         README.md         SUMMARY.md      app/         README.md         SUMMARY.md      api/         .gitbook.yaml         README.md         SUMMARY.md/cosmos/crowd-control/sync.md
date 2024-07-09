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

peers="6284697cfb67571b4dd5b25c8b57dd1215900687@37.27.47.29:39656,86b643ba743ccc78e6e086120d43c96f85872601@202.61.225.157:20056"  
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
