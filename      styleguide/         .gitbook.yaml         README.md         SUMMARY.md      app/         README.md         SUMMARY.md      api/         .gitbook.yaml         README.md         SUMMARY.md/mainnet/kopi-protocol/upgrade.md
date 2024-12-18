# 🖥️ Upgrade

Chain: luwak-1

Version: 0.6.5.2

**Download Binary:**

```bash
cd $HOME
git clone --quiet --depth 1 --branch v0.6.5.2 https://github.com/kopi-money/kopi.git
cd kopi
make install
kopid version --long | grep -e commit -e version
// or
cd $HOME
wget -O kopid https://github.com/kopi-money/kopi/releases/download/v0.6.5.2/kopid-v0.6.5.2-linux-amd64-static
chmod +x $HOME/kopid
mv kopid /usr/local/bin
sudo systemctl restart kopid && sudo journalctl -u kopid -fo cat
```
