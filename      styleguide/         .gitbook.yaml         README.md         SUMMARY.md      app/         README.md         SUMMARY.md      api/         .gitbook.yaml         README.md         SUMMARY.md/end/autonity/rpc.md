# 🟢 RPC

#### Install Dependencies: <a href="#install-dependencies" id="install-dependencies"></a>

```
sudo apt update && apt upgrade -y
sudo apt install git curl wget -y
sudo apt install make clang pkg-config libssl-dev jq build-essential -y
```

**Install GO:**&#x20;

```
rm -rf $HOME/go
sudo rm -rf /usr/local/go
cd $HOME
curl https://dl.google.com/go/go1.22.4.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf -
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF
source $HOME/.profile
go version
```

**PIPX Install:**

```
sudo apt install pipx -y
pipx install --force git+https://github.com/autonity/aut.git
pipx ensurepath
```

**Download Binary:**

```
mkdir autonity-client && cd autonity-client 
wget https://github.com/autonity/autonity/releases/download/v0.14.0/autonity-linux-amd64-0.14.0.tar.gz
tar -xzf autonity-linux-amd64-0.14.0.tar.gz
rm -rf autonity-linux-amd64-0.14.0.tar.gz
sudo cp -r autonity /usr/local/bin/autonity
autonity version

mkdir autonity-chaindata

// set 
cd  $HOME
your_ip="$(curl ifconfig.me)"
```

**Create Service:**

```
sudo tee /etc/systemd/system/autonityd.service > /dev/null << EOF
[Unit]
Description=Autonityd Node
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=$USER
Restart=always
RestartSec=3
LimitNOFILE=65535
ExecStart=autonity \
    --datadir ${HOME}/autonity-client/autonity-chaindata  \
    --piccadilly  \
    --http  \
    --http.addr 0.0.0.0 \
    --http.api aut,eth,net,txpool,web3,admin  \
    --http.vhosts \* \
    --ws  \
    --ws.addr 0.0.0.0 \
    --ws.api aut,eth,net,txpool,web3,admin  \
    --nat extip:$your_ip \
[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable autonityd
```

\=> <mark style="color:red;">Reboot</mark>&#x20;

```
pipx ensurepath
cd $HOME
mkdir piccadilly-keystore && chmod +x piccadilly-keystore && cd $HOME
sleep 1
cp -r $HOME/autonity-client/autonity-chaindata/autonity/autonitykeys $HOME/piccadilly-keystore/
sleep 1
head -c 64 $HOME/autonity-client/autonity-chaindata/autonity/autonitykeys > $HOME/piccadilly-keystore/autonitykeys_private.key
sleep 1
aut account import-private-key $HOME/piccadilly-keystore/autonitykeys_private.key > $HOME/piccadilly-keystore/autonity_keystore.txt
sleep 1
keystore="$(cat  $HOME/piccadilly-keystore/autonity_keystore.txt | grep "keystore" |   awk 'NR==1{print $2}')"
sleep 1
sudo tee .autrc > /dev/null << EOF
[aut]
rpc_endpoint=http://127.0.0.1:8545
keyfile=$keystore
EOF
echo "OK"
```

**Get info and form**

**Form:** [**https://game.autonity.org/awards/register-node.html**](https://game.autonity.org/awards/register-node.html)

```
// Get sign-message public rpc
aut account sign-message "public rpc" > $HOME/piccadilly-keystore/logs_rpc.txt && cat $HOME/piccadilly-keystore/logs_rpc.txt

// Get enode
echo "$(aut node info | jq -r '.admin_enode')"
```

**Command:**&#x20;

```
sudo systemctl daemon-reload
sudo systemctl enable autonityd
sudo systemctl restart autonityd
sudo journalctl -u autonityd -f -o cat
```
