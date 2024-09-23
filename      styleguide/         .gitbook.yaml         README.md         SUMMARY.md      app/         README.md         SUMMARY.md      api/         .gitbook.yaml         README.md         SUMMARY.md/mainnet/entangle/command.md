# 🛟 Command

**Command:**

Wallet:

```
// Add New Wallet
entangled keys add wallet

// Restore executing wallet
entangled keys add wallet --recover

//List All Wallets
entangled keys list

//Delete wallet
entangled keys delete wallet

//Check Balance
entangled q bank balances $(entangled keys show wallet -a)

//Export Key (save to wallet.backup)
entangled keys export wallet

//View EVM Prived Key
entangled keys unsafe-export-eth-key wallet

//Import Key (restore from wallet.backup)
entangled keys import  wallet.backup
```

Validator:

```
// Delegate
entangled tx staking delegate $(entangled keys show wallet --bech val -a) 1000000aNGL --from wallet --chain-id entangle_33033-1 --gas-prices 10aNGL --gas-adjustment 1.5 --gas auto -y 

// Withdraw rewards and commission
entangled tx distribution withdraw-rewards $(entangled keys show wallet --bech val -a) --commission --from wallet --chain-id entangle_33033-1 --keyring-backend file --fees 10aNGL -y

// Unjail
entangled tx slashing unjail --from wallet --chain-id entangle_33033-1 --gas-adjustment 1.4 --gas auto --gas-prices 10aNGL -y

// Unbond
entangled tx staking unbond $(entangled keys show wallet --bech val -a) 1000000uNGL --from wallet --chain-id entangle_33033-1 --gas-prices 10aNGL --gas-adjustment 1.5 --gas auto -y 
```

Vote:

```
entangled tx gov vote 1 yes --from wallet --chain-id entangle_33033-1  --fees 10aNGL
```

System:

```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable entangled

# Disable Service
sudo systemctl disable entangled

# Start Service
sudo systemctl start entangled

# Stop Service
sudo systemctl stop entangled

# Restart Service
sudo systemctl restart entangled

# Check Service Status
sudo systemctl status entangled

# Check Service Logs
sudo journalctl -u entangled -f --no-hostname -o cat
```
