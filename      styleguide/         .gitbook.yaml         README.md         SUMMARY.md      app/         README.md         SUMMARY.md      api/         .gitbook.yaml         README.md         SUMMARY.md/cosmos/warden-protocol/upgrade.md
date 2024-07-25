# 🖥️ Upgrade

Chain:&#x20;

Version: 0.3.0

**Download Binary Warden Protocol:**

```bash
// amd64

cd $HOME
rm -rf warden
wget https://github.com/warden-protocol/wardenprotocol/releases/download/v0.3.0/wardend_Linux_x86_64.zip
unzip wardend_Linux_x86_64.zip && rm -rf wardend_Linux_x86_64.zip
chmod +x wardend
sudo mv wardend /usr/local/bin
wardend version

//arm64

cd $HOME
rm -rf warden
wget https://github.com/warden-protocol/wardenprotocol/releases/download/v0.3.0/wardend_Linux_arm64.zip
unzip wardend_Linux_arm64.zip && rm -rf wardend_Linux_arm64.zip
chmod +x wardend
sudo mv wardend /usr/local/bin
wardend version
```
