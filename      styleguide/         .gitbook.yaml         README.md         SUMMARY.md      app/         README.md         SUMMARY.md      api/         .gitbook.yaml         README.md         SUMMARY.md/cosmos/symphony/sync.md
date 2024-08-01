# 🚅 Sync

**Snapshort:**&#x20;

```bash
sudo systemctl stop symphonyd

mv $HOME/.symphonyd/data/priv_validator_state.json $HOME/.symphonyd/priv_validator_state.json.backup 

symphonyd tendermint unsafe-reset-all --home $HOME/.symphonyd --keep-addr-book 
curl https://node39.top/testnet/symphony/symphony-20240750.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.symphonyd

mv $HOME/.symphonyd/priv_validator_state.json.backup $HOME/.symphonyd/data/priv_validator_state.json 

sudo systemctl restart symphonyd
sudo journalctl -u symphonyd -f --no-hostname -o cat
```

**State sync:**

```bash
systemctl stop symphonyd

symphonyd tendermint unsafe-reset-all --home $HOME/.symphonyd --keep-addr-book

peers="48e7d1e9d084adf0856f256665bdba1866573ac4@135.181.25.27:35656,6f2fbd9971c21eabe2c883c107ad2b7c48e1e3ad@116.202.32.209:45656,016eb93b77457cbc8793ba1ee01f7e2fa2e63a3b@136.243.13.36:29156,77ce4b0a96b3c3d6eb2beb755f9f6f573c1b4912@178.18.251.146:22656,9d4ee7dea344cc5ca83215cf7bf69ba4001a6c55@5.9.73.170:29156,7d1cf0710843670635a8e9c57f68fcf8a8809918@152.53.32.129:45656,afa7fe852d149205bf870d92947d06a0ae01fd74@113.166.212.185:20656,2629416e8e4624d42c7dd653b82ca3f401b18d05@37.27.133.17:35656,caaadda331ec062d75ca8d62333641c343e6b812@116.203.87.191:15656,7cf1c619a177fc622a5034e4d257c2fd1dc6ec7f@135.181.130.103:33656,021151a58f1cb3525cf2fc182d9443eb09e81181@65.109.83.40:29256,764d29fcc58b1f1d806e0c192bc2c39cadef784d@95.217.225.147:45656,710976805e0c3069662e63b9f244db68654e2f15@65.109.93.124:29256,ec7779545f6fb138fcddea05e8e9e87df0d2f92c@109.199.106.131:15656,c7397f07af182af2841b5fdcb063a7ed9761e1c7@159.69.154.105:15656,22e9b542b7f690922e846f479878ab391e69c4c3@57.129.35.242:26656,5a1a63b26e4082166775770891da28f23391beb3@37.27.127.107:29156,5bfb1bab99520ced649e6d25103c2dcc1db8b3e0@95.217.210.228:15656,39849d6f7f7cda441a7f41244d4a61c102e33987@109.123.241.176:45656,eb462ef39648a092733153f05916053e2c013860@193.34.212.80:35656,c3afc99e7420518e7d185b72f782549c93923aef@62.169.17.3:45656,01cacd46e1c54da342b9a576bff95298b47e1eb0@62.169.16.250:45656,67ac5a45b0f8762d02b6c373e73dfc2f472028a9@149.102.157.11:45656,d0291fff59890624630e4224bc6c48b2471914e8@44.198.174.33:26656,a4d4fc2d8e0b127d11f5c106ab241c475712d3f5@65.109.117.113:29256,0d4491b16b9c0b060b7b2fe9cacfdb54f7917719@37.27.134.110:42656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.symphonyd/config/config.toml

SNAP_RPC=https://symphony-testnet-rpc.node39.top:443

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.symphonyd/config/config.toml

systemctl restart symphonyd && journalctl -u nibid -f -o cat

```
