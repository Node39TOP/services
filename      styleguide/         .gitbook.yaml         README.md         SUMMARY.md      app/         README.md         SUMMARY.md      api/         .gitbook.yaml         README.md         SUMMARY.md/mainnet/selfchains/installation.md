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

**Install Binary Selfchain:**

```
//amd-64
cd $HOME && wget https://github.com/hotcrosscom/Self-Chain-Releases/releases/download/mainnet-v1.0.1/selfchaind-linux-amd64 && mv selfchaind-linux-amd64 selfchaind && chmod +x selfchaind && sudo mv selfchaind /usr/local/bin

//arm-64
cd $HOME && wget https://github.com/hotcrosscom/Self-Chain-Releases/releases/download/mainnet-v1.0.1/selfchaind-linux-arm64 && mv selfchaind-linux-arm64 selfchaind && chmod +x selfchaind && sudo mv selfchaind /usr/local/bin
```

**Set chain and Name Selfchain:** _Change_ _\<Change-Name>_

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
Description=selfchaind Daemon
After=network-online.target
[Service]
User=$USER
ExecStart=$(which selfchaind) start
Restart=always
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable selfchaind
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
