# 🟢 Entangle

**Hardware requirements:**&#x20;

* 8 cores
* 32GB RAM
* 500 GB SSD NVMe
* Ubuntu 22 - x86 or arm

\
Website: [https://entangle.fi](https://entangle.fi)\
Telegram: [https://t.me/entangle\_community](https://t.me/entangle\_community)\
Discord: [https://discord.com/invite/entangle](https://discord.com/invite/entangle)\
X: [https://twitter.com/Entanglefi](https://twitter.com/Entanglefi)\
\
Node39 support:

* [x] RPC: [https://entangle-rpc.node39.top](https://entangle-rpc.node39.top)
* [x] API: [https://entangle-api.node39.top](https://entangle-api.node39.top)
* [x] Snapshort:&#x20;
* [x] Explorer: [https://explorer.node39.top/entangle/staking/entvaloper1vsc4a54c38e2h9a4d77hs559lkkx639hjxkhyf](https://explorer.node39.top/entangle/staking/entvaloper1vsc4a54c38e2h9a4d77hs559lkkx639hjxkhyf)
* [x] Staking: [https://explorer.entangle.fi/validators/entvaloper1vsc4a54c38e2h9a4d77hs559lkkx639hjxkhyf](https://explorer.entangle.fi/validators/entvaloper1vsc4a54c38e2h9a4d77hs559lkkx639hjxkhyf)

{% hint style="success" %}
**Update version 1.1.1**

```
cd $HOME
rm -rf entangle-blockchain
git clone https://github.com/Entangle-Protocol/entangle-blockchain
cd entangle-blockchain
make install
sudo systemctl restart entangled
```
{% endhint %}

#### Install Dependencies: <a href="#install-dependencies" id="install-dependencies"></a>

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

**Install binary  Entangle:**

```
cd $HOME
rm -rf entangle-blockchain
git clone https://github.com/Entangle-Protocol/entangle-blockchain
cd entangle-blockchain
make install
```

**Download Genesis & addressbook:**

```
curl -Ls https://node39.top/Mainnet/Entangle/genesis.json > $HOME/.entangled/config/genesis.json
curl -Ls https://node39.top/Mainnet/Entangle/addrbook.json > $HOME/.entangled/config/addrbook.json
```

**Set min gas & Entangled chain:**\
_<mark style="color:red;">Change</mark>_ _<mark style="color:red;">\<Change-Name></mark>_&#x20;

```
entangled init <Change-Name> --chain-id=entangle_33033-1
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.0001aNGL\"|" $HOME/.entangled/config/app.toml
```

**Peers:**

```
// Peers:
peers="19688fc3842bacb3f8be2b45f7e29227a7d42284@152.53.3.213:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nibid/config/config.toml
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

**Download snapshort:**&#x20;

```
sudo systemctl stop entangled

cp $HOME/.entangled/data/priv_validator_state.json $HOME/.nibid/priv_validator_state.json.backup 

entangled tendermint unsafe-reset-all --home $HOME/.entangled --keep-addr-book 
curl https://node39.top/Mainnet/Entangle/snapshort-entangle.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.entangled

mv $HOME/.entangled/priv_validator_state.json.backup $HOME/.entangled/data/priv_validator_state.json 

sudo systemctl restart entangled
sudo journalctl -u entangled -f --no-hostname -o cat
```

**State sync:**

```
soon
```

**Check sync:  **<mark style="color:red;">**(False -> Done)**</mark>

```
entangled status 2>&1 | jq .SyncInfo
```

**Wallet:**

```
// Check balance
entangled q bank balances <wallet-address>

// Add new Wallet
entangled keys add wallet

// Recover existing key
entangled keys add wallet --recover

//List All Keys
entangled keys list
```

**Create Validator:**

```
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
```

**Edit Validator:**

```
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
```

**Unjail validator:**

```
entangled tx slashing unjail --from wallet --chain-id entangle_33033-1 --gas-adjustment 1.4 --gas auto --gas-prices 10aNGL -y
```

**Withdraw commission and rewards validator:**

```
entangled tx distribution withdraw-rewards $(entangled keys show wallet --bech val -a) --commission --from wallet --chain-id entangle_33033-1 --keyring-backend file --fees 10aNGL -y
```

**Vote:**

```
entangled tx gov vote 1 <option> --from wallet --chain-id entangle_33033-1 --keyring-backend file --fees 10aNGL

// Option
yes
no
abstain
NoWithVeto
```
