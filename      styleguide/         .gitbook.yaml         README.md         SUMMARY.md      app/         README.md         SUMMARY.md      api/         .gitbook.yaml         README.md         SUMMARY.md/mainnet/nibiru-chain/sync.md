# 🚅 Sync

**Download Snapshot: Daily** | **db:** goleveldb | **pruning**: 100/0/50 | **indexer**: null

```bash
sudo systemctl stop nibid 

cp $HOME/.nibid/data/priv_validator_state.json $HOME/.nibid/priv_validator_state.json.backup

nibid tendermint unsafe-reset-all --home $HOME/.nibid --keep-addr-book
SNAP_NAME=$(curl -s https://file3.node39.top/Mainnet/Nibiru/ | egrep -o 'snapshot-nibiru-[0-9]+\.tar\.lz4' | sort -V | tail -n 1)
wget -c https://file3.node39.top/Mainnet/Nibiru/${SNAP_NAME} -O - | lz4 -dc - | tar -xf - -C $HOME/.

mv $HOME/.nibid/priv_validator_state.json.backup $HOME/.nibid/data/priv_validator_state.json

sudo systemctl restart nibid && sudo journalctl -u nibid -f --no-hostname -o cat

```

**State sync:**

```bash
systemctl stop nibid

nibid tendermint unsafe-reset-all --home $HOME/.nibid --keep-addr-book

SNAP_RPC=https://nibiru-rpc.node39.top:443
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.nibid/config/config.toml

systemctl restart nibid && journalctl -u nibid -f -o cat
```
