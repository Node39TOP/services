# 🛟 Command

**Command:**

**Wallet:**

```
// Add New Wallet
shentud keys add wallet

// Restore executing wallet
shentud keys add wallet --recover

//List All Wallets
shentud keys list

//Delete wallet
shentud keys delete wallet

//Check Balance
shentud q bank balances $(shentud keys show wallet -a)

//Export Key (save to wallet.backup)
shentud keys export sedad

//Import Key (restore from wallet.backup)
shentud keys import selfchain wallet.backup
```

**Validator:**

```
// Check sync | False -> done.
shentud status 2>&1 | jq .SyncInfo.catching_up

// Delegate
shentud tx staking delegate $(shentud keys show your-wallet --bech val -a) 1000000uctk --from your-wallet --chain-id shentud --gas-adjustment 1.4 --gas auto --gas-prices 0.1uctk -y

// Withdraw rewards and commission
shentud tx distribution withdraw-rewards $(shentud keys show your-wallet --bech val -a) --commission --from your-wallet --chain-id shentud --gas-adjustment 1.4 --gas auto --gas-prices 0.1uctk -y

// Unjail
shentud tx slashing unjail --from your-wallet --chain-id shentu-2.2 --gas-adjustment 1.4 --gas auto --gas-prices 0.1uctk -y

// Unbond
shentud tx staking unbond $(shentud keys show your-wallet --bech val -a) 1000000uctk --from your-wallet --chain-id shentud --gas-adjustment 1.4 --gas auto --gas-prices 0.1uctk -y

```

**Vote:**

```
shentud tx gov vote <number-vote> <yes/no/no_with_veto/abstain> \
--from wallet \
--chain-id shentu-2.2  \
--fees 0.1uslf
```

**System:**

```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable shentud

# Disable Service
sudo systemctl disable shentud

# Start Service
sudo systemctl start shentud

# Stop Service
sudo systemctl stop shentud

# Restart Service
sudo systemctl restart shentud

# Check Service Status
sudo systemctl status shentud

# Check Service Logs
sudo journalctl -u shentud -f --no-hostname -o cat
```
