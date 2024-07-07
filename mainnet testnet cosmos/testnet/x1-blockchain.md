# ⛔ X1 Blockchain

**Install dependencies**

```
sudo apt-get update && apt-get upgrade -y
sudo apt-get install python3 python3-pip git nano wget jq tmux moreutils wine htop sudo vim make -y
sudo apt install python3-pip
sudo pip install passlib requests tqdm argon2_cffi web3==6.11.1
```

**Clone and build the X1 binary Version 1.1.5-rc.5**

```
sudo git clone --branch x1 https://github.com/FairCrypto/go-x1 && sudo wget https://go.dev/dl/go1.21.4.linux-amd64.tar.gz && sudo tar -C /usr/local -xzf go1.21.4.linux-amd64.tar.gz && export PATH=$PATH:/usr/local/go/bin && source ~/.profile && source ~/.bashrc
cd && chmod -R 777 go-x1 && cd go-x1 && go mod tidy && make x1 && sudo cp build/x1 /usr/local/bin
```

**Download snapshort, extract & Update**

```
cd && wget --no-check-certificate https://xenblocks.io/snapshots/current.snapshot.tar
cd && rm -rf ~/.x1/chaindata && rm -rf ~/.x1/go-x1 && tar -xvf current.snapshot.tar
cd && cd go-x1 
git stash
git pull
git checkout x1
git stash pop
go mod tidy
make x1
sudo cp build/x1 /usr/local/bin && cd
```

**Create Validator**&#x20;

```
x1 validator new
```

**Save the Validator Password**\
(Change pass MY\_STRONG\_PASSWORD)

```
echo "MY_STRONG_PASSWORD" > ~/.x1/.password
chmod 600 ~/.x1/.password
```

<mark style="color:red;">**When running x1 blockchain for the first time, please see the staking step below before proceeding to the next step**</mark>

**Run node:** (Change Validator ID & Validator Pubkey)

```
x1 --testnet --validator.id 516 --validator.pubkey 0xc00420ca56c7430545b1d0d57e0d57f7d7012baa785e3746044302230d615b36128fe4efa0dd8fb5ae02db2866e961b0947ed0b5d90e46b71d8dd1930d87bccf75b7 --validator.password ~/.x1/.password --xenblocks-endpoint ws://xenblocks.io:6668 --gcmode full --syncmode full --cache 32093
```

**More information**

Explorer: [https://pwa-explorer.x1-testnet.xen.network/staking](https://pwa-explorer.x1-testnet.xen.network/staking)



**Stake XN**&#x20;

1/ Click link: [https://explorer.x1-testnet.xen.network/address/0xFC00FACE00000000000000000000000000000000/write-contract#address-tabs](https://explorer.x1-testnet.xen.network/address/0xFC00FACE00000000000000000000000000000000/write-contract#address-tabs)

2/ Connection Wallet&#x20;

3/ Enter Your Validator Public Key

<figure><img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fih5DmZIOuFzhS2gw61us%2Fuploads%2F2IYYvqwjD2TB42pxV7Ec%2Fcreate-validator.ZzcNDk3Z.png?alt=media&#x26;token=d35b08d1-4295-47e7-afb4-c2e0f4977025" alt=""><figcaption></figcaption></figure>

**Update Version version 1.1.5-rc.5**

```
Stop your node
cd go-x1
sudo git checkout x1
sudo git pull
sudo make x1
sudo cp build/x1 /usr/local/bin
cd
```
