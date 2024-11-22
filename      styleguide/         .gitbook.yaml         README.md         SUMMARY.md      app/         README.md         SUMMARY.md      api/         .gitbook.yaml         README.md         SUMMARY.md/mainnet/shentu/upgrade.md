# 🖥️ Upgrade

Update version 2.13.1

amd64:

```bash
cd $HOME
rm -rf shentu
git clone https://github.com/shentufoundation/shentu
cd shentu
git checkout v2.13.1
make install
shentud version
sudo systemctl restart shentud 
sudo journalctl -u shentud -f -o cat
```

arm64:

<pre class="language-bash"><code class="lang-bash">cd $HOME
rm -rf shentu
git clone https://github.com/shentufoundation/shentu
cd shentu
git checkout v2.13.1
GOARCH=arm64 GOOS=linux make build
chmod +x $HOME/shentu/build/shentud
mv $HOME/shentu/build/shentud /usr/local/bin
<strong>sudo systemctl restart shentud 
</strong>sudo journalctl -u shentud -f -o cat
</code></pre>
