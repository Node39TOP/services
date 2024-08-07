# ⚙️ Installation

**Server preparation:**

```
sudo apt update && sudo apt upgrade -y && sudo apt install tmux git curl -y && sudo apt install make clang pkg-config libssl-dev build-essential -y 
```

**Download Pactus v1.2.0: (amd64)**

```
cd $HOME && rm -rf node_pactus && wget https://github.com/pactus-project/pactus/releases/download/v1.2.0/pactus-cli_1.2.0_linux_amd64.tar.gz && tar -xzf pactus-cli_1.2.0_linux_amd64.tar.gz && rm -rf pactus-cli_1.2.0_linux_amd64.tar.gz && mv pactus-cli_1.2.0 node_pactus && cd node_pactus
```

**Download Pactus v1.2.0: (arm64)**

```
cd $HOME && rm -rf node_pactus && wget https://github.com/pactus-project/pactus/releases/download/v1.2.0/pactus-cli_1.2.0_linux_arm64.tar.gz && tar -xzf pactus-cli_1.2.0_linux_arm64.tar.gz && rm -rf pactus-cli_1.2.0_linux_arm64.tar.gz && mv pactus-cli_1.2.0 node_pactus && cd node_pactus
Setup:
```

```
sudo ./pactus-daemon init
```

\*Note: It is recommended to run <mark style="color:red;">**1-7**</mark> validator.\
Save all important information in this step (<mark style="color:red;">Address validator, Wallet & seed</mark>).

**If you already had a wallet before, you can use this command to restore the wallet**

```
./pactus-daemon init --restore "<your-seed>"
```

{% hint style="danger" %}
**Important: Set tx\_pool = 1000 -> 50**

```
sed -i.bak '/^\s*[tx_pool]/, /^\s*[/{s/(max_size = )100/\150/;}' $HOME/pactus/config.toml
```

**Command**

<img src="../../.gitbook/assets/Ảnh màn hình 2024-05-26 lúc 10.26.31.png" alt="" data-size="original">
{% endhint %}

**Start Pactus: Run in tmux**

```
sudo ./pactus-daemon start
```

**Wallet seed:**

```
./pactus-wallet seed
```

#### List of Addresses: <a href="#list-of-addresses" id="list-of-addresses"></a>

```
./pactus-wallet address all
```

**Check Balance:**

```
./pactus-wallet address balance <ADDRESS>
```

**Bond to the validator**

```
./pactus-wallet tx bond <Reward address> <Validator address> <AMOUNT>
```

**Send token**

```
./pactus-wallet tx transfer <reward address> <receiver address> <AMOUNT>
```
