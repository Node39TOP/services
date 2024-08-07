# 🚅 Sync

**Download snapshort:**

```bash
sudo systemctl stop wardend

mv $HOME/.warden/data/priv_validator_state.json $HOME/.warden/priv_validator_state.json

wardend tendermint unsafe-reset-all --home $HOME/.warden
rm -fR $HOME/.warden/wasm

curl https://buenavista-genesis.s3.eu-west-1.amazonaws.com/warden-snaphot.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.warden

mv $HOME/.warden/priv_validator_state.json $HOME/.warden/data/priv_validator_state.json

sudo systemctl restart wardend && sudo journalctl -u wardend -f --no-hostname -o cat
```

**State sync:**

```bash
sudo systemctl stop wardend

cp $HOME/.warden/data/priv_validator_state.json $HOME/.warden/priv_validator_state.json
wardend tendermint unsafe-reset-all --home $HOME/.warden

SNAP_RPC="https://warden-testnet-rpc.node39.top:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000));
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



