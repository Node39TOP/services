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

**Download Crowd Control:**

```
cd $HOME
wget -O cardchaind https://github.com/DecentralCardGame/Cardchain/releases/download/v0.16.0/cardchaind
chmod +x cardchaind
sudo mv cardchaind /usr/local/bin/
```

**Set chain and Name Crowd Control:**\
_<mark style="color:red;">Change</mark>_ _<mark style="color:red;">\<Change-Name></mark>_&#x20;

```
Cardchaind config keyring-backend os
Cardchaind config chain-id cardtestnet-11
Cardchaind init <Change-Name>  --chain-id cardtestnet-11
```

**Custom Port: (Option)**

<pre><code>Cardchaind config node tcp://localhost:39657
<strong>sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:39658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:39657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:39660\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:39656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":39666\"%" $HOME/.cardchaind/config/config.toml
</strong>sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:39617\"%; s%^address = \":8080\"%address = \":39680\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:39690\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:39691\"%; s%:8545%:14845%; s%:8546%:14846%; s%:6065%:39665%" $HOME/.cardchaind/config/app.toml
</code></pre>

**Set min gas:**&#x20;

```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.0ubpf"|g' $HOME/.cardchaind/config/app.toml
```

**Set indexing: (Option)**&#x20;

```
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.cardchaind/config/config.toml
```

**Download Genesis & addressbook:**

```
wget -O $HOME/.cardchaind/config/genesis.json https://node39.top/testnet/crowdcontrol/genesis.json
wget -O $HOME/.cardchaind/config/addrbook.json https://node39.top/testnet/crowdcontrol/addrbook.json
```

**Peers:**

```
PEERS="6284697cfb67571b4dd5b25c8b57dd1215900687@37.27.47.29:39656,86b643ba743ccc78e6e086120d43c96f85872601@202.61.225.157:20056"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.cardchaind/config/config.toml
```

**Create Service:**

```
sudo tee /etc/systemd/system/Cardchaind.service > /dev/null <<EOF
[Unit]
Description=Cardchain node
After=network-online.target

[Service]
User=$USER
WorkingDirectory=$HOME/.cardchaind
ExecStart=$(which Cardchaind) start --home $HOME/.cardchaind
Restart=on-failure
RestartSec=5
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable Cardchaind
```

**Download snapshort:**

```
Cardchaind tendermint unsafe-reset-all --home $HOME/.cardchaind
if curl -s --head curl https://node39.top/testnet/crowdcontrol/snap-crowdcontrol.tar.lz4 | head -n 1 | grep "200" > /dev/null; then
  curl https://node39.top/testnet/crowdcontrol/snap-crowdcontrol.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.cardchaind
    else
  echo no have snap
fi
```

Wallet:

```
// Add New Wallet
Cardchaind keys add wallet

// Restore executing wallet
Cardchaind keys add wallet --recover

// List All Wallets
Cardchaind keys list

// Delete wallet
Cardchaind keys delete wallet

// Check Balance
Cardchaind q bank balances $(Cardchaind keys show wallet -a)

// Show validator
Cardchaind tendermint show-validator

// Show EVM address
Cardchaind keys unsafe-export-eth-key 

// Backup
Seed + priv_validator_key.json
```

**Check sync: (**<mark style="color:red;">**False -> Done**</mark>**)**

```
Cardchaind status 2>&1 | jq .SyncInfo.catching_up
```

**Create Validator:**

```
Cardchaind tx staking create-validator \
--amount 1000000ubpf \
--from wallet \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(Cardchaind tendermint show-validator) \
--moniker "xxxxxxxxxxxx \
--identity "xxxxxxxxxxxx" \
--details "xxxxxxxxxxxx" \
--website "xxxxxxxxxxxx" \
--security-contact "xxxxxxxxxxxx" \
--details "POS Node&Validator 🚀" \
--chain-id cardtestnet-11 \
--gas auto --gas-adjustment 1.5 \
-y
```

**Edit Validator:**

```
Cardchaind tx staking edit-validator \
--new-moniker "xxxxxxxxxxxx" \
--identity "xxxxxxxxxxxx" \
--details "xxxxxxxxxxxx" \
--website "xxxxxxxxxxxx" \
--security-contact "xxxxxxxxxxxx" \
--details "POS Node&Validator 🚀" \
--from wallet \
--chain-id cardtestnet-11 \
--gas auto --gas-adjustment 1.5 \
-y
```

