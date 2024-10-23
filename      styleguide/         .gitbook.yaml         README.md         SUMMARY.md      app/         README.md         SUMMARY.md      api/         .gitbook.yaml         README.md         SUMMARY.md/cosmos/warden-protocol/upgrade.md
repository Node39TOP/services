# 🖥️ Upgrade

Chain: chiado\_10010-1

Version: 0.5.2

**Download Binary Warden Protocol:**

```bash
// amd64
cd $HOME
rm -rf download
mkdir download
cd download
wget https://github.com/warden-protocol/wardenprotocol/releases/download/v0.5.2/wardend_Linux_x86_64.zip
unzip wardend_Linux_x86_64.zip
rm wardend_Linux_x86_64.zip
chmod +x ~/wardend
sudo mv ~/wardend /usr/local/bin
sudo systemctl restart wardend && sudo journalctl -u wardend -f -o cat

//arm64
cd $HOME
rm -rf download
mkdir download
cd download
wget https://github.com/warden-protocol/wardenprotocol/releases/download/v0.5.2/wardend_Linux_x86_64.zip
unzip wardend_Linux_arm64_64.zip
rm wardend_Linux_x86_64.zip
chmod +x ~/wardend
sudo mv ~/wardend /usr/local/bin
sudo systemctl restart wardend && sudo journalctl -u wardend -f -o cat
```
