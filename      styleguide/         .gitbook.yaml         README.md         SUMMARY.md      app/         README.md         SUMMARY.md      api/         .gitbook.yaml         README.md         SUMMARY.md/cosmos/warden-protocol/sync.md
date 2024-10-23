# 🚅 Sync

**Download Genesis & addressbook:**

```bash
wget -O $HOME/.warden/config/genesis.json https://file.node39.top/testnet/warden/genesis.json
wget -O $HOME/.warden/config/addrbook.json https://file.node39.top/testnet/warden/addrbook.json
```

**Download Wasm**:

```bash
rm -rf $HOME/.warden/data $HOME/.warden/wasm
curl https://file.node39.top/testnet/warden/wasm-warden.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.warden
```

**Download snapshot:** Height 6915

```bash
sudo systemctl stop wardend

mv $HOME/.warden/data/priv_validator_state.json $HOME/.warden/priv_validator_state.json.backup

rm -rf $HOME/.warden/data $HOME/.warden/wasm
curl https://file.node39.top/testnet/warden/snapshot-warden-6915.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.warden

mv $HOME/.warden/priv_validator_state.json.backup $HOME/.warden/data/priv_validator_state.json

sudo systemctl restart wardend && sudo journalctl -u wardend -f --no-hostname -o cat
```

**State sync:**

```bash
sudo systemctl stop wardend

cp $HOME/.warden/data/priv_validator_state.json $HOME/.warden/priv_validator_state.json
wardend tendermint unsafe-reset-all --home $HOME/.warden

SNAP_RPC="https://warden-testnet-rpc.node39.top:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) 

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH && sleep 2

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ;
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.warden/config/config.toml

mv $HOME/.warden/priv_validator_state.json $HOME/.warden/data/priv_validator_state.json

sudo systemctl restart wardend && sudo journalctl -u wardend -f --no-hostname -o cat
```



