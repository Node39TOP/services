# Sync

**Download Snapshort:**

```
sudo systemctl stop junctiond
cp $HOME/.junction/data/priv_validator_state.json $HOME/.junction/priv_validator_state.json.backup
curl https://node39.top/testnet/airchains/snapshort-airchains.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.junction
mv $HOME/.junction/priv_validator_state.json.backup $HOME/.junction/data/priv_validator_state.json
sudo systemctl restart junctiond && sudo journalctl -u junctiond -f --no-hostname -o cat
```

**State sync:**

```
sudo systemctl stop junctiond
cp $HOME/.junction/data/priv_validator_state.json $HOME/.junction/priv_validator_state.json.backup
junctiond tendermint unsafe-reset-all --home ~/.junction/ --keep-addr-book

peers="de2e7251667dee5de5eed98e54a58749fadd23d8@34.22.237.85:26656,1918bd71bc764c71456d10483f754884223959a5@35.240.206.208:26656,48887cbb310bb854d7f9da8d5687cbfca02b9968@35.200.245.190:26656,de2e7251667dee5de5eed98e54a58749fadd23d8@34.22.237.85:26656,8b72b2f2e027f8a736e36b2350f6897a5e9bfeaa@131.153.232.69:26656,d92c7efcb453ba2edab6d80ad6e3b692e3a7e4f5@49.13.120.225:26656,5c5989b5dee8cff0b379c4f7273eac3091c3137b@57.128.74.22:56256,e09fa8cc6b06b99d07560b6c33443023e6a3b9c6@65.21.131.187:26656,0305205b9c2c76557381ed71ac23244558a51099@162.55.65.162:26656,3e5f3247d41d2c3ceeef0987f836e9b29068a3e9@168.119.31.198:56256,086d19f4d7542666c8b0cac703f78d4a8d4ec528@135.148.232.105:26656,976a0fe0a0fa205478beb66addaae3842907c3f6@37.27.48.77:32656,7d6694fb464a9c9761992f695e6ba1d334403986@164.90.228.66:26656,b2e9bebc16bc35e16573269beba67ffea5932e13@95.111.239.250:26656,23152e91e3bd642bef6508c8d6bd1dbedccf9e56@95.111.237.24:26656,c1e9d12d80ec74b8ddbabdec9e0dad71337ba43f@135.181.82.176:26656,3b429f2c994fa76f9443e517fd8b72dcf60e6590@37.27.11.132:26656,84b6ccf69680c9459b3b78ca4ba80313fa9b315a@159.69.208.30:26656,e78a440c57576f3743e6aa9db00438462980927e@5.161.199.115:26656,49fb1316b22c71e455720af15dd552dafb9af39a@5.189.151.175:26656,e831fa909cce0d1807cfcf417e28e782530f5c94@161.97.66.254:26666,db38d672f66df4de01b26e1fa97e1632fbfb1bdf@173.249.57.190:26656,08a0014125bded5fe76b9dc3275b0a58b6841b43@116.203.184.36:26656,6025c1523ad0cd6926ef277cfcf46d82ebb43c21@116.203.24.46:26656,fed2e80e159a23bf9f71f980b501c2304cec2f6d@185.194.216.61:17656,1ad9bdeac0b06f585a9c64261d0705c4cbfd28e7@144.91.99.93:26656,a6384bd23bd728ffa9a8452b12fc865dadf51672@81.200.154.160:26656,5b31fdf605645b44ad615c8b79b1550540895fe5@35.214.147.230:26656,6a3a13d7631823eb6dcd00882243c913c819a125@38.242.196.100:26656,3e182e463425dfa6d4cef83f4bdd67c98c36eba3@195.26.243.208:47656,c97c7a9c11cf3cb059ce89c36f7ff219daa3ada4@195.26.246.26:26656,7d162ef2392d25720d7cdb2cfdf2ccf146e32bac@49.13.234.149:26656,5a161464aa73571f1b7e22204bcf3bdb6fb71f3b@195.26.241.184:26656,bff7c802021ed3b4aaf222da9d42280bfc5dad88@65.109.139.181:26656,c284cbda3ab197001136c39c9df8e45af2038513@34.93.143.222:26656,449297568d9d6d4aa51a93f7a1b1e92e1ec38619@65.108.242.9:26656,611440c7193678ec1cd0c60b55abfca07dfa27cd@95.217.161.97:26656,beec52199d4b28cab6fc3b2f2a2718c6667ac46a@37.27.19.95:26656,226b9c42e81cddd185d435348cd89f87fee37279@135.181.42.46:26656,479b63df84e247c55e80cccd9abbf7100a334fcc@65.108.156.83:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nibid/config/config.toml

SNAP_RPC="https://airchains-testnet-rpc.node39.top:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" ~/.junction/config/config.toml
more ~/.junction/config/config.toml | grep 'rpc_servers'
more ~/.junction/config/config.toml | grep 'trust_height'
more ~/.junction/config/config.toml | grep 'trust_hash'

sudo mv $HOME/.junction/priv_validator_state.json.backup $HOME/.junction/data/priv_validator_state.json

sudo systemctl restart junctiond && journalctl -u junctiond -f -o cat
```
