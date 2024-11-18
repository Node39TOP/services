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
curl https://dl.google.com/go/go1.23.2.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf -
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF
source $HOME/.profile
go version
```

**Download Binary**

```bash
cd $HOME
wget -O kopid https://github.com/kopi-money/kopi/releases/download/v0.6.4.1/kopid-v0.6.4.1-linux-amd64
chmod +x kopid
mv kopid /usr/local/bin
kopid version
```

**Set chain:**\
_<mark style="color:red;">Change</mark>_ _<mark style="color:red;">\<Change-Name></mark>_&#x20;

```bash
kopid init <Change-Name> --chain-id kopi-test-5
kopid config set client chain-id kopi-test-5
kopid config set client keyring-backend test
```

**Set min gas:**&#x20;

```bash
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0ukopi\"|" $HOME/.kopid/config/app.toml
```

**Set indexing:**&#x20;

```bash
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.kopid/config/config.toml
```

**Set pruning:**

```
sed -i -e 's|^pruning *=.*|pruning = "custom"|' $HOME/.kopid/config/app.toml
sed -i -e 's|^pruning-keep-recent  *=.*|pruning-keep-recent = "100"|' $HOME/.kopid/config/app.toml
sed -i -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' $HOME/.kopid/config/app.toml
sed -i -e 's|^pruning-interval *=.*|pruning-interval = "50"|' $HOME/.kopid/config/app.toml

sed -i -e 's/timeout_propose = "3s"/timeout_propose = "1s"/g' $HOME/.kopid/config/config.toml
sed -i -e 's/timeout_commit = "5s"/timeout_commit = "1s"/g' $HOME/.kopid/config/config.toml

```

**Custom Ports:**

```
echo "export KOPI_PORT="39"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

```
sed -i.bak -e "s%:1317%:${KOPI_PORT}317%g;
s%:8080%:${KOPI_PORT}080%g;
s%:9090%:${KOPI_PORT}090%g;
s%:9091%:${KOPI_PORT}091%g;
s%:8545%:${KOPI_PORT}545%g;
s%:8546%:${KOPI_PORT}546%g;
s%:6065%:${KOPI_PORT}065%g" $HOME/.kopid/config/app.toml

sed -i.bak -e "s%:26658%:${KOPI_PORT}658%g;
s%:26657%:${KOPI_PORT}657%g;
s%:6060%:${KOPI_PORT}060%g;
s%:26656%:${KOPI_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${KOPI_PORT}656\"%;
s%:26660%:${KOPI_PORT}660%g" $HOME/.kopid/config/config.toml
```

**Download Genesis & addressbook:**

```bash
wget -O $HOME/.kopid/config/genesis.json https://file.node39.top/testnet/kopi/genesis.json
wget -O $HOME/.kopid/config/addrbook.json https://file.node39.top/testnet/kopi/addrbook.json
```

**Peers:**

```bash
PEERS="96b8b794306a195e22db77e2f6118cafd43a4c82@144.76.109.19:17656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.kopid/config/config.toml
```

**Create Service:**

<pre class="language-bash"><code class="lang-bash">sudo tee /etc/systemd/system/kopid.service > /dev/null &#x3C;&#x3C;EOF

[Unit]
Description=Kopi Protocol
After=network-online.target

[Service]
User=root
ExecStart=$(which kopid) start
Restart=always
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF

<strong>sudo systemctl daemon-reload
</strong>sudo systemctl enable kopid
</code></pre>

**Check sync: (**<mark style="color:red;">**False -> Done**</mark>**)**

```bash
kopid status 2>&1 | jq .SyncInfo.catching_up
```
