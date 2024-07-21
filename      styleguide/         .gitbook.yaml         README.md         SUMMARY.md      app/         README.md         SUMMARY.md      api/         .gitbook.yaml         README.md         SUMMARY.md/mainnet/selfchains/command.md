# 🛟 Command

**Command:**

**Wallet:**

```
// Add New Wallet
selfchain keys add wallet

// Restore executing wallet
selfchain keys add wallet --recover

//List All Wallets
selfchain keys list

//Delete wallet
selfchain keys delete wallet

//Check Balance
selfchain q bank balances $(selfchain keys show wallet -a)

//Export Key (save to wallet.backup)
selfchain keys export sedad

//Import Key (restore from wallet.backup)
selfchain keys import selfchain wallet.backup
```

**Validator:**

```
// Check sync | False -> done.
selfchaind status 2>&1 | jq .SyncInfo.catching_up

// Delegate
selfchaind tx staking delegate $(selfchaind keys show wallet --bech val -a) 1000000000000000000agnet \
--from wallet \
--chain-id self-1 \
--gas 200000 \
--gas-prices 0.5uslf

// Withdraw rewards and commission
selfchaind tx distribution withdraw-rewards $(selfchaind keys show wallet --bech val -a) \
--from wallet \
--commission --chain-id self-1 \
--gas 200000 \
--gas-prices 0.5uslf

// Unjail
selfchaind tx slashing unjail \
--from wallet \
--chain-id self-1 \
--gas 200000 \
--gas-prices 0.5uslf

// Unbond
selfchaind tx staking unbond $(selfchaind keys show wallet --bech val -a) 1000000uslf \
--from wallet \
--chain-id self-1 \
--gas-prices 0.5uslf \
--gas-adjustment 1.5 \
--gas auto 
```

**Vote:**

```
selfchaind tx gov vote <number-vote> <yes/no/no_with_veto/abstain> \
--from wallet \
--chain-id self-1  \
--fees 0.5uslf
```

**System:**

```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable selfchaind

# Disable Service
sudo systemctl disable selfchaind

# Start Service
sudo systemctl start selfchaind

# Stop Service
sudo systemctl stop selfchaind

# Restart Service
sudo systemctl restart selfchaind

# Check Service Status
sudo systemctl status selfchaind

# Check Service Logs
sudo journalctl -u selfchaind -f --no-hostname -o cat
```
