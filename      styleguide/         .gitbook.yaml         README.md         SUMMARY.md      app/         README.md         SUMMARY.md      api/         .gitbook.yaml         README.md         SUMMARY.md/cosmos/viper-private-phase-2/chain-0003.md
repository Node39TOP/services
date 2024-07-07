# 🟢 Chain 0003

**Config chain 0003 in blockchain.json**

```
nano ~/.viper/config/blockchains.json
```

<figure><img src="../../.gitbook/assets/Ảnh màn hình 2024-06-13 lúc 11.49.06.png" alt=""><figcaption></figcaption></figure>

**Stake with chain 003**

```
viper servicers stake self <addr> <amt> 0001,0002,0003 $GEO-ID https://rpc-testnet.suiscan.xyz:443 testnet
```

**Waiting 3 block and check again:**

```
viper servicers query servicer <wallet-address>
```

<figure><img src="../../.gitbook/assets/Ảnh màn hình 2024-06-13 lúc 12.05.50.png" alt=""><figcaption></figcaption></figure>
