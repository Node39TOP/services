---
icon: square-dollar
---

# Slinky

**Download Binary Slinky:**

```
cd $HOME
rm -rf connect
git clone https://github.com/skip-mev/connect.git
cd connect
git checkout v1.0.5
make install
slinky version
```

**Create a service Slinky**

```
SLINKY_PORT=$(grep 'address = ' "$HOME/.warden/config/app.toml" | awk -F: '{print $NF}' | grep '90"$' | tr -d '"')
echo $SLINKY_PORT

tee /etc/systemd/system/slinky.service > /dev/null <<EOF
[Unit]
Description=Slinky Oracle Warden
After=network-online.target

[Service]
User=$USER
ExecStart=$(which slinky) --market-map-endpoint="127.0.0.1:$SLINKY_PORT"
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```
systemctl daemon-reload
systemctl enable slinky
systemctl restart slinky && journalctl -u slinky -f -o cat
```
