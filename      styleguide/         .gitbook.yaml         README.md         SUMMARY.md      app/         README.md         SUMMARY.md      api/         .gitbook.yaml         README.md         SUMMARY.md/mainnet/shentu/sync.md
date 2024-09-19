# 🚅 Sync

**Download snapshort: Height 20355055**

```
sudo systemctl stop shentud 
cp $HOME/.shentud/data/priv_validator_state.json $HOME/.shentud/priv_validator_state.json.backup
shentud tendermint unsafe-reset-all --home $HOME/.shentud --keep-addr-book
wget -c https://file.node39.top/Mainnet/Shentu/snapshot-shentu-20355055.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.shentud
mv $HOME/.shentud/priv_validator_state.json.backup $HOME/.shentud/data/priv_validator_state.json
sudo systemctl restart shentud && sudo journalctl -u shentud -f --no-hostname -o cat
```

**State sync:**

```
sudo systemctl stop shentud 
SNAP_RPC="https://shentu-rpc.node39.top:443"
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i "/\[statesync\]/, /^enable =/ s/=.*/= true/;\
/^rpc_servers =/ s|=.*|= \"$SNAP_RPC,$SNAP_RPC\"|;\
/^trust_height =/ s/=.*/= $BLOCK_HEIGHT/;\
/^trust_hash =/ s/=.*/= \"$TRUST_HASH\"/" $HOME/.shentud/config/config.toml

sudo systemctl restart shentud && sudo journalctl -u shentud -f --no-hostname -o cat
```

