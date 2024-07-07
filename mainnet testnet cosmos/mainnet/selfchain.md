# 🟢 SelfChain

**Hardware requirements:**&#x20;

* 8 cores
* 16GB RAM
* 500 GB SSD NVMe
* Ubuntu 22 - x86 or arm

\
Website: [https://selfchain.xyz/](https://selfchain.xyz/)\
Telegram: [https://t.me/selfchainxyz](https://t.me/selfchainxyz)\
Discord: [https://discord.com/invite/selfchainxyz](https://discord.com/invite/selfchainxyz)\
X: [https://twitter.com/selfchainxyz](https://twitter.com/selfchainxyz)

Node39 support:

* [x] RPC: [http://selfchain-rpc.node39.top](http://selfchain-rpc.node39.top)
* [x] API: [http://selfchain-api.node39.top](http://selfchain-api.node39.top)
* [x] gRPC: [http://selfchain-grpc.node39.top](http://selfchain-grpc.node39.top)
* [x] Snapshort: waiting...
* [x] Explorer: [https://explorer.node39.top/Selfchain/staking](https://explorer.node39.top/Selfchain/staking)
* [x] Staking: waiting...

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

**Install Selfchain:**&#x20;

```
//amd-64
cd $HOME && wget https://node39.top/Mainnet/selfchaind-linux-amd64 && mv selfchaind-linux-amd64 selfchaind && chmod +x selfchaind

//arm-64
cd $HOME && wget https://node39.top/Mainnet/selfchaind-linux-arm64 && mv selfchaind-linux-amd64 selfchaind && chmod +x selfchaind
```

**Set chain and Name Selfchain:**\
_<mark style="color:red;">Change</mark>_ _<mark style="color:red;">\<Change-Name></mark>_&#x20;

```
selfchaind init <Change-Name> --chain-id=self-1
```

**Download Genesis & addressbook:**

```
curl -Ls https://node39.top/node39.top/Mainnet/Selfchain/genesis.json > $HOME/.selfchain/config/genesis.json 
curl -Ls https://node39.top/node39.top/Mainnet/Selfchain/addrbook.json > $HOME/.selfchain/config/addrbook.json
```



**Create Service:**

```
sudo tee /etc/systemd/system/selfchaind.service > /dev/null <<EOF
[Unit]
Description=Selfchain node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.selfchain
ExecStart=$(which selfchain) start --home $HOME/.selfchain --chain-id self-1
Restart=always
RestartSec=5
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable selfchaind
```

**Snapshort: waiting...**

```
```

**State sync:**

```
```

**Wallet:**

```
// Create wallet
selfchaind keys add wallet

// Check Balance
selfchaind q bank balances $(galacticad keys show wallet -a)
```

**Check sync: **<mark style="color:red;">**(False -> done)**</mark>

```
selfchaind status 2>&1 | jq .SyncInfo.catching_up
```

**Create validator:**

```
selfchaind tx staking create-validator \
    --amount=1000000uslf \
    --node https://rpc.selfchain.io:26657/ \
    --pubkey=$(selfchaind tendermint show-validator) \
    --moniker="xxxxxxxxxxxxxx" \
    --website="xxxxxxxxxxxxxx" \
    --identity "xxxxxxxxxxxxxx" \
    --details="xxxxxxxxxxxxxx" \
    --security-contact "xxxxxxxxxxxxxx@" \
    --chain-id="self-1" \
    --commission-rate="0.10" \
    --commission-max-rate="0.15" \
    --commission-max-change-rate="0.05" \
    --broadcast-mode sync \
    --gas="200000" \
    --min-self-delegation="1000000" \
    --gas-adjustment="1.2" \
    --gas-prices="0.5uslf" \
    --keyring-backend file \
    --from="wallet"
```

**Edit Validator:**

```
selfchaind tx staking edit-validator \
--commission-rate 0.1 \
--new-moniker "xxx" \
--identity "xxx" \
--details "xxx" \
--from="wallet"
--chain-id="self-1" \
--gas="200000" \
--gas-prices="0.5uslf"
```

**Delegate Token to your own validator:**

```
selfchaind tx staking delegate $(selfchaind keys show wallet --bech val -a) 1000000000000000000agnet --from wallet --chain-id galactica_9302-1 --gas 200000 --gas-prices 10agnet  -y
```

**Withdraw rewards and commission from your validator:**

```
selfchaind tx distribution withdraw-rewards $(galacticad keys show wallet --bech val -a) --from wallet --commission --chain-id galactica_9302-1 --gas 200000 --gas-prices 10agnet -y
```

**Unjail validator:**

```
selfchaind tx slashing unjail --from wallet --chain-id self-1 --gas 200000 --gas-prices 0.5uslf -y
```

**Command**:

```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable selfchain

# Disable Service
sudo systemctl disable selfchaind

# Start Service
sudo systemctl start selfchaind

# Stop Service
sudo systemctl stop selfchaind

# Restart Service
sudo systemctl restart selfchaind

# Check Service Status
sudo systemctl status selfchaind

# Check Service Logs
sudo journalctl -u selfchaind -f --no-hostname -o cat
```
