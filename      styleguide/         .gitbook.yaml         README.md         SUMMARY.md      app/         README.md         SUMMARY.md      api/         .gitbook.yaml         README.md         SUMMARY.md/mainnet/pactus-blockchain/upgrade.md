# 🖥️ Upgrade

**Download Pactus v1.6.3: (amd64)**

```bash
ver="1.6.3" && \
cd $HOME && rm -rf node_pactus && \
wget https://github.com/pactus-project/pactus/releases/download/v${ver}/pactus-cli_${ver}_linux_amd64.tar.gz && \
tar -xzf pactus-cli_${ver}_linux_amd64.tar.gz && \
rm -rf pactus-cli_${ver}_linux_amd64.tar.gz && \
mv pactus-cli_${ver} node_pactus && cd node_pactus
```

**Download Pactus v1.6.3: (arm64)**

<pre class="language-bash"><code class="lang-bash">ver="1.6.3" &#x26;&#x26; \
<strong>cd $HOME &#x26;&#x26; rm -rf node_pactus &#x26;&#x26; \
</strong>wget https://github.com/pactus-project/pactus/releases/download/v${ver}/pactus-cli_${ver}_linux_arm64.tar.gz &#x26;&#x26; \
tar -xzf pactus-cli_${ver}_linux_arm64.tar.gz &#x26;&#x26; \
rm -rf pactus-cli_${ver}_linux_arm64.tar.gz &#x26;&#x26; \
mv pactus-cli_${ver} node_pactus &#x26;&#x26; cd node_pactus
</code></pre>
