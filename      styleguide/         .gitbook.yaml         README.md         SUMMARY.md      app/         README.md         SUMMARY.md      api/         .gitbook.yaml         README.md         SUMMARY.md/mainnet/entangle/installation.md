# ⚙️ Installation

**Install Dependencies:**

```
sudo apt update && sudo apt upgarade -y
sudo apt-get install git curl build-essential make jq gcc snapd chrony lz4 tmux unzip make bc -y
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

**Install GO: (arm64)**

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

**Install binary Entangle:**

```
cd $HOME
rm -rf entangle-blockchain
git clone https://github.com/Entangle-Protocol/entangle-blockchain
cd entangle-blockchain
make install
```

**Download Genesis & addressbook: Upgrade 24**

```
curl -Ls https://file.node39.top/Mainnet/Entangle/genesis.json > $HOME/.entangled/config/genesis.json
curl -Ls https://file.node39.top/Mainnet/Entangle/addrbook.json > $HOME/.entangled/config/addrbook.json
```

**Set min gas & Entangled chain:** _Change_ _\<Change-Name>_

```
entangled init <Change-Name> --chain-id=entangle_33033-1
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.0001aNGL\"|" $HOME/.entangled/config/app.toml
```

**Peers:**

```
// Peers:
peers="19688fc3842bacb3f8be2b45f7e29227a7d42284@152.53.3.213:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.entangled/config/config.toml
```

**Pruning (optional):**

```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.entangled/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.entangled/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.entangled/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.entangled/config/app.toml
```

**Indexer (optional):**

```
indexer="null" &&
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.entangled/config/config.toml
```

**Create Service:**

```
sudo tee /etc/systemd/system/entangled.service > /dev/null <<EOF
[Unit]
Description=entangled Daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$(which entangled) start --home /root/.entangled --chain-id entangle_33033-1
Restart=always
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable entangled
```



**Check sync: (**<mark style="color:red;">**False -> Done**</mark>**)**

```bash
zetacored status 2>&1 | jq .SyncInfo.catching_up
```

**Create Validator:**

```bash
entangled tx staking create-validator \
    --amount 1000000000000000000aNGL \
    --commission-max-change-rate "0.1" \
    --commission-max-rate "0.1" \
    --commission-rate "0.1" \
    --min-self-delegation "1" \
    --pubkey=$(entangled tendermint show-validator) \
    --moniker 'Node39.TOP - Guide' \
    --website "https://node39.top" \
    --identity "xxxxxxxxxxxxx" \
    --details "xxxxxxxxxxxxx" \
    --chain-id entangle_33033-1 \
    --gas-prices="10aNGL" \
    --gas="500000" \
    --gas-adjustment="1.5" \
    --from wallet \
    --keyring-backend file \
    -y
```

**Edit Validator:**

```bash
entangled tx staking edit-validator \
    --new-moniker "YOUR_MONIKER_NAME" \
    --identity "YOUR_KEYBASE_ID" \
    --details "YOUR_DETAILS" \
    --website "YOUR_WEBSITE_URL" \
    --chain-id entangle_33033-1 \
    --commission-rate 0.05 \
    --from wallet \
    --keyring-backend file \
    --fees 5000aNGL \
    -y
```
