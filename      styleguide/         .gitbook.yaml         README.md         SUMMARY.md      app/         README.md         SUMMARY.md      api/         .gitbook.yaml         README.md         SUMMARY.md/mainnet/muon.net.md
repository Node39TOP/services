# 🟢 Muon.net

**Hardware requirement: 4/8/40**\
\
<mark style="color:red;">Note: Pion is currently not available for unstake. If you stake, you have to wait for the dev to update the feature.</mark>

**Install dependencies**

```
sudo apt-get update && apt-get upgrade -y
sudo apt-get install ca-certificates curl gnupg -y
```

**Install docker**

```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

echo "Docker Installed Successfully!"
echo "Check Docker: docker ps "
sudo rm -rf docker_install
```

**Install Muon.net**

```
curl -o docker-compose.yml https://raw.githubusercontent.com/muon-protocol/muon-node-js/pion/docker-compose-pull.yml
docker compose pull
docker compose up -d
```

**Waiting for 5 minute**

```
curl http://localhost:8012/status
```

_**The result should look like this and means your node is running properly. (It is not added to the network yet.)**_

```
{"address":"0x22E25482F7B27b8765FE93F4ac0Cb79a0e8cB3ef",
"peerId":"QmVZrfc47AQQoPGjbWAmd1wQv9BtGYpFQ67JnbFmmHxAWN",
"networkingPort":"9012","node":{"addedToNetwork":false,...}}
```

**Buy PION**

```
https://thena.fi/swap?inputCurrency=0xb8067235c9b71feec069af151fdf0975dfbdfba5&outputCurrency=BNB
```

**Staking PION**

```
https://app.muon.net/pion/getting-started
```

**Verify on https://app.muon.net/pion/getting-started**



**Backup & restore**

```
docker exec -it muon-node ./node_modules/.bin/ts-node ./src/cmd/index.ts keys backup > backup.json
cat backup.json
```

**Restore**

```
docker exec -it muon-node ./node_modules/.bin/ts-node ./src/cmd/index.ts keys restore "$(cat backup.json)"
```

.env created successfully. -> done



Get OG: [https://docs.muon.net/muon-network/muon-nodes/pion/joining-pion-network/pion-nodes-discord-bot](https://docs.muon.net/muon-network/muon-nodes/pion/joining-pion-network/pion-nodes-discord-bot)
