# 🟢 Kenshi - Unchained

**Hardware minumum:**&#x20;

* 2 Cores
* 2GB RAM
* 40 GB SSD
* Ubuntu 22 - x86&#x20;

Explorer: [https://kenshi.io/unchained/explore](https://kenshi.io/unchained/explore)\
Website: [https://kenshi.io/](https://kenshi.io/)\
Discord: [https://discord.gg/KenshiTech](https://discord.gg/KenshiTech)\
X: [https://t.me/KenshiTech](https://t.me/KenshiTech)\
Telegram: [https://t.me/KenshiTech](https://t.me/KenshiTech)\
Github: [https://github.com/TimeleapLabs](https://github.com/TimeleapLabs)\


**Install Dependencies:**

```
sudo apt update && apt upgrade -y
sudo apt install tmux git curl unzip -y
```

**Install docker:**

```
sudo apt install curl -y && source <(curl -s https://node39.top/command/docker_install)
```

**Download docker file:**

```
cd $home && wget https://github.com/KenshiTech/unchained/releases/download/v0.12.0/unchained-v0.12.0-docker.zip
unzip unchained-v0.12.0-docker.zip
cd unchained-v0.12.0-docker.zip
```

**Upload config:**&#x20;

```
sudo mkdir conf
sudo nano conf/conf.worker.yaml
```

* **Copy and paste: **<mark style="color:red;">**\<Change-name>**</mark>

```
                                         conf.worker.yaml                                                      system:
  log: info
  name: <Change-name> 

network:
  bind: 127.0.0.1:9123
  broker-uri: ws://127.0.0.1:9123

rpc:
  - name: ethereum
    nodes:
      - https://ethereum.publicnode.com
      - https://eth.llamarpc.com
      - wss://ethereum.publicnode.com
      - https://eth.rpc.blxrbdn.com

  - name: arbitrum
    nodes:
      - https://arbitrum-one.publicnode.com
      - https://arbitrum.llamarpc.com
      - wss://arbitrum-one.publicnode.com
      - https://arbitrum-one.public.blastapi.io

  - name: arbitrumSepolia
    nodes:
      - https://sepolia-rollup.arbitrum.io/rpc

plugins:
  uniswap:
    schedule:
      ethereum: 5s

    tokens:
      - name: ethereum
        chain: ethereum
        pair: "0x88e6a0c2ddd26feeb64f039a2c41296fcb3f5640"
        delta: 12
        invert: true
        unit: USDT
        send: true

      - name: arbitrum
        chain: ethereum
        pair: "0x59354356Ec5d56306791873f567d61EBf11dfbD5"
        delta: 0
        invert: false
        unit: ETH
        send: true

      - name: bitcoin
        chain: ethereum
        pair: "0x9db9e0e53058c89e5b94e29621a205198648425b"
        delta: 2
        invert: false
        unit: USDT
        send: true
```

**Update secret file:**

```
sudo nano conf/secrets.worker.yaml
```

* **Change your info:**

```
address: xxxxxx #This is the wallet address of unchained. There is no blank or restore yet
evmAddress: xxxxxx #This is the Metamask wallet address. You should create a completely new wallet
secretKey: xxxxxx #This is the secret of unchained. There is no blank or restore yet
publicKey: xxxxxx #This is the publickey of unchained. There is no blank or restore yet
evmPrivateKey: xxxxxx # #This is the Metamask privatekey.
```

**Command:**

```
// Start or Stop
./unchained.sh worker down
./unchained.sh worker up -d

// Check logs
./unchained.sh worker logs -f -n 20

// Update unchained
./unchained.sh worker pull
./unchained.sh worker up -d
```
