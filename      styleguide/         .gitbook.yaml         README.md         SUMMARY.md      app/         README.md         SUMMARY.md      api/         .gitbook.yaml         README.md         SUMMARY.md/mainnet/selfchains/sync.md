# 🚅 Sync

**Snapshort: Height 688148 | 58Mb**

```
sudo systemctl stop selfchaind

mv $HOME/.selfchain/data/priv_validator_state.json $HOME/.selfchain/priv_validator_state.json.backup 

selfchaind tendermint unsafe-reset-all --home $HOME/.selfchain --keep-addr-book 
curl https://node39.top/Mainnet/Selfchain/selfchain_data.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME

cp $HOME/.selfchain/priv_validator_state.json.backup $HOME/.selfchain/data/priv_validator_state.json 

sudo systemctl restart selfchaind
sudo journalctl -u selfchaind -f --no-hostname -o cat
```

**State sync:**

```
sudo systemctl stop selfchaind

selfchaind tendermint unsafe-reset-all --home $HOME/.selfchain --keep-addr-book

SNAP_RPC="http://selfchain-rpc.node39.top:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.selfchain/config/config.toml

sudo systemctl restart selfchaind && sudo journalctl -u selfchaind -f --no-hostname -o cat
```
