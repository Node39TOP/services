# 🚅 Sync

**Snapshort:**&#x20;

```
sudo systemctl stop zetacored

mv $HOME/.zetacored/data/priv_validator_state.json $HOME/.zetacored/priv_validator_state.json.backup 

zetacored tendermint unsafe-reset-all --home $HOME/.zetacored --keep-addr-book 
curl https://node39.top/Mainnet/Zeta/zeta_data.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.zetacored

cp $HOME/.zetacored/priv_validator_state.json.backup $HOME/.zetacored/data/priv_validator_state.json 

sudo systemctl restart zetacored
sudo journalctl -u zetacored -f --no-hostname -o cat
```

**State sync:**

```
Soon.
```
