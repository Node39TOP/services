# ⚙️ Installation

**Install dependencies:**

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

**Install GO: (amd64 - x86)**

```bash
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

\
**Download binary Symphony:**

```bash
cd $HOME
rm -rf symphony
git clone https://github.com/Orchestra-Labs/symphony
cd symphony
git checkout v0.2.1
make install
```

**Set chain and Name Symhony:**\
_<mark style="color:red;">Change</mark>_ _<mark style="color:red;">\<Change-Name></mark>_&#x20;

```bash
symphonyd init <Change-Name> --chain-id symphony-testnet-2
```

**Custom Port: (Option)**

```bash
symphonyd config node tcp://localhost:39657
sed -i -e "s%:1317%:3917%; s%:8080%:39580%; s%:9090%:39590%; s%:9091%:39591%; s%:8545%:3945%; s%:8546%:39546%; s%:6065%:39565%" $HOME/.symphonyd/config/app.toml
sed -i -e "s%:26658%:39658%; s%:26657%:39657%; s%:6060%:3960%; s%:26656%:39656%; s%:26660%:39661%" $HOME/.symphonyd/config/config.toml
```

**Set min gas:**&#x20;

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = "0note"/" $HOME/.symphonyd/config/app.toml
```

**Set indexing: (Option)**&#x20;

```bash
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.symphonyd/config/config.toml
```

**Download Genesis & addressbook:**

```bash
wget -O $HOME/.symphonyd/config/genesis.json https://node39.top/testnet/symphony/genesis.json
wget -O $HOME/.symphonyd/config/addrbook.json https://node39.top/testnet/symphony/addrbook.json
```

**Peers:**

```bash
peers="a89590c95e43697eefb7530d98c73c90847ed833@95.217.128.197:39656,48e7d1e9d084adf0856f256665bdba1866573ac4@135.181.25.27:35656,6f2fbd9971c21eabe2c883c107ad2b7c48e1e3ad@116.202.32.209:45656,016eb93b77457cbc8793ba1ee01f7e2fa2e63a3b@136.243.13.36:29156,77ce4b0a96b3c3d6eb2beb755f9f6f573c1b4912@178.18.251.146:22656,9d4ee7dea344cc5ca83215cf7bf69ba4001a6c55@5.9.73.170:29156,7d1cf0710843670635a8e9c57f68fcf8a8809918@152.53.32.129:45656,afa7fe852d149205bf870d92947d06a0ae01fd74@113.166.212.185:20656,2629416e8e4624d42c7dd653b82ca3f401b18d05@37.27.133.17:35656,caaadda331ec062d75ca8d62333641c343e6b812@116.203.87.191:15656,7cf1c619a177fc622a5034e4d257c2fd1dc6ec7f@135.181.130.103:33656,021151a58f1cb3525cf2fc182d9443eb09e81181@65.109.83.40:29256,764d29fcc58b1f1d806e0c192bc2c39cadef784d@95.217.225.147:45656,710976805e0c3069662e63b9f244db68654e2f15@65.109.93.124:29256,ec7779545f6fb138fcddea05e8e9e87df0d2f92c@109.199.106.131:15656,c7397f07af182af2841b5fdcb063a7ed9761e1c7@159.69.154.105:15656,22e9b542b7f690922e846f479878ab391e69c4c3@57.129.35.242:26656,5a1a63b26e4082166775770891da28f23391beb3@37.27.127.107:29156,5bfb1bab99520ced649e6d25103c2dcc1db8b3e0@95.217.210.228:15656,39849d6f7f7cda441a7f41244d4a61c102e33987@109.123.241.176:45656,eb462ef39648a092733153f05916053e2c013860@193.34.212.80:35656,c3afc99e7420518e7d185b72f782549c93923aef@62.169.17.3:45656,01cacd46e1c54da342b9a576bff95298b47e1eb0@62.169.16.250:45656,67ac5a45b0f8762d02b6c373e73dfc2f472028a9@149.102.157.11:45656,d0291fff59890624630e4224bc6c48b2471914e8@44.198.174.33:26656,a4d4fc2d8e0b127d11f5c106ab241c475712d3f5@65.109.117.113:29256,0d4491b16b9c0b060b7b2fe9cacfdb54f7917719@37.27.134.110:42656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.symphonyd/config/config.toml
```

**Create Service:**

```bash
sudo tee /etc/systemd/system/symphonyd.service > /dev/null <<EOF
[Unit]
Description=symphony testnet node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which symphonyd) start
Restart=always
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable symphonyd
```

**Check sync: (**<mark style="color:red;">**False -> Done**</mark>**)**

```bash
sy status 2>&1 | jq .SyncInfo.catching_up
```

**Create Validator:**

```bash
symphonyd tx staking create-validator \
--amount 90000note \
--from wallet \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(symphonyd tendermint show-validator) \
--moniker "Node39.TOP Guide" \
--identity "xxxxxxxxx" \
--details "xxxxxxxxx" \
--website "xxxxxxxxx" \
--security-contact xxxxxxxxx \
--chain-id symphony-testnet-2 \
--fees=800note \
-y
```

**Edit Validator:**

```bash
symphonyd tx staking edit-validator \
--new-moniker="Node39.TOP Guide" \
--identity="xxxxxxxxx" \
--details="xxxxxxxxx" \
--chain-id=symphony-testnet-2 \
--commission-rate=0.1 \
--from=wallet \
--gas-prices=1node \
--gas-adjustment=1.5 \
--gas=auto \
-y
```
