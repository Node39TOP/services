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

**Download binary Seda: (amd-64)**

```
wget -O sedad https://github.com/sedaprotocol/seda-chain/releases/download/v0.1.5/sedad-amd64
chmod +x sedad
sudo mv sedad /usr/local/bin
sedad version
```

**Download binary Seda: (amd-64)**

```
wget -O sedad https://github.com/sedaprotocol/seda-chain/releases/download/v0.1.5/sedad-arm64
chmod +x sedad
sudo mv sedad /usr/local/bin
sedad version
```

**Set chain and Name Seda:**\
&#xNAN;_<mark style="color:red;">Change</mark>_ _<mark style="color:red;">\<Change-Name></mark>_&#x20;

```
sedad config chain-id seda-1
sedad config keyring-backend file
sedad init <Change-Name> --chain-id seda-1
```

**Custom Port: (Option)**

```
sedad config node tcp://localhost:29657
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:29658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:29657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:29660\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:29656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":29666\"%" $HOME/.sedad/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:2917\"%; s%^address = \":8080\"%address = \":29680\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:29690\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:29691\"%; s%:8545%:2945%; s%:8546%:2946%; s%:6065%:2965%" $HOME/.sedad/config/app.toml
```

**Set min gas:**&#x20;

```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "10000000000aseda"|g' $HOME/.sedad/config/app.toml
```

**Set indexing: (Option)**&#x20;

```
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.sedad/config/config.toml
```

**Download Genesis & addressbook:**&#x20;

```
wget -O $HOME/.sedad/config/genesis.json https://file.node39.top/Mainnet/Seda/genesis.json
wget -O $HOME/.sedad/config/addrbook.json https://file.node39.top/Mainnet/Seda/addrbook.json
```

**Peers:**

```
peers="c22afdfb425190b1e5ab0677a597eeb517c6c317@140.238.198.159:29656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.sedad/config/config.toml
```

**Create Service:**

```
sudo tee /etc/systemd/system/sedad.service > /dev/null <<EOF
[Unit]
Description=Seda
After=network-online.target
[Service]
User=root
ExecStart=$(which sedad) start
Restart=always
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable sedad
```

**Download snapshort:**

```
Tab Sync
```

Wallet:

```
// Add New Wallet
sedad keys add wallet

// Restore executing wallet
sedad keys add wallet --recover

// List All Wallets
sedad keys list

// Delete wallet
sedad keys delete wallet

// Check Balance
sedad q bank balances $(sedad keys show wallet -a)

// Show validator
sedad tendermint show-validator

// Show EVM address
sedad keys unsafe-export-eth-key 

// Backup
Seed + priv_validator_key.json
```

**Check sync: (**<mark style="color:red;">**False -> Done**</mark>**)**

```
sedad status 2>&1 | jq .SyncInfo.catching_up
```

**Create Validator:**

```
// Show Validator public key and copy
sedad comet show-validator
```

```
// Create file validator.json and paste (*)
nano $HOME/validator.json

{
 "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"lR1d7YBVK5jYijOfWVKRFoWCsS4dg3kagT7LB9GnG8I="},
 "amount": "1000000000000000000aseda", 
 "moniker": "Node39.TOP Guide",
 "identity": "",
 "website": "",
 "security": "",
 "details": "",
 "commission-rate": "0.1",
 "commission-max-rate": "0.2",
 "commission-max-change-rate": "0.1",
 "min-self-delegation": "1" 
}

// Create Validator
sedad tx staking create-validator validator.json --from wallet --chain-id seda-1 \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 10000000000aseda
```

