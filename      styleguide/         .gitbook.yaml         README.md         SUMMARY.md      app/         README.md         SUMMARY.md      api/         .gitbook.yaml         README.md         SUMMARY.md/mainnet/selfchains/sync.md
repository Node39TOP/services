# 🚅 Sync

**Snapshort:** 03:30 UTC | Daily | **db:** goleveldb | **pruning**: 100/0/50 | **indexer**: null

```
sudo systemctl stop selfchaind

mv $HOME/.selfchain/data/priv_validator_state.json $HOME/.selfchain/priv_validator_state.json.backup 

selfchaind tendermint unsafe-reset-all --home $HOME/.selfchain --keep-addr-book 
SNAP_NAME=$(curl -s https://file.node39.top/Mainnet/Selfchain/ | egrep -o 'snapshot-selfchain-[0-9]+\.tar\.lz4' | sort -V | tail -n 1)
wget -c https://file.node39.top/Mainnet/Shentu/${SNAP_NAME} -O - | lz4 -dc - | tar -xf - -C $HOME/.selfchain

cp $HOME/.selfchain/priv_validator_state.json.backup $HOME/.selfchain/data/priv_validator_state.json 

sudo systemctl restart selfchaind
sudo journalctl -u selfchaind -f --no-hostname -o cat
```

**State sync:**

```
sudo systemctl stop selfchaind

cp $HOME/.selfchain/data/priv_validator_state.json $HOME/.selfchain/priv_validator_state.json.backup
selfchaind tendermint unsafe-reset-all --home $HOME/.selfchain --keep-addr-book

SNAP_RPC="https://selfchain-rpc.node39.top:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.selfchain/config/config.toml

mv $HOME/.selfchain/priv_validator_state.json.backup $HOME/.selfchain/data/priv_validator_state.json
sudo systemctl restart selfchaind && sudo journalctl -u selfchaind -f --no-hostname -o cat
```
