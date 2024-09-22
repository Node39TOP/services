# 🛟 Command

**Command:**

Wallet:

```
// Add New Wallet
sedad keys add wallet

// Restore executing wallet
sedad keys add wallet --recover

//List All Wallets
sedad keys list

//Delete wallet
sedad keys delete wallet

//Check Balance
sedad q bank balances $(sedad keys show wallet -a)

//Export Key (save to wallet.backup)
sedad keys export sedad

//View EVM Prived Key
sedad keys unsafe-export-eth-key sedad

//Import Key (restore from wallet.backup)
sedad keys import sedad wallet.backup
```

Validator:

```
// Delegate
sedad tx staking delegate $(sedad keys show wallet --bech val -a) 1290000000000000000000aseda 
--from wallet 
--chain-id seda-1 
--gas-adjustment 1.4 
--node https://seda-rpc.node39.top:443 --fees 2000000000000000aseda

// Withdraw rewards and commission
sedad tx distribution withdraw-rewards $(sedad keys show wallet --bech val -a) \
--from wallet \
--commission \
--chain-id=seda-1 \
--gas-adjustment 1.4 \
--node https://seda-rpc.node39.top:443 --fees 2000000000000000aseda

// Unjail
sedad tx slashing unjail \
--from wallet \
--chain-id seda-1 \
--gas-adjustment 1.4 \
--node https://seda-rpc.node39.top:443 --fees 2000000000000000aseda

// Unbond
sedad tx staking unbond $(sedad keys show wallet --bech val -a) \
--from wallet \
--commission \
--chain-id=seda-1 \
--gas-adjustment 1.4 \
--node https://seda-rpc.node39.top:443 --fees 2000000000000000aseda

// Edit Validator
sedad tx staking edit-validator \
--new-moniker="Node39.TOP" \
--commission-rate=0.05 \
--from wallet \
--chain-id seda-1 \
--gas-adjustment 1.4 \
--node https://seda-rpc.node39.top:443 --fees 2000000000000000aseda
```

Vote:

```
sedad tx gov vote 1 yes --from wallet --chain-id seda-1  --fees 10000000000aseda
```

System:

```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable sedad

# Disable Service
sudo systemctl disable sedad

# Start Service
sudo systemctl start sedad

# Stop Service
sudo systemctl stop sedad

# Restart Service
sudo systemctl restart sedad

# Check Service Status
sudo systemctl status sedad

# Check Service Logs
sudo journalctl -u sedad -f --no-hostname -o cat
```
