# 🛟 Command

**Command:**

Wallet:

```bash
// Add New Wallet
swisstronikd keys add wallet

// Restore executing wallet
swisstronikd keys add wallet --recover

//List All Wallets
swisstronikd keys list

//Delete wallet
swisstronikd keys delete wallet

//Check Balance
swisstronikd q bank balances $(swisstronikd keys show wallet -a)

//Export Key (save to wallet.backup)
swisstronikd keys export wallet

//View EVM Prived Key
swisstronikd keys unsafe-export-eth-key wallet

//Import Key (restore from wallet.backup)
swisstronikd keys import wallet wallet.backup
```

Validator:

```bash
// Withdraw Rewards
swisstronikd tx distribution withdraw-all-rewards --from wallet --chain-id swisstronik_1291-1 --keyring-backend file --gas auto --gas-adjustment 1.5 --gas-prices 800000aswtr -y

// Withdraw Rewards with Comission
swisstronikd tx distribution withdraw-rewards $(swisstronikd keys show wallet --bech val -a) --from wallet --commission --chain-id swisstronik_1291-1 --keyring-backend file --gas auto --gas-adjustment 1.5 --gas-prices 800000aswtr -y

// Delegate Token to your own validator
swisstronikd tx staking delegate $(swisstronikd keys show wallet --bech val -a) 1000000000000000000aswtr --from wallet --chain-id swisstronik_1291-1 --keyring-backend file --gas auto --gas-adjustment 1.5 --gas-prices 800000aswtr  -y

// Delegate Token to other validator
swisstronikd tx staking redelegate $(swisstronikd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000000000000000aswtr --from wallet --chain-id swisstronik_1291-1 --keyring-backend file --gas auto --gas-adjustment 1.5 --gas-prices 800000aswtr  -y

// Unbond Token from your validator
swisstronikd tx staking unbond $(swisstronikd keys show $WALLET --bech val -a) 1000000000000000000aswtr --from wallet --chain-id swisstronik_1291-1 --keyring-backend file --gas-adjustment 1.4 --gas auto --gas-prices 7aswtr -y 

// Send Token to another wallet
swisstronikd tx bank send wallet <TO_WALLET_ADDRESS> 1000000000000000000aswtr --from wallet -chain-id swisstronik_1291-1 --keyring-backend file --gas auto --gas-adjustment 1.5 --gas-prices 800000aswtr  -y

// Unjail
swisstronikd tx slashing unjail --from wallet -chain-id swisstronik_1291-1 --keyring-backend file --gas auto --gas-adjustment 1.5 --gas-prices 800000aswtr  -y
```

Vote:

```bash
swisstronikd tx gov vote 1 yes --from wallet --chain-id swisstronik_1291-1  --keyring-backend file --gas auto --gas-adjustment 1.5 --gas-prices 800000aswtr  -y
```

System:

```bash
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable swisstronikd

# Disable Service
sudo systemctl disable swisstronikd

# Start Service
sudo systemctl start empswisstronikded

# Stop Service
sudo systemctl stop swisstronikd

# Restart Service
sudo systemctl restart swisstronikd

# Check Service Status
sudo systemctl status swisstronikd

# Check Service Logs
sudo journalctl -u swisstronikd -f --no-hostname -o cat
```
