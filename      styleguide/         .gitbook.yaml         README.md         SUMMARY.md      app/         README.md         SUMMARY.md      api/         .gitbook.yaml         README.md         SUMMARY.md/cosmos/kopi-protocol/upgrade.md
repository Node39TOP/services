# 🖥️ Upgrade

Chain: kopi-test-5

Version: 0.6.4.1

**Download Binary:**

```bash
cd $HOME
wget -O kopid https://github.com/kopi-money/kopi/releases/download/v0.6.4.1/kopid-v0.6.4.1-linux-amd64
chmod +x kopid
mv kopid /usr/local/bin
kopid version
sudo systemctl restart kopid
sudo journalctl -u kopid -f -o cat
```
