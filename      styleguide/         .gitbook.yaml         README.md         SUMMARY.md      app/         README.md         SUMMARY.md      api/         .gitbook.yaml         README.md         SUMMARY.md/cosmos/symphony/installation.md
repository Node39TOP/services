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
peers="a89590c95e43697eefb7530d98c73c90847ed833@95.217.128.197:39656"
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
sudo systemctl enable 
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
