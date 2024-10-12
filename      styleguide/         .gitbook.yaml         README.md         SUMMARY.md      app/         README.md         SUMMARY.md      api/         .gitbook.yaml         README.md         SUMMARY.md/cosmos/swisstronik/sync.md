# 🚅 Sync

**State sync:**

<pre class="language-bash"><code class="lang-bash">sudo systemctl stop swisstronikd

swisstronikd tendermint unsafe-reset-all --home $HOME/.swisstronik --keep-addr-book 
mv $HOME/.swisstronik/data/priv_validator_state.json $HOME/.swisstronik/priv_validator_state.json.backup 

SNAP_RPC="https://swisstronik-testnet-rpc.node39.top/:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).$|\1"$SNAP_RPC,$SNAP_RPC"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).$|\1"$TRUST_HASH"|" $HOME/.swisstronik/config/config.toml
mv $HOME/.swisstronik/priv_validator_state.json.backup $HOME/.swisstronik/data/priv_validator_state.json

<strong>sudo systemctl restart swisstronikd &#x26;&#x26; journalctl -u swisstronikd -f -o cat
</strong></code></pre>
