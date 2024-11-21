# 🛟 Command

**Check syncing:**

```bash
kopid status 2>&1 | jq .SyncInfo.catching_up
```

Wallet:

```bash
// Add New Wallet
kopid keys add wallet

// Restore executing wallet
kopid keys add wallet --recover

// List All Wallets
kopid keys list

// Delete wallet
kopid keys delete wallet

// Check Balance
kopid q bank balances $(kopid keys show wallet -a)

// Show validator
kopid tendermint show-validator

// Backup
Seed + priv_validator_key.json
```

**Validator:**

```bash
// Create Validator

Create file validator.json in $HOME/.kopid/config

kopid tendermint show-validator

# EX: pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"A7JSLhhRRXl3/Cfyi1kDQXBsp8yA5Z2BOIPQQ6JzXy7K"}' type: local Change info

{
  "pubkey": {
    "@type":"/cosmos.crypto.ed25519.PubKey",
    "key":"<Paste Key here>"
  },
  "amount": "1000000ukopi",
  "moniker": "Node39 Guide",
  "identity": "",
  "website": "https://services.node39.top",
  "security": "contact@node39.top",
  "details": "Node39.TOP Member",
  "commission-rate": "0.1",
  "commission-max-rate": "0.2",
  "commission-max-change-rate": "0.01",
  "min-self-delegation": "1"
}

#and

kopid tx staking create-validator $HOME/.kopid/config/validator.json \
--from=wallet \
--chain-id=luwak-1 \
--fees 100ukopi \
--node=https://kopi-testnet-rpc.node39.top:443 \
-y

// Delegate you Validator (Change token 1XKP = 1000000ukopi)

kopid tx staking delegate $(kopid keys show wallet --bech val -a)  1000000ukopi \
--from=wallet \
--chain-id=luwak-1 \
--fees 100ukopi \
--node=https://kopi-rpc.node39.top:443 \
-y

// Withdraw rewards and commission from your validator

kopid tx distribution withdraw-rewards $(kopid keys show wallet --bech val -a) \
--from=wallet \
--chain-id=luwak-1 \
--fees 100ukopi \
--node=https://kopi-rpc.node39.top:443 \
-y

//Unjail

kopid tx slashing unjail \
--from=wallet \
--chain-id=luwak-1 \
--fees 100ukopi \
--node=https://kopi-rpc.node39.top:443 \
-y

```

**Command:**

```bash
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable kopid

# Disable Service
sudo systemctl disable kopid

# Start Service
sudo systemctl start kopid

# Stop Service
sudo systemctl stop kopid

# Restart Service
sudo systemctl restart kopid

# Check Service Status
sudo systemctl status kopid

# Check Service Logs
sudo journalctl -u kopid -f -o cat
```
