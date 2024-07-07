# ⛔ Nulink

**Hardware minumum:**&#x20;

* 2 Cores
* 2GB RAM
* 40 GB SSD
* Ubuntu 22 - x86&#x20;

\
Website: [https://www.nulink.org](https://www.nulink.org)\
Discord: [https://discord.gg/25CQFUuwJS](https://discord.gg/25CQFUuwJS)\
X: [https://twitter.com/NuLink\_](https://twitter.com/NuLink\_)

**Install Dependencies:**

```
sudo apt update && apt upgrade -y
sudo apt install tmux git curl -y
```

**Run in tmux (option)**

```
tmux new -s nulink
```

**Download Binary:**

```
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.23-d901d853.tar.gz
tar -xvzf geth-linux-amd64-1.10.23-d901d853.tar.gz
```

**Create validator address: **<mark style="color:red;">**(Change pass or default pass: 123456789) Save info.**</mark>

```
cd geth-linux-amd64-1.10.23-d901d853/
./geth account new --keystore ./keystore

// Same ex:
  + Public address of the key:   0x143788ce589fc62eea12345678990qwq  # OPERATOR ADDRESS
  + Path of the secret key file: /root/geth-linux-amd64-1.10.23-d901d853/keystore/UTC--2024-01-06T19-56-64.615513747Z--123456ce129fa62aae59a5c4a495b0231e123456
```

**Install Docker & pull Nulink:**

```
apt install docker.io -y
docker pull nulink/nulink:latest
cd && mkdir nulink
cp /root/geth-linux-amd64-1.10.23-d901d853/keystore/* /root/nulink
chmod -R 777 /root/nulink
curl https://sh.rustup.rs -sSf | sh
source "$HOME/.cargo/env"

```

**Install python:**

```
apt install python3-pip -y
apt install python3-virtualenv -y
virtualenv /root/nulink-venv
source /root/nulink-venv/bin/activate
wget https://download.nulink.org/release/core/nulink-0.5.0-py3-none-any.whl
pip install nulink-0.5.0-py3-none-any.whl
source /root/nulink-venv/bin/activate
python -c "import nulink"
```

**Export  Pass: **<mark style="color:red;">**(Default 123456789)**</mark>

```
export NULINK_KEYSTORE_PASSWORD=123456789
export NULINK_OPERATOR_ETH_PASSWORD=123456789
```

**Run node: (Change info)**

_UTC and operator address_&#x20;

```
sudo docker run -it --rm \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/UTC--2024-01-06T19-56-64.615513747Z--123456ce129fa62aae59a5c4a495b0231e123456 \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--payment-network bsc_testnet \
--operator-address 0x143788ce589fc62eea12345678990qwq \
--max-gas-price 10000000000
```

**Faucet BNB:**

```
https://www.bnbchain.org/en/testnet-faucet
```

**Faucet NLK:**

```
https://dashboard.testnet.nulink.org/staking
```

Stake 10 NLK in dashboard

Bond Opera address

Send 0,1 bnb to opera address.

```
docker run --restart on-failure -d \
--name ursula \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
-e NULINK_OPERATOR_ETH_PASSWORD \
nulink/nulink nulink ursula run --no-block-until-ready
```
