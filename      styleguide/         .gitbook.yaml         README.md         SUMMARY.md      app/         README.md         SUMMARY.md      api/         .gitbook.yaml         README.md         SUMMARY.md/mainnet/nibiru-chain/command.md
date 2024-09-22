# 🛟 Command

**Command:**

Wallet:

```bash
// Add New Wallet
nibid keys add wallet

// Restore executing wallet
nibid keys add wallet --recover

//List All Wallets
nibid keys list

//Delete wallet
nibid keys delete wallet

//Check Balance
nibid q bank balances $(nibid keys show wallet -a)

//Export Key (save to wallet.backup)
nibid keys export sedad

//View EVM Prived Key
nibid keys unsafe-export-eth-key

//Import Key (restore from wallet.backup)
nibid keys import wallet.backup
```

Validator:

```bash
// Delegate
nibid tx staking delegate $(nibid keys show wallet --bech val -a) 39000000unibi 
--from wallet 
--chain-id cataclysm-1 \
--node https://nibiru-rpc.node39.top:443 \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.025unibi \
-y

// Withdraw rewards and commission
sedad tx distribution withdraw-rewards $(sedad keys show wallet --bech val -a) \
--from wallet \
--commission \
--chain-id cataclysm-1 \
--node https://nibiru-rpc.node39.top:443 \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.025unibi \
-y

// Unjail
nibid tx slashing unjail \
--from wallet \
--chain-id cataclysm-1 \
--node https://nibiru-rpc.node39.top:443 \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.025unibi \
-y


// Unbond
nibid tx staking unbond $(nibid keys show wallet --bech val -a) \
--from wallet \
--commission \
--chain-id cataclysm-1 \
--gas-adjustment 1.4 \
--node https://nibiru-rpc.node39.top:443 \
--gas auto \
--gas-prices 0.025unibi \
-y

// Edit Validator
nibid tx staking edit-validator \
--new-moniker="Node39.TOP Guide" \
--commission-rate=0.05 \
--from wallet \
--chain-id cataclysm-1 \
--gas-adjustment 1.4 \
--node https://nibiru-rpc.node39.top:443 \
--gas auto \
--gas-prices 0.025unibi \
-y
```

**Vote**:

```bash
nibid tx gov vote 1 <option> --from wallet --chain-id cataclysm-1 --fees 5000unibi -y

// Option
yes
no
abstain
NoWithVeto
```

**System:**

```bash
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable nibid

# Disable Service
sudo systemctl disable nibid

# Start Service
sudo systemctl start nibid

# Stop Service
sudo systemctl stop nibid

# Restart Service
sudo systemctl restart nibid

# Check Service Status
sudo systemctl status nibid

# Check Service Logs
sudo journalctl -u nibid -f --no-hostname -o cat
```
