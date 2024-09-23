# 🚅 Sync

**Snapshort:** 06:00 UTC | Daily | **db:** goleveldb | **pruning**: 100/0/50 | **indexer**: null

```
sudo systemctl stop entangled

mv $HOME/.entangled/data/priv_validator_state.json $HOME/.entangled/priv_validator_state.json.backup 

entangled tendermint unsafe-reset-all --home $HOME/.entangled --keep-addr-book 
SNAP_NAME=$(curl -s https://file.node39.top/Mainnet/Entangle/ | egrep -o 'snapshot-entangle-[0-9]+\.tar\.lz4' | sort -V | tail -n 1)
wget -c https://file.node39.top/Mainnet/Entangle/${SNAP_NAME} -O - | lz4 -dc - | tar -xf - -C $HOME/.entangled

cp $HOME/.entangled/priv_validator_state.json.backup $HOME/.entangled/data/priv_validator_state.json 

sudo systemctl restart entangled
sudo journalctl -u entangled -f --no-hostname -o cat
```

**State sync:**

```
Soon.
```
