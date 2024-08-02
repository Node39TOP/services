# 🖥️ Upgrade

**Update Pactus version 1.4.0**

**amd64**

```bash
cd $HOME && rm -rf node_pactus && \
wget https://github.com/pactus-project/pactus/releases/download/v1.4.0/pactus-cli_1.4.0_linux_amd64.tar.gz && \
tar -xzf pactus-cli_1.4.0_linux_amd64.tar.gz && \
rm -rf pactus-cli_1.4.0_linux_amd64.tar.gz && \
mv pactus-cli_1.4.0 node_pactus && cd node_pactus
```

**arm64**

```bash
cd $HOME && rm -rf node_pactus && \
wget https://github.com/pactus-project/pactus/releases/download/v1.4.0/pactus-cli_1.4.0_linux_arm64.tar.gz && \
tar -xzf pactus-cli_1.4.0_linux_arm64.tar.gz && \
rm -rf pactus-cli_1.4.0_linux_arm64.tar.gz && \
mv pactus-cli_1.4.0 node_pactus && cd node_pactus
```
