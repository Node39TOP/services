# 🟢 Shentu Chain

**Hardware requirements:**&#x20;

* 8 cores
* 32GB RAM
* 500 GB SSD NVMe
* Ubuntu 22 - x86 or arm

\
Website: [https://www.shentu.technology/](https://www.shentu.technology/)\
Telegram: [https://t.me/shentu\_chain](https://t.me/shentu\_chain)\
Discord: [https://discord.gg/6nfAk956NH](https://discord.gg/6nfAk956NH)\
Github: [https://github.com/shentufoundation/](https://github.com/shentufoundation/)\
X: [https://x.com/ShentuChain](https://x.com/ShentuChain)\
\
Node39 support:

* [x] RPC: [https://shentu-rpc.node39.top](https://shentu-rpc.node39.top)
* [x] API: [https://shentu-api.node39.top](https://shentu-api.node39.top)
* [x] Snapshort: Every 24 hours
* [x] Dashboard: [https://dashboard.node39.top/shentu/staking/shentuvaloper1k7sjdr27evwgj36u08uprtfmjndz0mqlmfq898](https://dashboard.node39.top/shentu/staking/shentuvaloper1k7sjdr27evwgj36u08uprtfmjndz0mqlmfq898)
* [x] Staking: [https://wallet.keplr.app/chains/shentu?modal=validator\&chain=shentu-2.2\&validator\_address=shentuvaloper1k7sjdr27evwgj36u08uprtfmjndz0mqlmfq898](https://wallet.keplr.app/chains/shentu?modal=validator\&chain=shentu-2.2\&validator\_address=shentuvaloper1k7sjdr27evwgj36u08uprtfmjndz0mqlmfq898)

#### &#x20;<a href="#install-dependencies" id="install-dependencies"></a>

{% hint style="success" %}
Update version 2.10.0

```
cd $HOME && rm -rf shentu && git clone https://github.com/shentufoundation/shentu && cd shentu && git checkout v2.10.0 && make install && shentud version
```
{% endhint %}

#### Install Dependencies: <a href="#install-dependencies" id="install-dependencies"></a>

```
sudo apt update && sudo apt upgarade -y
sudo apt-get install git curl build-essential make jq gcc snapd chrony lz4 tmux unzip bc -y
```

**Install GO: (amd64 - x86)**

```
rm -rf $HOME/go
sudo rm -rf /usr/local/go
cd $HOME
curl https://dl.google.com/go/go1.21.8.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf -
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF
source $HOME/.profile
go version
```

**Install GO: (arm64)**\


```
rm -rf $HOME/go
sudo rm -rf /usr/local/go
cd $HOME
curl https://dl.google.com/go/go1.21.8.linux-arm64.tar.gz | sudo tar -C/usr/local -zxvf -
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF
source $HOME/.profile
go version
```

**Install Shentu chain:**

```
cd $HOME
rm -rf shentu
git clone https://github.com/shentufoundation/shentu
cd shentu
git checkout v2.10.0
make install
shentud version
```

**Set chain, min fees and Name Shentu chain:**\
_<mark style="color:red;">Change</mark>_ _<mark style="color:red;">\<Change-Name></mark>_&#x20;

```
shentud init <Change-Name> --chain-id=shentu-2.2
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0uctk\"|" $HOME/.shentud/config/app.toml
```

**Download Genesis & addressbook:**

```
curl -Ls https://node39.top/Mainnet/Shentu/genesis.json > $HOME/.shentud/config/genesis.json 
curl -Ls https://node39.top/Mainnet/Shentu/addrbook.json > $HOME/.shentud/config/addrbook.json
```

**Create Service:**

```
sudo tee /etc/systemd/system/shentud.service > /dev/null <<EOF
[Unit]
Description=shentud
After=network-online.target
[Service]
User=$USER
ExecStart=$(which shentud) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable shentud
```

**Seeds & Peer:**

```
SEEDS="bc9bbcae77a09b41417f597965f6fcbb8b280892@52.71.99.85:26656,fd2944af442b18dab4ce50d8e001816a38490d56@54.158.108.97:26656,3edd4e16b791218b623f883d04f8aa5c3ff2cca6@shentu-seed.panthea.eu:36656,258f523c96efde50d5fe0a9faeea8a3e83be22ca@seed.shentu-2.2.shentu.aviaone.com:10270,ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@seeds.polkachu.com:14056"
PEERS="1a3ac3e4a89c776c286df0c860313e5b9192354b@3.237.179.245:26656,40d3832c2f6409e039c01ab9494c7d705fe54dc8@213.136.80.20:26656,f36a8abd833ba375029d219cb4f3e53f4dfe1e14@146.59.81.204:61656,b53c36775ff9bc7bfc084df1cf633bda61735297@3.72.14.179:26656,dcceb7e119765d6ff54cb16fef8d008ba9099d56@52.202.184.217:26656,be3d05b4042d3b2404926fec1d37fe42ef455f63@135.181.163.185:26656,9c0b20c318d0ee8f84475ad47afed59b24ba2ea4@95.217.193.17:26656,40d3832c2f6409e039c01ab9494c7d705fe54dc8@213.136.80.20:26656,1480912d16f26b5ea1c4fea2496da95e44cbe845@65.109.115.226:14056,0d80e7cbdffd8a1db1477805ff51a2baf6268f0d@164.90.229.157:26656,d2577e282ec623168015ed7ccf4dc33c3fb07007@44.192.97.59:26656,a617ffcbaed1e55cb512f097a606b2c4dff14136@65.21.54.44:26656,147eeac0de54a973ade15e46ca427b70d0d535b2@135.181.128.114:14056,100aee4f6928d09e3dddfd0c5028cf127509bbd9@162.55.132.48:15607,e726816f42831689eab9378d5d577f1d06d25716@169.155.47.6:26656,b212d5740b2e11e54f56b072dc13b6134650cfb5@164.152.160.117:26656,0494d17e2cbe835c7e85a073c7c4f0b6dc17d834@31.7.196.49:26656,451f7656774f02e1356eb609ed31dec1c9566751@46.4.23.225:28656,55ef5099bb61b22c97a4c95c0361b0500654cbce@69.197.6.12:26656,c88951db960e645b494acb45bd50fe97ec40657f@91.207.54.63:26656,f092c40e4e2a7ab48cfcba38ffa61b6ee04b0b83@150.136.10.254:26656,4b0301042aa41757317681f5b4980e7e24dd3120@69.197.45.10:26656,f845d2ddfc081b61ffde641d29bda04c81915ea5@44.203.246.233:26656,61f11f0dcba923ce8fd4a4b1f32a4d4a62698b87@144.76.97.251:40656,dcceb7e119765d6ff54cb16fef8d008ba9099d56@52.202.184.217:26656,35113819447f4d86b7cccd4e2c429c5c37aa89e4@51.81.49.59:14056,065f66f818c1dfd41cf6aa434c21056339b5528d@136.243.39.175:36656,62501750a5244160817a8b510d7b2ecebfd884dc@3.219.242.171:26656,ea3c94549fd01ec3b7ba17db50b98e94e2170527@69.197.54.12:26656,441b736f12d4621f325a5f5a6622681518d50e2c@38.242.208.170:26656,db927f396ebc0cef65729961c732a19821834226@69.197.44.12:26656,f97807210f9547b8a5016fb18000b46072ca5e30@135.181.113.227:2407,baa8cfcad0eff850ef4e0f159bb9b4af620ae019@202.55.85.83:26656,fe394717ce027f33ef6efddbf7cada94d2b0eb9e@3.238.157.164:26656,7f70fb892b68a0a578282683512528aa860b428d@69.197.14.8:26656,94e911d79176c2ac90ce545b212429460dd34d5e@35.74.10.164:6656,4ba3f83efc53c834ba27eb22452840ee74aecf45@85.215.105.19:15604,fe3b71b0628a9af8625ffd05abeb3bafe9d2226f@168.119.240.200:46656,1ab529c3d771d5c982f5354e6a9fdb9b19be6839@51.222.40.84:26664,4d02f9868a20b81f4271f44dfc04d0bb2b64d699@23.20.192.115:26656,6146648cd0fee9f2e90eedb255c6c315bcee041b@178.63.93.41:26656,2d4cd12b345e995177e85e90a7d35d17115849c7@85.190.254.32:26656,a4b109d0b27e917829c34d02ef17f8701e7887e0@185.147.80.82:26656,94022bb1d17695c361f50253dfb927a414653b3a@87.20.12.119:26656,c9d9bce831cf34bb3b7056463dc2c02e59a1fc3c@23.88.71.160:36656,13e5b092aa878dcc54456040e12e57521124bb5c@85.10.203.212:26656,57715cae6d2cfe621dfc501f156063cb466d4aa8@65.108.126.22:26656,bc9bbcae77a09b41417f597965f6fcbb8b280892@52.71.99.85:26656,648c22816fdd2dc41fca47bde27f74b68b2886de@95.217.35.111:28656,89757803f40da51678451735445ad40d5b15e059@169.155.169.81:26656,bc1d1645903a3ace446e1168c8efb3f634268f0e@153.139.245.108:26656,722370de4cb68e3bcc7133b50e2c0e03110026de@209.145.56.75:26656,77a8840f2209400fb83e32472e47833af8d73751@69.197.43.14:28656,2ab1b30a04a1dce6ff50cab40fc0ef690eb048ea@51.210.99.161:26956,20f0daee37280bad3befc654171a4bde0d7ecd06@44.193.197.59:26656,62c55f070a395531f1f189a30e26b08aec6246de@51.159.16.49:5000,2e7d487185c430f2e684a6035c4040c717ed0367@144.91.65.13:26686,b6d870a3925baf56a70ea4d0a6a86f71d021257c@31.220.77.51:26656,cb230cbf82f18116269a000726e9b5f47c3c049f@129.213.155.54:26656,8f91d396e3395210ef3a8394d48d1888af6d01cd@142.132.202.86:56656,c645b957be937d01f2237e68b8e89835698b1a99@185.147.80.4:26656,6aacce771b4b7cb7a2e72a1eb433587ba29f9329@136.243.174.45:30022,1fa010a89dedf7dbb91c8239a4fe00c14ffe8715@161.97.133.184:26656,70afecb1dd5c79a378a47ce8bd5197c97997280c@23.88.72.34:14031,9755cab2585a2794453a5b396ef13b893393366f@65.108.212.224:46656,7e50d68fa1c3884cdcfe680ee1012fe29dcae3b0@69.197.44.4:26656,6b4ad002b3cd0dfd5d814d09622d25719a172ac5@89.233.109.34:26656,b1b97b6c72b65a41812d1a9057b2113dc6f1cebe@3.222.215.217:26656,039c186c6f4a323a6c840f4f7c17dce8d8b4bfb0@141.94.139.219:27056,ceda6325ce9ab8ab1f2f35af4b23d73c9b6dd417@144.76.63.67:26059,4432db62fd207b1a1f876beaae7826c53ad92e2f@5.161.47.237:26656,617e462c1b6a6845146e9318a8ff9b5ee324d26b@103.160.95.238:26656,18a9e4366f40ff32077478d6b99c84d0cce15825@158.247.201.157:15200,e1b058e5cfa2b836ddaa496b10911da62dcf182e@134.65.192.150:26656,20157e5c6538f1750618972db3c0d171dae8cf8c@94.130.90.82:26656,37d26ed4d655c3bd0d29987e501b969d8d3fac61@195.201.172.9:15607,5615e99d54ec4f46d1b398fcefc6d1276416e29c@148.113.162.234:29556,17ce46252e671cc23c50248b7a69a6be5452bb7d@18.195.96.54:26656,f3500190fbd2dadbe1df11a5bcba9034bc271586@69.197.54.28:26656,3692f4a70a36353dd2f5b1f0eb7ca38d3bde8748@135.181.208.166:27656,43b923d403b569575fbee4eef1c0fb0c5d39be2f@165.232.72.33:26656,c124ce0b508e8b9ed1c5b6957f362225659b5343@169.155.168.219:26656,f8701d61f5cfb8c6ba4a9cc985f0c1079c380a39@198.244.167.22:5000,077061805d63e5382dd5f6f2e941e58ca647368f@94.250.202.43:26656,1a3ac3e4a89c776c286df0c860313e5b9192354b@3.237.179.245:26656,0019800c27866bacf7193711034e335daff7fe79@65.109.122.105:61256,4dff83b8e2170e6fd9d027e2a092d52875c6c589@69.197.23.4:26656,fd2944af442b18dab4ce50d8e001816a38490d56@54.158.108.97:26656,5b73f98db91d006f7da1db22244bc316f6b3742e@95.111.244.78:26706,bb514a32edf3987353f02608af6ffc6d0918ed01@157.230.113.147:32289,e1754812621b14c4a993dd354a85421538284da4@89.58.59.44:26656,e4023e76021877584ab466c38d6c380a1bc72983@65.109.111.29:28656,6d536d8c75f09f33ad628d0fc12a536655aadbfe@144.168.47.18:26656,35cfb70f821827044a1e86c0bc7125f3043fb5bf@5.9.145.125:15604,74ceb11da633e2526388ab363c0f7ed9ba699459@35.75.32.253:6656,9442e09afb9b2fbfdcb3a0b7ba3126f0c22edaa3@161.97.90.102:26656,6f9bbce23f674829d8a5c306f5c75c21b7fead9b@78.47.87.115:26656,1f289695a5350759c597157df430908ce4468d94@34.87.190.126:26656,6da74ceece09b2864cadc4809054d435572992c8@74.225.248.193:26656,e5265d91570e7e16061449b88885c2586d9ca1bd@23.88.69.22:28366,3b387aeb1b5b9e01d132b425b27ec66723b6e2dd@100.27.47.192:26656,9023d9a3d60f147514129aabe6f6b60cfa4ee128@194.195.213.37:26656,c69d7772b7e87ce3df72fbb37a9eaab4c5375bcf@71.236.119.108:19656,24ceea5be19c46cc1354eaaff3db09468c60e422@142.132.248.214:26656,8b54a682382d7bd02d6b11660ed4a2446ce083ac@65.108.199.79:20656,bd5c7dda3f36b385927aa3054826c85dd81b8d2e@202.61.228.242:26656,fa275f5a5c20b8359cc65fb9848cc03a598ec5b6@85.237.193.98:26656,7e1828f4f8e5e02b540794df92d8d184c144985b@3.234.223.224:26656,8ba55b4582d898c79826af821c23854f8b4ec646@207.244.245.6:36656,a605e6fa81adf6e510da9a819103e4244d97cdff@54.241.84.226:15200,bb31faf9513baad299d35f9e01a0b5c2caf5c626@3.231.58.250:26656,e94daf30e209924bbbdac9d8f1f9fb5d2eb8c01f@35.153.156.23:26656,7bc3ca3cd6d5360ad2ac59949128b7eeadd9bdac@178.63.72.80:26656,1431ea1dd3ccf9363eaca5a464c19d6dfd2696c2@49.12.102.219:26656,d107488e238cee8bab76d69a321536ba8ad6f6d7@54.145.104.169:26656,d4cbdef21bde1fde444cd31f5a2842c76268f210@94.250.203.213:26656,a074234450b43bb260d93210d6d02d99154c5c7b@139.177.199.173:26656,207c47bed435e4174844064ef3f51ca35b059de2@5.189.128.119:26656,be10213a7f4ab416377a4ae3413f50392d5e2276@159.69.171.167:26656,9abf31887c1be97cafac77c58644acbbffce0639@69.197.46.13:26656,cdbe37fc5ea42278285dceb90a4fc35e88d1bde9@47.75.91.185:28656,ba2df0fafe7f4fd3c8e7415b996c278cb36bf423@65.108.202.146:26640,042982b3aaa361ca5a73e427c2478da45870850f@116.96.47.195:26656,cc96e64aa8dce28907a551ba6ad347ae3c3ad3d7@185.162.249.161:30656"
sed -i 's|^seeds *=.*|seeds = "'$SEEDS'"|; s|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.shentud/config/config.toml
```

**Download snapshort:**&#x20;

```
sudo systemctl stop shentud 
cp $HOME/.shentud/data/priv_validator_state.json $HOME/.shentud/priv_validator_state.json.backup
shentud tendermint unsafe-reset-all --home $HOME/.shentud --keep-addr-book
wget -c https://node39.top/Mainnet/Shentu/snap_shentu.tar.gz -O - | tar -xz -C $HOME/.shentud
mv $HOME/.shentud/priv_validator_state.json.backup $HOME/.shentud/data/priv_validator_state.json
sudo systemctl restart shentud && sudo journalctl -u shentud -f --no-hostname -o cat
```

**State sync:**

```
sudo systemctl stop shentud 
SNAP_RPC="https://rpc.shentu-2.2.shentu.aviaone.com:443"
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i "/\[statesync\]/, /^enable =/ s/=.*/= true/;\
/^rpc_servers =/ s|=.*|= \"$SNAP_RPC,$SNAP_RPC\"|;\
/^trust_height =/ s/=.*/= $BLOCK_HEIGHT/;\
/^trust_hash =/ s/=.*/= \"$TRUST_HASH\"/" $HOME/.shentud/config/config.toml

sudo systemctl restart shentud && sudo journalctl -u shentud -f --no-hostname -o cat
```

**Check sync:  (False -> Done)**

```
shentud status 2>&1 | jq .SyncInfo
```

**Wallet:**

```
// Check balance
shentud q bank balances wallet

// Add new Wallet
shentud keys add wallet

// Recover existing key
shentud keys add wallet --recover

//List All Keys
shentud keys list
```

**Unjail:**

```
shentud tx slashing unjail --from wallet --chain-id=shentu-2.2 -y
```
