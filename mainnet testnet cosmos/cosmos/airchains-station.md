# 🟢 Airchains Station

**Hardware minimum:**

* 3 cores
* 4 GB RAM
* 100 GB SSD
* Ubuntu 22 - x86 or arm64

<mark style="color:red;">Node39 run Station x Eigenlayer</mark>

Website: [https://points.airchains.io/](https://points.airchains.io/)

Faucet amf: [https://discord.gg/Vc8JMMCZ4Z](https://discord.gg/Vc8JMMCZ4Z)



**1- Install Dependencies:**

```
sudo apt update && sudo apt upgarade -y
sudo apt-get install git curl build-essential make jq gcc snapd chrony lz4 tmux unzip bc -
```

**2- Install GO: (amd64 - x86)**

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

**2- Install GO: (arm64)**

```
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

**3- Clone GitHub Repositories:**

```
git clone https://github.com/airchains-network/evm-station.git
git clone https://github.com/airchains-network/tracks.git
```

**4- etting Up and Running the EVM Station:**

```
cd evm-station
go mod tidy

/bin/bash ./scripts/local-setup.sh
/bin/bash ./scripts/local-start.sh 

# After successfully running the two start commands to logs

Ctrl + c

# Save information (Address, seed,...)

# Run evmosd in tmux

tmux new -s rollup
/bin/bash ./scripts/local-start.sh 

# Ctrl b + d -> exit
```

* **Get private key: (Save ìnomation)**

```
 /bin/bash ./scripts/local-keys.sh 
```



**5- Setting Up DA Keys and Running DA Client: **<mark style="color:red;">**amd64**</mark>\
<mark style="color:red;">**Change \[keyname]**</mark>

```
wget https://github.com/airchains-network/tracks/releases/download/v0.0.2/eigenlayer
chmod +x eigenlayer

./eigenlayer operator keys create --key-type ecdsa [keyname]
```

* **Result **<mark style="color:red;">**Save infomation**</mark>

```
eigenlayer operator keys create --key-type ecdsa test
? Enter password to encrypt the ecdsa private key:
ECDSA Private Key (Hex):  b3eba201405d5b5f7aaa9adf6bb734dc6c0f448ef64dd39df80ca2d92fca6d7b
Please backup the above private key hex in safe place.

Key location: /home/ubuntu/.eigenlayer/operator_keys/test.ecdsa.key.json
Public Key hex:  f87ee475109c2943038b3c006b8a004ee17bebf3357d10d8f63ef202c5c28723906533dccfda5d76c1da0a9f05cc6d32085ca1af8aaab5a28171474b1ad0aa68
Ethereum Address 0x6a8c0D554a694899041E52a91B4EC3Ff23d8aBD5
```

**5- Setting Up DA Keys and Running DA Client: **<mark style="color:red;">**arm64**</mark>\
<mark style="color:red;">**Change \[keyname]**</mark>

```
wget https://github.com/Layr-Labs/eigenlayer-cli/releases/download/v0.8.2/eigenlayer-cli_0.8.2_linux_arm64.tar.gztag/v0.8.1
tar -xvf eigenlayer-cli_0.8.1_linux_arm64.tar.gz
chmod +x eigenlayer

./eigenlayer operator keys create --key-type ecdsa [keyname]
```

* **Result: **<mark style="color:red;">**Save infomation**</mark>

```
eigenlayer operator keys create --key-type ecdsa test
? Enter password to encrypt the ecdsa private key:
ECDSA Private Key (Hex):  b3eba201405d5b5f7aaa9adf6bb734dc6c0f448ef64dd39df80ca2d92fca6d7b
Please backup the above private key hex in safe place.

Key location: /home/ubuntu/.eigenlayer/operator_keys/test.ecdsa.key.json
Public Key hex:  f87ee475109c2943038b3c006b8a004ee17bebf3357d10d8f63ef202c5c28723906533dccfda5d76c1da0a9f05cc6d32085ca1af8aaab5a28171474b1ad0aa68
Ethereum Address 0x6a8c0D554a694899041E52a91B4EC3Ff23d8aBD
```

**6- Setting Up and Running Tracks:**

```
cd $HOME
sudo rm -rf ~/.tracks
cd tracks
go mod tidy

#Change <moniker-name> & daKey (ECDSA Private Key (Hex):  b3eba201405d5b5f7aaa9adf6bb734dc6c0f448ef64dd39df80ca2d92fca6d7b)

go run cmd/main.go init --daRpc "disperser-holesky.eigenda.xyz" --daKey "daKey" --daType "eigen" --moniker "<moniker-name>" --stationRpc "http://127.0.0.1:8545" --stationAPI "http://127.0.0.1:8545" --stationType "evm"
```

**7- Create Keys for Junction:  Save infomation and faucet amf in Discord.**

```
#Change <account-name>
go run cmd/main.go keys junction --accountName <account-name> --accountPath $HOME/.tracks/junction-accounts/keys

go run cmd/main.go prover v1EVM
```

**8- Create Station on Junction:**

**Get nodeid:**

```
cat ~/.tracks/config/sequencer.toml
```

```
#Change infomation (<account-name>, 113.113.113.133 your ip vps, <node_id>)
go run cmd/main.go create-station --accountName <account-name> --accountPath $HOME/.tracks/junction-accounts/keys --jsonRPC "https://airchains-testnet-rpc.cosmonautstakes.com/" --info "EVM Track" --tracks <wallet-address> --bootstrapNode "/ip4/113.113.113.133/tcp/2300/p2p/<node_id>"
```

**9- Start tracks:**

```
tmux new -s tracks
go run cmd/main.go start
```

Add your rpc (ip:8545) Metamask and Chain ID 1234

Send 25tx -> Avtive
