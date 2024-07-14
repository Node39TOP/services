# 🛟 Command

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
