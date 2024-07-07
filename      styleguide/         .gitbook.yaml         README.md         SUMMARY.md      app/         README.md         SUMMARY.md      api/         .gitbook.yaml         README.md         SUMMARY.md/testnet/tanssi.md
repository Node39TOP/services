# 🟢 Tanssi

**Recommended Hardware**: 4 Core, 8Gb ram, 200Gb SSD NVMe\
**Minumum Hardware**: 2 Core, 4Gb ram, 40Gb SSD NVMe\
**Ubuntu 22.04, x86**&#x20;

```
Website: https://www.tanssi.network/
Discord: https://discord.com/invite/kuyPhew2KB
X: https://twitter.com/TanssiNetwork
Github: https://github.com/moondance-labs/tanssi
```

Node39 support:

* [ ] RPC:&#x20;
* [ ] API:&#x20;
* [ ] Dashboard:&#x20;

{% hint style="success" %}
Update Tanssi V0.7.0\
\
`sudo systemctl stop tanssid && cd $HOME/tanssi-data && rm -rf tanssi-node && wget https://github.com/moondance-labs/tanssi/releases/download/v0.7.0/tanssi-node && chmod +x tanssi-node sudo systemctl daemon-reload` \
`sudo systemctl restart tanssid sudo journalctl -u tanssid -f -o cat`
{% endhint %}

#### Install Dependencies: <a href="#install-dependencies" id="install-dependencies"></a>

```
sudo apt update && apt upgrade -y
sudo apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev libgmp3-dev tar clang bsdmainutils ncdu unzip llvm libudev-dev make protobuf-compiler -y
```

**Download binary:**

```
sudo mkdir -p $HOME/tanssi-data
sudo chmod +x  $HOME/tanssi-data
cd $HOME/tanssi-data
wget https://github.com/moondance-labs/tanssi/releases/download/v0.6.3/tanssi-node && \
chmod +x  tanssi-node
wget https://raw.githubusercontent.com/papermoonio/external-files/main/Tanssi/Dancebox/dancebox-raw-specs.json
wget https://raw.githubusercontent.com/papermoonio/external-files/main/Moonbeam/Moonbase-Alpha/westend-alphanet-raw-specs.json
cd $HOME
relay="_relay"
```

**Create service:**

```
sudo tee /etc/systemd/system/tanssid.service > /dev/null << EOF
[Unit]
Description=Tanssi Validator Node
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=$USER
Restart=always
RestartSec=3
LimitNOFILE=65535
ExecStart=$HOME/tanssi-data/tanssi-node \
--chain=dancebox \
--name=$MONIKER \
--base-path=$HOME/tanssi-data/para \
--state-pruning=2000 \
--blocks-pruning=2000 \
--collator \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0' \
--database paritydb \
-- \
--name=tanssi-appchain \
--base-path=$HOME/tanssi-data/container \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0' \
-- \
--name=$MONIKER \
--chain=westend_moonbase_relay_testnet \
--sync=fast \
--base-path=$HOME/tanssi-data/relay \
--state-pruning=2000 \
--blocks-pruning=2000 \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0' \
--database paritydb \

[Install]
WantedBy=multi-user.target
EOF
```

Command:

```
sudo systemctl daemon-reload
sudo systemctl enable tanssid
sudo systemctl restart tanssid
sudo journalctl -u tanssid -f -o cat | grep "Orchestrator"
```
