# 🛟 Command

Wallet:

```bash
// Add New Wallet
wardend keys add wallet

// Restore executing wallet
wardend keys add wallet --recover

// List All Wallets
wardend keys list

// Delete wallet
wardend keys delete wallet

// Check Balance
wardend q bank balances $(wardend keys show wallet -a)

// Show validator
wardend tendermint show-validator

// Show EVM address
wardend keys unsafe-export-eth-key 

// Backup
Seed + priv_validator_key.json
```

**Validator:**

```bash
// Create Validator

wardend comet show-validator

# EX: pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"A7JSLhhRRXl3/Cfyi1kDQXBsp8yA5Z2BOIPQQ6JzXy7K"}' type: local Change info
{    
    "pubkey": {"@type":"/cosmos.crypto.secp256k1.PubKey","key":"A7JSLhhRRXl3/Cfyi1kDQXBsp8yA5Z2BOIPQQ6JzXy7K"},
    "amount": "1000000uward",
    "moniker": "Validator name",
    "identity": "123123123-KEYBASE",
    "website": "Link Website",
    "security": "Contact(Email;X;Telegram)",
    "details": "details validator",
    "commission-rate": "0.1",
    "commission-max-rate": "0.2",
    "commission-max-change-rate": "0.01",
    "min-self-delegation": "1"
}

#and

wardend tx staking create-validator $HOME/validator.json \
--from=wallet \
--chain-id=chiado_10010-1 \
--fees=500uward

// Delegate you Validator (Change token 1ward = 1000000uward)

wardend tx staking delegate $(wardend keys show wallet --bech val -a)  1000000uward \
    --from=wallet \
    --chain-id=chiado_10010-1 \
    --fees=500uward
// Withdraw rewards and commission from your validator

wardend tx distribution withdraw-rewards $(wardend keys show wallet --bech val -a) \
--from wallet \
--commission \
--chain-id=chiado_10010-1 \
--fees=500uward

//Unjail

wardend tx slashing unjail --from wallet --chain-id chiado_10010-1 --fees=500uward -y

```

**Command:**

```bash
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable wardend

# Disable Service
sudo systemctl disable wardend

# Start Service
sudo systemctl start wardend

# Stop Service
sudo systemctl stop wardend

# Restart Service
sudo systemctl restart wardend

# Check Service Status
sudo systemctl status wardend

# Check Service Logs
sudo journalctl -u wardend -f --no-hostname -o cat
```
