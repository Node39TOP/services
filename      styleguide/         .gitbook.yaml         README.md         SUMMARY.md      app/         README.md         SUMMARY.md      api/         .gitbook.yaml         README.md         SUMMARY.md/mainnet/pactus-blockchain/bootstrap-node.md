# 🔌 Bootstrap node

<mark style="color:red;">**Bootstrap node: (option)**</mark>

**Edit config:**

```bash
nano $HOME/pactus/config.toml
```

**Find \[http]**

```bash
// Edit

[http]
  enable = false -> true
  listen = "127.0.0.1:80" -> "127.0.0.1:8085"

// EX:
[http]
  enable = true
  listen = "127.0.0.1:8085"
```

**Strat node: (Run in tmux)**

```bash
sudo ./pactus-daemon start
```

**Check node:**

_<mark style="color:red;">Save info Peer ID and Your ID</mark>_

```bash
http://your_ip_node:8085/node
```

**Fork**: [https://github.com/pactus-project/pactus](https://github.com/pactus-project/pactus)

**Pull requests:** edit bootstrap.json

```bash
,
    {
        "name": "yourname",
        "email": "your_email",
        "website": "n/a",
        "address": "/ip4/your_ip/tcp/21888/p2p/your_peers_id"
    }
```

\-> **Pull request**

**Add a title:** chore: add bootstrap address for \<Name> (Change name)

\-> DONE PR.

**Edit Config:**

```bash
nano $HOME/pactus/config.toml
```

**Find \[network] and \[sync]**

```bash
// Default
[network]
  network_key = "network_key"
  public_addr = ""
  listen_addrs = []
  bootstrap_addrs = []
  max_connections = 64
  enable_udp = false
  enable_nat_service = false
  enable_upnp = false
  enable_relay = true
  enable_relay_service = false
  enable_mdns = false
  enable_metrics = false
  force_private_network = false
[sync]
  moniker = "moniker"
  session_timeout = "10s"
  node_network = true
  
// Edit
[network]
  network_key = "network_key"
  public_addr = ""
  listen_addrs = []
  bootstrap_addrs = []
  max_connections = 64
  enable_udp = false
  enable_nat_service = true         #here
  enable_upnp = false
  enable_relay = true
  enable_relay_service = false
  enable_mdns = false
  enable_metrics = false
  force_private_network = false
[sync]
  moniker = "Node39."              #here
  session_timeout = "10s"
  node_network = true
```

Save: Ctrl + o and Ctrl + x

\-> Complete
