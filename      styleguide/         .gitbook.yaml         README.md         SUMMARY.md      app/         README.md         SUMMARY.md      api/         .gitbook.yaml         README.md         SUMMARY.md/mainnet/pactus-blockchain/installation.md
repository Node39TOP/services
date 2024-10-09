# ⚙️ Installation

**Server preparation:**

```bash
sudo apt update && sudo apt upgrade -y && sudo apt install tmux git curl -y && sudo apt install make clang pkg-config libssl-dev build-essential -y 
```

**Download Pactus v1.5.0: (amd64)**

```bash
ver="1.5.0" && \
cd $HOME && rm -rf node_pactus && \
wget https://github.com/pactus-project/pactus/releases/download/v${ver}/pactus-cli_${ver}_linux_amd64.tar.gz && \
tar -xzf pactus-cli_${ver}_linux_amd64.tar.gz && \
rm -rf pactus-cli_${ver}_linux_amd64.tar.gz && \
mv pactus-cli_${ver} node_pactus && cd node_pactus
```

**Download Pactus v1.5.0: (arm64)**

<pre class="language-bash"><code class="lang-bash">ver="1.5.0" &#x26;&#x26; \
<strong>cd $HOME &#x26;&#x26; rm -rf node_pactus &#x26;&#x26; \
</strong>wget https://github.com/pactus-project/pactus/releases/download/v${ver}/pactus-cli_${ver}_linux_arm64.tar.gz &#x26;&#x26; \
tar -xzf pactus-cli_${ver}_linux_arm64.tar.gz &#x26;&#x26; \
rm -rf pactus-cli_${ver}_linux_arm64.tar.gz &#x26;&#x26; \
mv pactus-cli_${ver} node_pactus &#x26;&#x26; cd node_pactus
</code></pre>

**Setup:**&#x20;

```
./pactus-daemon init
```

\*Note: It is recommended to run <mark style="color:red;">**1-32**</mark> validator.\
Save all important information in this step (<mark style="color:red;">Address validator, Wallet & seed</mark>).

**If you already had a wallet before, you can use this command to restore the wallet**

```bash
./pactus-daemon init --restore "<your-seed>"
```

{% hint style="danger" %}
**Important: Set tx\_pool = 1000 -> 50**

```bash
sed -i.bak '/^\s*[tx_pool]/, /^\s*[/{s/(max_size = )100/\150/;}' $HOME/pactus/config.toml
```

**Command**

<img src="../../.gitbook/assets/Ảnh màn hình 2024-05-26 lúc 10.26.31.png" alt="" data-size="original">
{% endhint %}

**Start Pactus: Run in tmux**

```bash
sudo ./pactus-daemon start
```

**Wallet seed:**

```bash
./pactus-wallet seed
```

#### List of Addresses: <a href="#list-of-addresses" id="list-of-addresses"></a>

```bash
./pactus-wallet address all
```

**Check Balance:**

```bash
./pactus-wallet address balance <ADDRESS>
```

**Bond to the validator**

```bash
./pactus-wallet tx bond <Reward address> <Validator address> <AMOUNT>
```

**Send token**

```bash
./pactus-wallet tx transfer <reward address> <receiver address> <AMOUNT>
```
