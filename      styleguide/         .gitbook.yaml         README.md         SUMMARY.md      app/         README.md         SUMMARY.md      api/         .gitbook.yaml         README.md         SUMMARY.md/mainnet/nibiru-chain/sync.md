# 🚅 Sync

**Download Snapshot:** Height 10182074 goleveldb | 4.6Gb

```bash
sudo systemctl stop nibid

cp $HOME/.nibid/data/priv_validator_state.json $HOME/.nibid/priv_validator_state.json.backup 

nibid tendermint unsafe-reset-all --home $HOME/.nibid --keep-addr-book 
curl https://file.node39.top/Mainnet/Nibiru/snapshot-nibid-10182074.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.nibid

cp $HOME/.nibid/priv_validator_state.json.backup $HOME/.nibid/data/priv_validator_state.json 

sudo systemctl restart nibid
sudo journalctl -u nibid -f --no-hostname -o cat
```

**State sync:**

```bash
systemctl stop nibid
nibid tendermint unsafe-reset-all --home $HOME/.nibid --keep-addr-book
peers="c416d67c3dbb2d30b803611469e6d2634099292d@135.181.210.171:11036,d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@65.108.233.103:13956,627766bb1d2f9ac3db9f9f3153c91b6d795d1028@89.58.30.164:26656,6c99d3c416f171c230a9891092827251194af006@5.78.83.5:13956,74f2e690e1be83c189bf227c4c61b266267795c8@94.130.138.48:35656,6604179787139eab744b8a1159fee9b03fcc3714@51.81.49.176:19856,cc8ff21a53f996fd729a10bcfbd85cf009505367@65.108.75.107:34656,81b9c09ae1c76a3e7f36db91b98d1fbf1e31233c@185.248.24.16:13956,4895f4acc4934823a17166c4d84aea2858b8f50a@65.108.199.120:37456,8b564f4ddd3e2eb24b1f321d7dacc03080c9c824@65.21.227.177:10026,807df0af03c7de32317eda4fe4dbdcc3ad4b4ae6@208.88.251.53:44441,98cadded622d291141f8a83972fa046267df94b6@38.109.200.36:44441,f0ccacd7cd19f7c30c203ca4c9cbee62d4f8f773@35.234.108.227:26656,8d8324141897243927359345bb4b1bb78a1e1df1@65.109.56.235:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nibid/config/config.toml
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
