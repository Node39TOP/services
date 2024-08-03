# 🖥️ Upgrade

Chain: buenavista-1

Version: 0.3.1

**Download Binary Warden Protocol:**

<pre class="language-bash"><code class="lang-bash">// amd64
sudo systemctl stop wardend

cd $HOME
rm -rf warden
wget https://github.com/warden-protocol/wardenprotocol/releases/download/v0.3.2/wardend_Linux_x86_64.zip
unzip wardend_Linux_x86_64.zip &#x26;&#x26; rm -rf wardend_Linux_x86_64.zip
chmod +x wardend
sudo mv wardend /usr/local/bin
wardend version

sudo systemctl restart wardend
sudo journalctl -u wardend -f --no-hostname -o cat

//arm64
sudo systemctl stop wardend

cd $HOME
rm -rf warden
wget https://github.com/warden-protocol/wardenprotocol/releases/download/v0.3.2/wardend_Linux_arm64.zip
unzip wardend_Linux_arm64.zip &#x26;&#x26; rm -rf wardend_Linux_arm64.zip
chmod +x wardend
sudo mv wardend /usr/local/bin
wardend version

sudo systemctl restart wardend
<strong>sudo journalctl -u wardend -f --no-hostname -o cat
</strong></code></pre>
