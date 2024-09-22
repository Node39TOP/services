# ⚙️ Installation

**Install Dependencies:**

```bash
sudo apt update && sudo apt upgarade -y
sudo apt-get install git curl build-essential make jq gcc snapd chrony lz4 tmux unzip make bc -y
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

**Install GO: (arm64)**

```bash
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

**Install binary Nibiru:**

```bash
cd $HOME
rm -rf nibiru
git clone https://github.com/NibiruChain/nibiru.git
cd nibiru
git checkout v1.5.0
make install
```

**Set min gas & Nibiru chain:** _Change_ _\<Change-Name>_

```bash
nibid init <Change-Name> --chain-id=cataclysm-1
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.025unibi\"|" $HOME/.nibid/config/app.toml
```

**Download Genesis & addressbook:**

```bash
curl -Ls https://file.node39.top/Mainnet/Nibiru/genesis.json > $HOME/.nibid/config/genesis.json
curl -Ls https://file.node39.top/Mainnet/Nibiru/addrbook.json > $HOME/.nibid/config/addrbook.json
```

**Peers & Seeds:**

```bash
// Peers:
peers="c416d67c3dbb2d30b803611469e6d2634099292d@135.181.210.171:11036,d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@65.108.233.103:13956,627766bb1d2f9ac3db9f9f3153c91b6d795d1028@89.58.30.164:26656,6c99d3c416f171c230a9891092827251194af006@5.78.83.5:13956,74f2e690e1be83c189bf227c4c61b266267795c8@94.130.138.48:35656,6604179787139eab744b8a1159fee9b03fcc3714@51.81.49.176:19856,cc8ff21a53f996fd729a10bcfbd85cf009505367@65.108.75.107:34656,81b9c09ae1c76a3e7f36db91b98d1fbf1e31233c@185.248.24.16:13956,4895f4acc4934823a17166c4d84aea2858b8f50a@65.108.199.120:37456,8b564f4ddd3e2eb24b1f321d7dacc03080c9c824@65.21.227.177:10026,807df0af03c7de32317eda4fe4dbdcc3ad4b4ae6@208.88.251.53:44441,98cadded622d291141f8a83972fa046267df94b6@38.109.200.36:44441,f0ccacd7cd19f7c30c203ca4c9cbee62d4f8f773@35.234.108.227:26656,8d8324141897243927359345bb4b1bb78a1e1df1@65.109.56.235:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nibid/config/config.toml

// Seeds:
sed -i -e "s|^seeds *=.*|seeds = \"ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@seeds.polkachu.com:19856,400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@nibiru.rpc.kjnodes.com:13959,98cadded622d291141f8a83972fa046267df94b6@38.109.200.36:44441,f0ccacd7cd19f7c30c203ca4c9cbee62d4f8f773@35.234.108.227:26656,8d8324141897243927359345bb4b1bb78a1e1df1@65.109.56.235:26656\"|" $HOME/.nibid/config/config.toml
```

**Create Service:**

```bash
sudo tee /etc/systemd/system/nibid.service > /dev/null <<EOF
[Unit]
Description=Nibid Service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which nibid) start
Restart=always
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable nibid
```

**Check sync: (False -> Done)**

```bash
nibid status 2>&1 | jq .SyncInfo
```

**Wallet:**

```bash
// Check balance
nibid q bank balances wallet

// Add new Wallet
nibid keys add wallet

// Recover existing key
nibid keys add wallet --recover

//List All Keys
nibid keys list
```

**Create Validator:**

```bash
nibid tx staking create-validator \
--amount 1000000unibi \
--pubkey $(nibid tendermint show-validator) \
--moniker "YOUR_MONIKER_NAME" \
--identity "YOUR_KEYBASE_ID" \
--details "YOUR_DETAILS" \
--website "YOUR_WEBSITE_URL" \
--chain-id cataclysm-1 \
--commission-rate 0.1 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--fees 5000unibi \
-y
```

**Edit Validator:**

```bash
nibid tx staking edit-validator \
--new-moniker "YOUR_MONIKER_NAME" \
--identity "YOUR_KEYBASE_ID" \
--details "YOUR_DETAILS" \
--website "YOUR_WEBSITE_URL" \
--chain-id cataclysm-1 \
--commission-rate 0.05 \
--from wallet \
--fees 5000unibi \
-y
```

**Unjail validator:**

```bash
nibid tx slashing unjail --from wallet --chain-id cataclysm-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025unibi -y
```

**Withdraw commission and rewards validator:**

```bash
nibid tx distribution withdraw-rewards $(nibid keys show wallet --bech val -a) --commission --from wallet --chain-id cataclysm-1 --fees 5000unibi -y
```

**Vote:**

```bash
nibid tx gov vote 1 <option> --from wallet --chain-id cataclysm-1 --fees 5000unibi -y

// Option
yes
no
abstain
NoWithVeto
```
