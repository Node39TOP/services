# ⛔ Dusk

Website: [https://dusk.network](https://dusk.network)\
Telegram: [https://t.me/DuskNetwork](https://t.me/DuskNetwork)\
Discord: [https://discord.com/invite/dusknetwork](https://discord.com/invite/dusknetwork)\
X: [https://twitter.com/duskfoundation](https://twitter.com/duskfoundation)\
Docs: [https://docs.dusk.network/nocturne/node-running-guide/](https://docs.dusk.network/nocturne/node-running-guide/)

{% hint style="success" %}
Update Version 2.0

```
curl --proto '=https' --tlsv1.2 -sSfL https://github.com/dusk-network/node-installer/releases/download/v0.2.0/node-installer.sh | sudo sh && service rusk start
ruskquery block-height
tail -F /var/log/rusk.log
```
{% endhint %}

#### Install Dependencies: <a href="#install-dependencies" id="install-dependencies"></a>

```
sudo apt update && sudo apt upgrade -y
sudo apt install tmux htop
```

**Create wallet:**

```
https://wallet.dusk.network/setup/
```

**Faucet:**

```
https://docs.dusk.network/nocturne/testnet-faucet
```

**Restore wallet:**

```
rusk-wallet restore
```

**Run the following command to export a consensus key for the given wallet:**

```
rusk-wallet export -d /opt/dusk/conf -n consensus.keys
```

```
sh /opt/dusk/bin/setup_consensus_pwd.sh
```

**Start rusk:**

```
service rusk start
```

**Check sync:**

```
ruskquery block-height
```

**Stake Dusk:**

```
rusk-wallet stake --amt 1000
```

**Check stake:**

```
rusk-wallet stake-info
```

**Check logs:**

```
grep "execute_state_transition" /var/log/rusk.log | tail -n 5
```

```
tail -n 30 /var/log/rusk.log
```

**Check status:**

```
service rusk status
```
