# 🖥️ Upgrade

**Update Pactus version 1.3.0**

**amd64**

```
cd $HOME && rm -rf node_pactus && \
wget https://github.com/pactus-project/pactus/releases/download/v1.3.0/pactus-cli_1.3.0_linux_amd64.tar.gz && \
tar -xzf pactus-cli_1.3.0_linux_amd64.tar.gz && \
rm -rf pactus-cli_1.3.0_linux_amd64.tar.gz && \
mv pactus-cli_1.3.0 node_pactus && cd node_pactus
```

**arm64**

```
cd $HOME && rm -rf node_pactus && \
wget https://github.com/pactus-project/pactus/releases/download/v1.3.0/pactus-cli_1.3.0_linux_arm64.tar.gz && \
tar -xzf pactus-cli_1.3.0_linux_arm64.tar.gz && \
rm -rf pactus-cli_1.3.0_linux_arm64.tar.gz && \
mv pactus-cli_1.3.0 node_pactus && cd node_pactus
```
