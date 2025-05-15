# Phase 5: Remote Access & Self-Hosting

This phase teaches you how to **safely expose your MongoDB server to other devices**, including secure connections over the internet or local networks.

You’ll learn how to:

🔐 Encrypt remote access with TLS certificates<br>
🛡️ Harden your server with VPN or SSH tunnels<br>
🚫 Block brute-force attacks with Fail2Ban<br>
📈 Monitor your database’s health and usage

> This phase assumes your MongoDB server is hosted on a computer you control — either a home server, virtual machine, or cloud instance.

---

## Step 1: Enable Remote Access (Optional but Powerful)

By default, MongoDB only allows connections from `localhost` (127.0.0.1). If you want to access it from another device (your laptop, for example), you’ll need to allow external connections.

### A. Change Bind Address

Open your MongoDB config file:

```bash
sudo nano /etc/mongod.conf
```

Find the `net:` section and change:

```yaml
net:
  bindIp: 0.0.0.0
  port: 27017
```

> This allows MongoDB to accept connections from **any network interface** — not just `localhost`.

> **Warning:** Don’t expose MongoDB to the internet without authentication and encryption.

Restart MongoDB:

```bash
sudo systemctl restart mongod
```

Now MongoDB will listen on your public and private IP addresses.

### B. Allow Access in Firewall

If you trust a specific remote IP (e.g., `192.168.1.42`), allow it:

```bash
sudo ufw allow from 192.168.1.42 to any port 27017
```

To allow access from your whole LAN (adjust subnet as needed):

```bash
sudo ufw allow from 192.168.1.0/24 to any port 27017
```

To check:

```bash
sudo ufw status
```

---

## Step 2: Enable TLS Encryption with Self-Signed Certificate

Without encryption, your login and data are sent in plain text. Let’s add **TLS/SSL**.

### A. Generate a Self-Signed Certificate

You can use OpenSSL to generate your own cert:

```bash
openssl req -newkey rsa:4096 -nodes -keyout mongodb.key -x509 -days 365 -out mongodb.crt
```

Combine the key and cert into a `.pem` file:

```bash
cat mongodb.key mongodb.crt > mongodb.pem
```

Move it to a secure location:

```bash
sudo mv mongodb.pem /etc/ssl/mongodb.pem
sudo chmod 600 /etc/ssl/mongodb.pem
```

### B. Update `mongod.conf`

Edit:

```bash
sudo nano /etc/mongod.conf
```

Add or modify:

```yaml
net:
  port: 27017
  bindIp: 0.0.0.0
  tls:
    mode: requireTLS
    certificateKeyFile: /etc/ssl/mongodb.pem
```

Restart MongoDB:

```bash
sudo systemctl restart mongod
```

Your MongoDB server is now protected with TLS.

### C. Connect with TLS

From Compass, check **“More Options” → SSL** and choose “Server Validates Only”.

From shell:

```bash
mongosh --tls --tlsCAFile mongodb.crt --host your_server_ip
```

You’ll also need authentication (`-u`, `-p`, `--authenticationDatabase admin`).

---

## Step 3: Harden Access with SSH or VPN

To avoid exposing port `27017` to the open internet, use **secure tunnels**.

### Option 1: SSH Tunnel (Simple & Fast)

From your client machine:

```bash
ssh -L 27017:localhost:27017 user@your_server_ip
```

Now on your local machine, you can run:

```bash
mongosh --host localhost --port 27017
```

Or connect via Compass using `mongodb://localhost:27017`.

This traffic is forwarded over SSH and never exposed.

### Option 2: VPN (Private Networking)

Use tools like:

* `tailscale` (zero-config, automatic)
* `openvpn` or `wireguard` (more control)

Once connected, MongoDB will appear like it’s on your LAN — secure, private, and authenticated.

---

## Step 4: Prevent Brute-Force Attacks

If you expose MongoDB to the internet (even with TLS), protect it from repeated login attempts.

### A. Install Fail2Ban

```bash
sudo apt install fail2ban
```

Create a MongoDB rule (optional, advanced users can write custom jails). Basic protection is already available if SSH is exposed.

Also consider firewall rate limiting:

```bash
sudo ufw limit ssh
```

This slows down attackers who try to brute-force your login.

---

## Step 5: Monitor MongoDB Performance

Use MongoDB’s built-in monitoring tools:

### A. `mongotop`: View Collection Usage

```bash
mongotop
```

Shows which collections are being read or written to — live.

### B. `mongostat`: Live Stats Summary

```bash
mongostat
```

Shows inserts, queries, connections, memory, and more.

Use this to spot unusual spikes or stalls.

---

## Summary: What You Learned in Phase 5

✅ Allow secure remote access via IP and port<br>
✅ Encrypt connections using TLS (self-signed certs)<br>
✅ Tunnel access with SSH or VPN<br>
✅ Harden server against brute-force attacks<br>
✅ Monitor database usage and performance

With this phase complete, your MongoDB server is now **ready for secure remote access and long-term self-hosting**.

---

## Next Step

🚀 [Capstone](https://github.com/tims-computer-academy/mongodb/blob/main/capstone.md)
