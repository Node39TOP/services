---
description: Next-gen Modular L1 Blockchain Infrastructure for Omnichain Applications.
---

# 🟢 Warden Protocol

<figure><img src="../.gitbook/assets/1500x500.jpeg" alt=""><figcaption></figcaption></figure>

**Hardware minimum:**&#x20;

* 4 core
* 8 GB RAM
* 80 GB SSD NVMe
* Ubuntu 22 - x86 or arm

\
Website: [https://wardenprotocol.org](https://wardenprotocol.org)\
Telegram: [https://t.me/wardenprotocol](https://t.me/wardenprotocol)\
Discord: [https://discord.gg/wardenprotocol](https://discord.gg/wardenprotocol)\
X: [https://x.com/wardenprotocol](https://x.com/wardenprotocol)\
\
Node39 support:

* [x] RPC: [https://warden-testnet-rpc.node39.top](https://warden-testnet-rpc.node39.top)
* [x] API: [https://warden-testnet-rpc.node39.top](https://warden-testnet-rpc.node39.top)
* [x] Snapshort: Every 4 hours
* [x] Dashboard: [https://dashboard.node39.top/warden-testnet/](https://dashboard.node39.top/warden-testnet/)

#### Install Dependencies <a href="#install-dependencies" id="install-dependencies"></a>

```
sudo apt update && sudo apt install curl -y
sudo apt update && sudo apt upgrade -y
sudo apt install curl tar wget clang pkg-config protobuf-compiler libssl-dev jq build-essential protobuf-compiler bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```

**Install GO: (amd64 - x86)**

```
VER="1.21.3"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile && mkdir -p ~/go/bin
```

**Install GO: (arm64)**

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

Node Install

```
cd $HOME && rm -rf wardenprotocol && git clone --depth 1 --branch v0.2.0 https://github.com/warden-protocol/wardenprotocol/ && cd wardenprotocol/cmd/wardend && go build && mv wardend $HOME/go/bin
```

```
MONIKER="<Name>"
wardend init <Name> --chain-id alfama
```

```
curl -Ls https://node39.top/testnet/warden/genesis.json > $HOME/.warden/config/genesis.json
curl -Ls https://node39.top/testnet/warden/addrbook.json > $HOME/.warden/config/addrbook.json
```

```
SEEDS="ff0885377c44d58164f29d356b9d3d3a755c6213@warden-testnet-seed.itrocket.net:18656"
PEERS="f995c84635c099329bfaaa255389d63e052cb0ac@warden-testnet-peer.itrocket.net:18656,55c5b6ab09002dfba354ff924775ea9ba319f226@81.31.197.120:26656,9eb15351ad2d1fd9cd866e4f9e4153f6dddcf151@51.178.92.69:16656,4b5777664aacfeeb76a51dea8d1264c2983e6aed@65.109.104.111:56103,225054d5ddf2386762450e21075c0e8677c3d0fc@144.76.29.90:26656,00c0b45d650def885fcbcc0f86ca515eceede537@152.53.18.245:15656,f362d57aa6f78e035c8924e7144b7225392b921d@213.239.217.52:38656,ad6db1f33c559707509a777c26a5db86b5dddd0c@37.27.97.16:26656,27994efdba4df95118dc2748f0ebbccf72d8bd0a@65.108.232.156:29656,7e9adbd0a34fcab219c3a818a022248c575f622b@65.108.227.207:16656,2992ea96175253603828620b3bab8688ef5d7517@65.109.92.148:61556,a5c872823b6a010e10f7afba24b1904da5ecfb20@45.76.150.121:26656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.warden/config/config.toml
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.warden/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.warden/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"50\"/" $HOME/.warden/config/app.toml
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.0025uward"|g' $HOME/.warden/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.warden/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.warden/config/config.toml
```

Custom Port

```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:24158\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:24157\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:24160\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:24156\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":24166\"%" $HOME/.warden/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:24117\"%; s%^address = \":8080\"%address = \":24180\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:24190\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:24191\"%; s%:8545%:24145%; s%:8546%:24146%; s%:6065%:24165%" $HOME/.warden/config/app.toml
```

Systemd

```
sudo tee /etc/systemd/system/warden.service > /dev/null <<EOF
[Unit]
Description=Warden node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.warden
ExecStart=$(which wardend) start --home $HOME/.warden
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable warden
```

Snapshot

```
cp $HOME/.warden/data/priv_validator_state.json $HOME/.warden/priv_validator_state.json.backup
wardend tendermint unsafe-reset-all --home $HOME/.warden
rm -rf $HOME/.warden/data $HOME/.warden/wasmPath
curl https://node39.top/testnet/warden/snap_warden.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.warden
mv $HOME/.warden/priv_validator_state.json.backup $HOME/.warden/data/priv_validator_state.json
```

Wallet

Creat new wallet&#x20;

```
wardend keys add wallet
```

Recover keys

```
wardend keys add wallet --recover
```

List All Keys

```
wardend keys list
```

Check Balance

```
wardend q bank balances $(wardend keys show wallet -a)
```

Check sync: False = synced

```
wardend status 2>&1 | jq .sync_info
```

Create Validator

```
wardend comet show-validator
```

EX: pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"A7JSLhhRRXl3/Cfyi1kDQXBsp8yA5Z2BOIPQQ6JzXy7K"}' type: local\
\
Change info\


```
{    
    "pubkey": {"@type":"/cosmos.crypto.secp256k1.PubKey","key":"A7JSLhhRRXl3/Cfyi1kDQXBsp8yA5Z2BOIPQQ6JzXy7K"},
    "amount": "1000000uward",
    "moniker": "Validator name",
    "identity": "123123123-KEYBASE",
    "website": "Link Website",
    "security": "Contact(Email;X;Telegram)",
    "details": "details validator",
    "commission-rate": "0.1",
    "commission-max-rate": "0.2",
    "commission-max-change-rate": "0.01",
    "min-self-delegation": "1"
}
```

Finally, we're ready to submit the transaction to create the validator:

```
wardend tx staking create-validator $HOME/validator.json \
--from=wallet \
--chain-id=alfama \
--fees=500uward
```

#### Delegate you Validator (Change token 1ward = 1000000uward) <a href="#id-2.5-delegate-token-to-your-own-validator" id="id-2.5-delegate-token-to-your-own-validator"></a>

```
wardend tx staking delegate $(wardend keys show wallet --bech val -a)  1000000uward \
    --from=wallet \
    --chain-id=alfama \
    --fees=500uward
```

Withdraw rewards and commission from your validator

```
wardend tx distribution withdraw-rewards $(wardend keys show wallet --bech val -a) \
--from wallet \
--commission \
--chain-id=alfama \
--fees=500uward
```

#### Unjail <a href="#id-2.7-unjail-validator" id="id-2.7-unjail-validator"></a>

```
wardend tx slashing unjail --from wallet --chain-id alfama --fees=500uward -y
```

#### Services Management <a href="#id-2.8-services-management" id="id-2.8-services-management"></a>

```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable wardend

# Disable Service
sudo systemctl disable wardend

# Start Service
sudo systemctl start wardend

# Stop Service
sudo systemctl stop wardend

# Restart Service
sudo systemctl restart wardend

# Check Service Status
sudo systemctl status wardend

# Check Service Logs
sudo journalctl -u wardend -f --no-hostname -o cat
```
