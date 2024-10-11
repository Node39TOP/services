# ⚙️ Installation

**Setup SGX:**

```bash
wget https://download.01.org/intel-sgx/sgx-linux/2.22/distro/ubuntu22.04-server/sgx_linux_x64_driver_2.11.54c9c4c.bin 
chmod +x sgx_linux_x64_driver_2.11.54c9c4c.bin
sudo ./sgx_linux_x64_driver_2.11.54c9c4c.bin
```

**Install Intel AESM service:**

```bash
echo "deb https://download.01.org/intel-sgx/sgx_repo/ubuntu $(lsb_release -cs) main" | sudo tee -a /etc/apt/sources.list.d/intel-sgx.list >/dev/null
curl -sSL "https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key" | sudo -E apt-key add -
sudo apt update
sudo apt install sgx-aesm-service libsgx-aesm-launch-plugin libsgx-aesm-epid-plugin
```

**Install all required libraries:**

```bash
echo "deb https://download.01.org/intel-sgx/sgx_repo/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/intel-sgx.list >/dev/null
curl -sSL "https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key" | sudo -E apt-key add -
sudo apt update
sudo apt install libsgx-launch libsgx-urts libsgx-epid libsgx-quote-ex sgx-aesm-service libsgx-aesm-launch-plugin libsgx-aesm-epid-plugin libsgx-quote-ex libsgx-dcap-ql libsnappy1v5
```

**Install RUST:**

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"
```

**Build & Install SGX tool:**

```bash
cargo install sgxs-tools
```

```bash
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

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

**Install GO: (amd64 - x86)**

```bash
rm -rf $HOME/go
sudo rm -rf /usr/local/go
cd $HOME
curl https://dl.google.com/go/go1.22.2.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf -
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF
source $HOME/.profile
go version
```

\
**Download binary Swisstroni:**

```bash
ver="v1.0.6"
wget https://github.com/SigmaGmbH/swisstronik-chain/releases/download/testnet-${ver}/swisstronikd.zip
unzip swisstronikd.zip
sudo cp $HOME/bin/libsgx_wrapper_${ver}.x86_64.so /usr/lib
cp $HOME/bin/${ver}_enclave.signed.so $HOME/.swisstronik-enclave
chmod +x $HOME/bin/swisstronikd
sudo mv $HOME/bin/swisstronikd $(which swisstronikd)
```

**Set chain and Name Swisstroni::**\
_<mark style="color:red;">Change</mark>_ _<mark style="color:red;">\<Change-Name></mark>_&#x20;

```bash
swisstronikd enclave request-master-key swisstronik-testnet-rpc.node39.top:443
swisstronikd init <Change-Name> --chain-id swisstronik_1291-1
```

**Custom Port: (Option)**

```bash
swisstronikd config node tcp://localhost:39657
sed -i -e "s%:1317%:3917%; s%:8080%:39580%; s%:9090%:39590%; s%:9091%:39591%; s%:8545%:3945%; s%:8546%:39546%; s%:6065%:39565%" $HOME/.swisstronikd/config/app.toml
sed -i -e "s%:26658%:39658%; s%:26657%:39657%; s%:6060%:3960%; s%:26656%:39656%; s%:26660%:39661%" $HOME/.swisstronikd/config/config.toml
Set min gas: 
```

```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "7uswtr"|g' $HOME/.swisstronik/config/app.toml
```

**Set Pruning: (Option)**

```bash
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.swisstronik/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.swisstronik/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.swisstronik/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.swisstronik/config/app.toml
```

**Set indexing: (Option)**&#x20;

```bash
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.swisstronik/config/config.toml
```

**Download Genesis & addressbook:**

```bash
wget -O $HOME/.swisstronik/config/genesis.json https://node39.top/testnet/Swisstronik/genesis.json
wget -O $HOME/.swisstronik/config/addrbook.json https://node39.top/testnet/Swisstronik/addrbook.json
```

**Peers:**

```bash
peers="1d75c288ecafc177ea0e15133c3f02117059e8cc@185.183.33.133:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.swisstronik/config/config.toml
```

**Create Service:**

```bash
sudo tee /etc/systemd/system/swisstronikd.service > /dev/null <<EOF
[Unit]
Description=Swisstronik
After=network-online.target

[Service]
User=root
ExecStart=/usr/local/bin/swisstronikd start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target

systemctl daemon-reload && sudo systemctl enable swisstronikd && systemctl restart swisstronikd
```

**Download cli v6:**

```bash
rm -rf /usr/local/bin/swisstronikcli \
wget https://github.com/SigmaGmbH/swisstronik-chain/releases/download/testnet-v1.0.6/swisstronikcli-linux-amd64.zip \
unzip swisstronikcli-linux-amd64.zip && \
mv swisstronikcli-linux-amd64 swisstronikcli && \
chmod +x swisstronikcli && ./swisstronikcli
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

**Next**

```bash
mv swisstronikcli /usr/local/bin/swisstronikcli
```

```bash
swisstronikcli config chain-id swisstronik_1291-1
swisstronikcli config keyring-backend file
```

\
**Check sync: (**<mark style="color:red;">**False -> Done**</mark>**)**

```bash
swisstronikd status 2>&1 | jq .SyncInfo.catching_up
```

**Create Validator:**

```bash
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
--keyring-backend file \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 800000aswtr \ 
-y 
```

**Edit Validator:**

```bash
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
--keyring-backend file \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 800000aswtr \
-y 
```
