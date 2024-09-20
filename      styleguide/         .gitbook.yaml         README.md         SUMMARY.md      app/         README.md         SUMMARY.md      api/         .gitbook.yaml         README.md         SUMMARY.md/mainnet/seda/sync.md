# 🚅 Sync

**Snapshort:** 04:00 UTC | Daily | **db:** goleveldb | **pruning**: 100/0/50 | **indexer**: null

```bash
sudo systemctl stop sedad

cp $HOME/.sedad/data/priv_validator_state.json $HOME/.sedad/priv_validator_state.json.backup

sedad tendermint unsafe-reset-all --home $HOME/.sedad --keep-addr-book
SNAP_NAME=$(curl -s https://file.node39.top/Mainnet/Seda/ | egrep -o 'snapshot-seda-[0-9]+\.tar\.lz4' | sort -V | tail -n 1)
wget -c https://file.node39.top/Mainnet/Seda/${SNAP_NAME} -O - | lz4 -dc - | tar -xf - -C $HOME/.sedad
mv $HOME/.sedad/priv_validator_state.json.backup $HOME/.sedad/data/priv_validator_state.json

sudo systemctl restart sedad && sudo journalctl -u sedad -f --no-hostname -o cat
```

**Stale sync:**

```bash
sudo systemctl stop sedad

cp $HOME/.sedad/data/priv_validator_state.json $HOME/.sedad/priv_validator_state.json.backup
sedad tendermint unsafe-reset-all --home $HOME/.sedad

SNAP_RPC="https://seda-rpc.node39.top:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) 

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH && sleep 2

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ;
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.sedad/config/config.toml

cp $HOME/.sedad/priv_validator_state.json.backup $HOME/.sedad/data/priv_validator_state.json

sudo systemctl restart sedad && sudo journalctl -u sedad -f --no-hostname -o cat
```
