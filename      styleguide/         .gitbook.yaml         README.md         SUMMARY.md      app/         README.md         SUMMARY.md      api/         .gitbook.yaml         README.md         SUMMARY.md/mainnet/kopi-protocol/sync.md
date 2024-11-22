# 🚅 Sync

**Download Genesis & addressbook:**

```bash
wget -O $HOME/.kopid/config/genesis.json https://file.node39.top/Mainnet/Kopi/genesis.json
wget -O $HOME/.kopid/config/addrbook.json https://file.node39.top/Mainnet/Kopi/addrbook.json
```

**Download snapshot: snapshort**: Auto snapshot 0h00 UTC and 12h00 UTC | **Daily** | **db**: goleveldb | **pruning**: 100/0/50 | **indexer**: null

```bash
sudo systemctl stop kopid 

cp $HOME/.kopid/data/priv_validator_state.json $HOME/.kopid/priv_validator_state.json.backup

kopid tendermint unsafe-reset-all --home $HOME/.kopid --keep-addr-book
SNAP_NAME=$(curl -s https://file.node39.top/Mainnet/Kopi/ | egrep -o 'snapshot-kopi-[0-9]+\.tar\.lz4' | sort -V | tail -n 1)
wget -c https://file.node39.top/Mainnet/Kopi/${SNAP_NAME} -O - | lz4 -dc - | tar -xf - -C $HOME/.kopid

mv $HOME/.kopid/priv_validator_state.json.backup $HOME/.kopid/data/priv_validator_state.json

sudo systemctl restart kopid && sudo journalctl -u kopid -f --no-hostname -o cat
```

**State sync:**

```bash
sudo systemctl stop kopid

cp $HOME/.kopid/data/priv_validator_state.json $HOME/.kopid/priv_validator_state.json
kopid tendermint unsafe-reset-all --home $HOME/.kopid

SNAP_RPC="https://kopi-rpc.node39.top:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) 

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH && sleep 2

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ;
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.kopid/config/config.toml

mv $HOME/.kopid/priv_validator_state.json $HOME/.kopid/data/priv_validator_state.json

sudo systemctl restart kopid && sudo journalctl -u kopid -f --no-hostname -o cat
```



