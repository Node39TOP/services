# 🚅 Sync

**Snapshort:** 03:30 UTC | Daily | **db:** pebbledb | **pruning**: 100/0/50 | **indexer**: null

```
sudo systemctl stop zetacored

mv $HOME/.zetacored/data/priv_validator_state.json $HOME/.zetacored/priv_validator_state.json.backup 

zetacored tendermint unsafe-reset-all --home $HOME/.zetacored --keep-addr-book 
SNAP_NAME=$(curl -s https://file.node39.top/Mainnet/Zeta/ | egrep -o 'snapshot-zeta-[0-9]+\.tar\.lz4' | sort -V | tail -n 1)
wget -c https://file.node39.top/Mainnet/Zeta/${SNAP_NAME} -O - | lz4 -dc - | tar -xf - -C $HOME/.zetacored

cp $HOME/.zetacored/priv_validator_state.json.backup $HOME/.zetacored/data/priv_validator_state.json 

sudo systemctl restart zetacored
sudo journalctl -u zetacored -f --no-hostname -o cat
```

**State sync:**

```
Soon.
```
