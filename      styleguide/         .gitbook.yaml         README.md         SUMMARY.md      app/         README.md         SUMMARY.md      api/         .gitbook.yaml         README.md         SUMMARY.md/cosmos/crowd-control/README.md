# 🟢 Crowd Control

**Hardware minimum:**&#x20;

* 2 cores
* 2 GB RAM
* 80 GB SSD NVMe
* Ubuntu 22 - x86

\
Website: [https://crowdcontrol.network/](https://crowdcontrol.network/#/)\
Telegram: [https://t.me/DecentralCardNetwork](https://t.me/DecentralCardNetwork)\
Discord: [https://discord.gg/ZKKbhUs](https://discord.gg/ZKKbhUs)\
X: [https://twitter.com/CrowdControlNet](https://twitter.com/CrowdControlNet)\
\
Node39 support:

* [x] RPC: [https://crowdcontrol-testnet-rpc.node39.top](https://crowdcontrol-testnet-rpc.node39.top)
* [x] API: [https://crowdcontrol-testnet-rpc.node39.top](https://crowdcontrol-testnet-rpc.node39.top)
* [x] Snapshort: Every 12 hours
* [x] Explorer: [https://explorer.node39.top/crowncontrol-testnet/staking](https://explorer.node39.top/crowncontrol-testnet/staking)

**Command:**

```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable Cardchaind

# Disable Service
sudo systemctl disable Cardchaind

# Start Service
sudo systemctl start Cardchaind

# Stop Service
sudo systemctl stop Cardchaind

# Restart Service
sudo systemctl restart Cardchaind

# Check Service Status
sudo systemctl status Cardchaind

# Check Service Logs
sudo journalctl -u Cardchaind -f --no-hostname -o cat
```
