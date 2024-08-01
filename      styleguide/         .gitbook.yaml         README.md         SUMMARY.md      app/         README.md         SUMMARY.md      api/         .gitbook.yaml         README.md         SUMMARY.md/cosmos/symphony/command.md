# 🛟 Command

**Command:**

Wallet:

```
// Add New Wallet
symphonyd keys add wallet

// Restore executing wallet
symphonyd keys add wallet --recover

//List All Wallets
symphonyd keys list

//Delete wallet
symphonyd keys delete wallet

//Check Balance
symphonyd q bank balances $(symphonyd keys show wallet -a)

//Export Key (save to wallet.backup)
symphonyd keys export sedad

//View EVM Prived Key
symphonyd keys unsafe-export-eth-key sedad

//Import Key (restore from wallet.backup)
symphonyd keys import sedad wallet.backup
```

Validator:

```
// Delegate
symphonyd tx staking delegate $(symphonyd keys show wallet --bech val -a) 1000000note --from wallet --chain-id symphony-testnet-2 --gas-prices 1note --gas-adjustment 1.5 --gas auto -y 

// Withdraw rewards and commission
symphonyd tx distribution withdraw-rewards $(symphonyd keys show wallet --bech val -a) --commission --from wallet --chain-id symphony-testnet-2 --gas-prices 1note --gas-adjustment 1.5 --gas auto -y 

// Unjail
symphonyd tx slashing unjail --from wallet --chain-id symphony-testnet-2 --gas-adjustment 1.5 --gas auto --gas-prices 1note -y

// Unbond
symphonyd tx staking unbond $(symphonyd keys show wallet --bech val -a) 1000000note --from wallet --chain-id symphony-testnet-2 --gas-prices 1note --gas-adjustment 1.5 --gas auto -y 
```

Vote:

```
symphonyd tx gov vote 1 yes --from wallet --chain-id symphony-testnet-2  --fees 800
```

System:

```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable symphonyd

# Disable Service
sudo systemctl disable symphonyd

# Start Service
sudo systemctl start symphonyd

# Stop Service
sudo systemctl stop symphonyd

# Restart Service
sudo systemctl restart symphonyd

# Check Service Status
sudo systemctl status symphonyd

# Check Service Logs
sudo journalctl -u symphonyd -f --no-hostname -o cat
```
