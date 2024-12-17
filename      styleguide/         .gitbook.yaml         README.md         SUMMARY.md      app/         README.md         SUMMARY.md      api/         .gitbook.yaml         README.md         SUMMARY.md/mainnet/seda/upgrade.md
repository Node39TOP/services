# 🖥️ Upgrade

**Chain: seda-1**

**Version: 0.1.5**

**Download binary Seda: (amd-64)**

<pre><code><strong>wget -O sedad https://github.com/sedaprotocol/seda-chain/releases/download/v0.1.5/sedad-amd64
</strong>chmod +x sedad
sudo mv sedad /usr/local/bin
sedad version

sudo systemctl restart sedad
sudo journalctl -u sedad -fo cat
</code></pre>

**Download binary Seda: (arm-64)**

<pre><code>wget -O sedad https://github.com/sedaprotocol/seda-chain/releases/download/v0.1.5/sedad-arm64
<strong>chmod +x sedad
</strong>sudo mv sedad /usr/local/bin
sedad version

sudo systemctl restart sedad
sudo journalctl -u sedad -fo cat
</code></pre>
