# 🚅 Sync

**Snapshort:**&#x20;

```bash
sudo systemctl stop symphonyd

mv $HOME/.symphonyd/data/priv_validator_state.json $HOME/.symphonyd/priv_validator_state.json.backup 

symphonyd tendermint unsafe-reset-all --home $HOME/.symphonyd --keep-addr-book 
curl https://node39.top/testnet/symphony/symphony-20240750.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.symphonyd

mv $HOME/.symphonyd/priv_validator_state.json.backup $HOME/.symphonyd/data/priv_validator_state.json 

sudo systemctl restart symphonyd
sudo journalctl -u symphonyd -f --no-hostname -o cat
```

**State sync:**

```bash
Soon.
```
