# 🖥️ Upgrade

**Chain:**&#x20;

**Version:**&#x20;

**Download binary :**

```
ver="v1.0.6"
wget https://github.com/SigmaGmbH/swisstronik-chain/releases/download/testnet-${ver}/swisstronikd.zip
unzip swisstronikd.zip
sudo cp $HOME/bin/libsgx_wrapper_${ver}.x86_64.so /usr/lib
cp $HOME/bin/${ver}_enclave.signed.so $HOME/.swisstronik-enclave
chmod +x $HOME/bin/swisstronikd
sudo mv $HOME/bin/swisstronikd $(which swisstronikd)

Next: Edit systemd to v1.0.6 (Option)

sudo systemctl daemon-reload && sudo systemctl restart swisstronikd && journalctl -u swisstronikd -f -o cat

```
