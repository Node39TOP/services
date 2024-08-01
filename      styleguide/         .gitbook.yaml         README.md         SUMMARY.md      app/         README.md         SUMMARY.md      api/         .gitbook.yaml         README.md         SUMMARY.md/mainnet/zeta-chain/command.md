# 🛟 Command

**Command:**

Wallet:

```
// Add New Wallet
zetacored keys add wallet

// Restore executing wallet
zetacored keys add wallet --recover

//List All Wallets
zetacored keys list

//Delete wallet
zetacored keys delete wallet

//Check Balance
zetacored q bank balances $(zetacored keys show wallet -a)

//Export Key (save to wallet.backup)
zetacored keys export sedad

//View EVM Prived Key
zetacored keys unsafe-export-eth-key sedad

//Import Key (restore from wallet.backup)
zetacored keys import sedad wallet.backup
```

Validator:

```
// Delegate
zetacored tx staking delegate $(zetacored keys show wallet --bech val -a) 1000000azeta --from wallet --chain-id zetachain_7000-1 --gas-prices 2000000000000000azeta --gas-adjustment 1.5 --gas auto -y 

// Withdraw rewards and commission
zetacored tx distribution withdraw-rewards $(zetacored keys show wallet --bech val -a) --commission --from wallet --chain-id zetachain_7000-1 --gas-prices 2000000000000000azeta --gas-adjustment 1.5 --gas auto -y 

// Unjail
zetacored tx slashing unjail --from wallet --chain-id zetachain_7000-1 --gas-adjustment 1.5 --gas auto --gas-prices 2000000000000000azeta -y

// Unbond
zetacored tx staking unbond $(zetacored keys show wallet --bech val -a) 1000000azeta --from wallet --chain-id zetachain_7000-1 --gas-prices 2000000000000000azeta --gas-adjustment 1.5 --gas auto -y 
```

Vote:

```
zetacored tx gov vote 1 yes --from wallet --chain-id zetachain_7000-1  --fees 2000000000000000azeta
```

System:

```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable zetacored

# Disable Service
sudo systemctl disable zetacored

# Start Service
sudo systemctl start zetacored

# Stop Service
sudo systemctl stop zetacored

# Restart Service
sudo systemctl restart zetacored

# Check Service Status
sudo systemctl status zetacored

# Check Service Logs
sudo journalctl -u zetacored -f --no-hostname -o cat
```
