# ⚙️ Installation

**Install dependencies:**

```
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

**Install GO: (amd64 - x86)**

```
rm -rf $HOME/go
sudo rm -rf /usr/local/go
cd $HOME
curl https://dl.google.com/go/go1.20.6.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf -
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF
source $HOME/.profile
go version
```

**Install GO: (arm64)**

```
rm -rf $HOME/go
sudo rm -rf /usr/local/go
cd $HOME
curl https://dl.google.com/go/go1.20.6.linux-arm64.tar.gz | sudo tar -C/usr/local -zxvf -
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
**Download binary Zeta Chain:**

```
cd $HOME
rm -rf node
git clone https://github.com/zeta-chain/node.git
cd node
git checkout v18.0.0
make install
```

**Set chain and Name Zeta Chain:**\
_<mark style="color:red;">Change</mark>_ _<mark style="color:red;">\<Change-Name></mark>_&#x20;

```
zetacored init Moniker --chain-id=zetachain_7000-1
```

**Custom Port: (Option)**

```
zetacored config node tcp://localhost:39657
sed -i -e "s%:1317%:3917%; s%:8080%:39580%; s%:9090%:39590%; s%:9091%:39591%; s%:8545%:3945%; s%:8546%:39546%; s%:6065%:39565%" $HOME/.zetacored/config/app.toml
sed -i -e "s%:26658%:39658%; s%:26657%:39657%; s%:6060%:3960%; s%:26656%:39656%; s%:26660%:39661%" $HOME/.zetacored/config/config.toml
```

**Set min gas:**&#x20;

```
sed -i -e 's|^minimum-gas-prices *=.*|minimum-gas-prices = "10000000000azeta"|' $HOME/.zetacored/config/app.toml
```

**Set indexing: (Option)**&#x20;

```
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.zetacored/config/config.toml
```

**Download Genesis & addressbook:**

```
wget -O $HOME/.zetacored/config/genesis.json https://node39.top/Mainnet//genesis.json
wget -O $HOME/.zetacored/config/addrbook.json https://node39.top/Mainnet/Zeta/addrbook.json
```

**Peers:**

```
peers="74764a3fc91b5ebbdc1c5a0fcc46efff66ac0fdd@152.53.3.200:59656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.zetacored/config/config.toml
```

**Create Service:**

```
sudo tee /etc/systemd/system/zetacored.service > /dev/null <<EOF
[Unit]
Description=zetacored Daemon
After=network-online.target
[Service]
User=$USER
ExecStart=$(which zetacored) start
Restart=always
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable zetacored
```

**Check sync: (**<mark style="color:red;">**False -> Done**</mark>**)**

```
zetacored status 2>&1 | jq .SyncInfo.catching_up
```

**Create Validator:**

```
zetacored tx staking create-validator \
--amount=1000000000000000000azeta \
--pubkey=$(zetacored tendermint show-validator) \
--moniker="Node39.TOP " \
--identity=xxxx \
--details="xxxx" \
--chain-id=zetachain_7000-1 \
--commission-rate=0.1 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--gas-prices=20000000000azeta \
--gas-adjustment=1.5 \
--gas=auto \
-y
```

**Edit Validator:**

```
zetacored tx staking edit-validator \
--new-moniker="Node39.TOP " \
--identity=xxxx \
--details="xxxx" \
--chain-id=zetachain_7000-1 \
--commission-rate=0.1 \
--from=wallet \
--gas-prices=20000000000azeta \
--gas-adjustment=1.5 \
--gas=auto \
-y
```
