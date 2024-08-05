# 🛟 Command

**Command:**

Wallet:

```bash
// Add New Wallet
emped keys add wallet

// Restore executing wallet
emped keys add wallet --recover

//List All Wallets
emped keys list

//Delete wallet
emped keys delete wallet

//Check Balance
emped q bank balances $(emped keys show wallet -a)

//Export Key (save to wallet.backup)
emped keys export wallet

//View EVM Prived Key
emped keys unsafe-export-eth-key wallet

//Import Key (restore from wallet.backup)
emped keys import wallet wallet.backup
```

Validator:

```bash
// Withdraw Rewards
emped tx distribution withdraw-all-rewards --from wallet --chain-id empe-testnet-2 --fees 500uempe -y

// Withdraw Rewards with Comission
emped tx distribution withdraw-rewards $(emped keys show wallet --bech val -a) --commission --from wallet --chain-id empe-testnet-2 --fees 500uempe -y

// Delegate Token to your own validator
emped tx staking delegate $(emped keys show wallet --bech val -a) 100000uempe --from wallet --chain-id empe-testnet-2 --fees 500uempe -y

// Delegate Token to other validator
emped tx staking redelegate $(emped keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 100000uempe --from wallet --chain-id empe-testnet-2 --fees 500uempe -y

// Unbond Token from your validator
emped tx staking unbond $(emped keys show wallet --bech val -a) 100000uempe --from wallet --chain-id empe-testnet-2 --fees 500uempe -y

// Send Token to another wallet
emped tx bank send wallet <TO_WALLET_ADDRESS> 100000uempe --from wallet --chain-id empe-testnet-2 --fees 500uempe -y

// Unjail
emped tx slashing unjail --from wallet --chain-id empe-testnet-2 --fees 500uempe -y
```

Vote:

```bash
emped tx gov vote 1 yes --from wallet --chain-id empe-testnet-2  --fees 500uempe
```

System:

```bash
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable emped

# Disable Service
sudo systemctl disable emped

# Start Service
sudo systemctl start emped

# Stop Service
sudo systemctl stop emped

# Restart Service
sudo systemctl restart emped

# Check Service Status
sudo systemctl status emped

# Check Service Logs
sudo journalctl -u emped -f --no-hostname -o cat
```
