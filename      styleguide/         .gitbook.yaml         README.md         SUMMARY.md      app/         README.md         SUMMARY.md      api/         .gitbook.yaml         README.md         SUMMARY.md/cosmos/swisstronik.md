---
description: >-
  Swisstronik is a layer 1 solution built on the Cosmos SDK framework. It aims
  to combine the benefits of the Ethereum Virtual Machine (EVM) with the
  underlying infrastructure provided by the Cosmos SDK
---

# 🟢 Swisstronik

**Hardware requirements:**&#x20;

* For now, you can use any **Intel CPU** which supports **SGX via SPS** and EPID remote attestation
* 32GB RAM
* 500 GB SSD
* Ubuntu 22

\
Website: [https://swisstronik.com/](https://swisstronik.com/)\
Telegram: [https://t.me/swisstronik](https://t.me/swisstronik)\
Discord: [https://discord.com/invite/73cKRu9BQF](https://discord.com/invite/73cKRu9BQF)\
X: [https://x.com/swisstronik](https://x.com/swisstronik)\
\
Node39 support:

* [x] RPC: [https://swisstronik-testnet-rpc.node39.top/](https://swisstronik-testnet-rpc.node39.top/)
* [x] API: [https://swisstronik-testnet-rpc.node39.top/](https://swisstronik-testnet-rpc.node39.top/)
* [x] gRPC: [https://swisstronik-testnet-grpc.node39.top](https://swisstronik-testnet-grpc.node39.top)
* [x] Json-RPC: [https://swisstronik-testnet-json-rpc.node39.top/](https://swisstronik-testnet-json-rpc.node39.top/)
* [x] Dashboard: [https://dashboard.node39.top/swisstroik-testnet](https://dashboard.node39.top/swisstroik-testnet)

#### &#x20;<a href="#install-dependencies" id="install-dependencies"></a>

{% hint style="success" %}
Update version 1.0.4



```
wget https://github.com/SigmaGmbH/swisstronik-chain/releases/download/testnet-v1.0.4/swisstronikd.zip
unzip swisstronikd.zip
sudo cp $HOME/bin/libsgx_wrapper_v1.0.4.x86_64.so /usr/lib
cp $HOME/bin/v1.0.4_enclave.signed.so $HOME/.swisstronik-enclave
chmod +x $HOME/bin/swisstronikd
sudo mv $HOME/bin/swisstronikd $(which swisstronikd)

Next: Edit systemd to v1.0.4 (Option)

sudo systemctl daemon-reload && sudo systemctl restart swisstronikd && journalctl -u swisstronikd -f -o cat
```
{% endhint %}

#### Setup SGX: <a href="#install-dependencies" id="install-dependencies"></a>

```
wget https://download.01.org/intel-sgx/sgx-linux/2.22/distro/ubuntu22.04-server/sgx_linux_x64_driver_2.11.54c9c4c.bin 
chmod +x sgx_linux_x64_driver_2.11.54c9c4c.bin
sudo ./sgx_linux_x64_driver_2.11.54c9c4c.bin
```

#### Install Intel AESM service: <a href="#install-dependencies" id="install-dependencies"></a>

```
echo "deb https://download.01.org/intel-sgx/sgx_repo/ubuntu $(lsb_release -cs) main" | sudo tee -a /etc/apt/sources.list.d/intel-sgx.list >/dev/null
curl -sSL "https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key" | sudo -E apt-key add -
sudo apt update
sudo apt install sgx-aesm-service libsgx-aesm-launch-plugin libsgx-aesm-epid-plugin

```

#### Install all required libraries: <a href="#install-dependencies" id="install-dependencies"></a>

```
echo "deb https://download.01.org/intel-sgx/sgx_repo/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/intel-sgx.list >/dev/null
curl -sSL "https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key" | sudo -E apt-key add -
sudo apt update
sudo apt install libsgx-launch libsgx-urts libsgx-epid libsgx-quote-ex sgx-aesm-service libsgx-aesm-launch-plugin libsgx-aesm-epid-plugin libsgx-quote-ex libsgx-dcap-ql libsnappy1v5
```

#### Install RUST: <a href="#install-dependencies" id="install-dependencies"></a>

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"
```

#### Build & Install SGX tool: <a href="#install-dependencies" id="install-dependencies"></a>

```
cargo install sgxs-tools
```

```
sudo $(which sgx-detect)

Detecting SGX, this may take a minute...
✔  SGX instruction set
  ✔  CPU support
  ✔  CPU configuration
  ✔  Enclave attributes
  ✔  Enclave Page Cache
  SGX features
    ✔  SGX2  ✔  EXINFO  ✔  ENCLV  ✔  OVERSUB  ✔  KSS  
    Total EPC size: 92.2MiB
✔  Flexible launch control
  ✔  CPU support
  ✔  CPU configuration
  ✔  Able to launch production mode enclave
✔  SGX system software
  ✔  SGX kernel device (/dev/sgx_enclave)
  ✔  libsgx_enclave_common
  ✔  AESM service
  ✔  Able to launch enclaves
    ✔  Debug mode
    ✔  Production mode
    ✔  Production mode (Intel whitelisted)

You're all set to start running SGX programs!
```

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

**Download Swisstronik:**

```
wget https://github.com/SigmaGmbH/swisstronik-chain/releases/download/testnet-v1.0.3/swisstronikd.zip
unzip swisstronikd.zip
sudo cp bin/libsgx_wrapper_v1.0.3.x86_64.so /usr/lib
mkdir .swisstronik-enclave
cp bin/v1.0.3_enclave.signed.so .swisstronik-enclave
rm -rf /usr/local/bin/swisstronikd
cp bin/swisstronikd /usr/local/bin/
```

**Set chain and Name Swisstronik:**\
_<mark style="color:red;">Change</mark>_ _<mark style="color:red;">\<Change-Name></mark>_&#x20;

```
swisstronikd enclave request-master-key rpc.testnet.swisstronik.com:46789
swisstronikd init YOUR_MONIKER --chain-id swisstronik_1291-1
```

If err request-master-key: [https://swisstronik.gitbook.io/swisstronik-docs/node-setup/upgrade-your-node/swisstronik-v3-testnet](https://swisstronik.gitbook.io/swisstronik-docs/node-setup/upgrade-your-node/swisstronik-v3-testnet)

**Download Genesis & addressbook:**

```
wget -O $HOME/.swisstronik/config/genesis.json https://node39.top/testnet/Swisstronik/genesis.json
wget -O $HOME/.swisstronik/config/addrbook.json https://node39.top/testnet/Swisstronik/addrbook.json
```

**Create Service:**

```
sudo tee /etc/systemd/system/swisstronikd.service > /dev/null <<EOF
[Unit]
Description=Swisstronik
After=network-online.target

[Service]
User=root
ExecStart=/usr/local/bin/swisstronikd_v1.0.3 start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
```

**State sync:**

```
sudo systemctl stop swisstronikd
SNAP_RPC="https://rpc.testnet.swisstronik.com:443"
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).$|\1"$SNAP_RPC,$SNAP_RPC"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).$|\1"$TRUST_HASH"|" $HOME/.swisstronik/config/config.toml
swisstronikd tendermint unsafe-reset-all --home $HOME/.swisstronik --keep-addr-book sudo systemctl restart swisstronikd && journalctl -u swisstronikd -f -o cat | grep heightDownload snapshort:
```

**Download snapshort:**

```
swisstronikd tendermint unsafe-reset-all --home $HOME/.swisstronik
if curl -s --head curl https://testnet-files.itrocket.net/swisstronik/snap_swisstronik.tar.lz4 | head -n 1 | grep "200" > /dev/null; then
  curl https://testnet-files.itrocket.net/swisstronik/snap_swisstronik.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.swisstronik
    else
  echo no have snap
fi
```

**Download cli v4:**

```
rm -rf /usr/local/bin/swisstronikcli
wget https://github.com/SigmaGmbH/swisstronik-chain/releases/download/testnet-v1.0.3/swisstronikcli-linux-amd64.zip && unzip swisstronikcli-linux-amd64.zip && mv swisstronikcli-linux-amd64 swisstronikcli && chmod +x swisstronikcli && ./swisstronikcli
```

The command above will output a list of accessible commands and flags:

```
Usage:
  swisstronikcli [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  config      Create or query an application CLI configuration file
  debug       Commands for debug
  help        Help about any command
  keys        Manage your application's keys
  query       Querying subcommands
  status      Query remote node for status
  tx          Transactions subcommands

Flags:
  -b, --broadcast-mode string    Transaction broadcasting mode (sync|async|block) (default "sync")
      --chain-id string          Specify Chain ID for sending Tx (default "swisstronik")
      --fees string              Fees to pay along with transaction; eg: 10uswtr
      --from string              Name or address of private key with which to sign
      --gas-adjustment float     adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string        Gas prices to determine the transaction fee (e.g. 10uswtr)
  -h, --help                     help for swisstronikcli
      --home string              directory for config and data (default "/Users/voldyrvovka/.swisstronik")
      --keyring-backend string   Select keyring's backend (default "test")
      --log_format string        The logging format (json|plain) (default "plain")
      --log_level string         The logging level (trace|debug|info|warn|error|fatal|panic) (default "info")
      --node string              <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trace                    print out full stack trace on errors

Use "swisstronikcli [command] --help" for more information about a command.
```

Next

```
mv swisstronikcli /usr/local/bin/swisstronikcli
```

```
swisstronikcli config chain-id swisstronik_1291-1
swisstronikcli config keyring-backend file
```

**Wallet:**

```
// Add New Wallet
swisstronikcli keys add wallet --keyring-backend file

// Restore executing wallet
swisstronikcli keys add wallet --keyring-backend file --recover

// List All Wallets
swisstronikd keys list

// Delete wallet
swisstronikd keys delete wallet

// Check Balance
swisstronikd q bank balances $(swisstronikd keys show wallet -a)

// Show validator
swisstronikd tendermint show-validator

// Show EVM address
swisstronikd keys unsafe-export-eth-key 

// Backup
Seed + priv_validator_key.json
```

**Check sync: (**<mark style="color:red;">**False -> Done**</mark>**)**

```
swisstronikd status 2>&1 | jq .SyncInfo.catching_up
```

**Create Validator:**

```
swisstronikd tx staking create-validator \
--amount 1000000000000uswtr \
--from wallet \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(swisstronikd tendermint show-validator) \
--moniker "xxxxxxxxxxxx \
--identity "xxxxxxxxxxxx" \
--details "xxxxxxxxxxxx" \
--website "xxxxxxxxxxxx" \
--security-contact "xxxxxxxxxxxx" \
--details "POS Node&Validator 🚀" \
--chain-id swisstronik_1291-1 \
--keyring-backend test --gas-prices 300000uswtr \
-y 
```

**Edit Validator:**

```
swisstronikd tx staking edit-validator \
--commission-rate 0.1 \
--new-moniker "xxxxxxxxxxxx" \
--identity "xxxxxxxxxxxx" \
--details "xxxxxxxxxxxx" \
--website "xxxxxxxxxxxx" \
--security-contact "xxxxxxxxxxxx" \
--details "POS Node&Validator 🚀" \
--from wallet \
--chain-id swisstronik_1291-1 \
--keyring-backend test --gas-prices 300000uswtr \
-y 
```

**Command:**

```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable swisstronikd

# Disable Service
sudo systemctl disable swisstronikd

# Start Service
sudo systemctl start swisstronikd

# Stop Service
sudo systemctl stop swisstronikd

# Restart Service
sudo systemctl restart swisstronikd

# Check Service Status
sudo systemctl status swisstronikd

# Check Service Logs
journalctl -u swisstronikd -f -o cat | grep height
```
