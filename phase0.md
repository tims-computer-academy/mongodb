## Phase 0: Install MongoDB (Community Edition) via `apt`

### Step 1: Import the MongoDB public GPG key

```bash
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
```

---

### Step 2: Create the MongoDB source list file

Replace `bookworm` with your distro codename if needed (e.g., `bullseye`, `buster`):

```bash
echo "deb [signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg] https://repo.mongodb.org/apt/debian bookworm/mongodb-org/7.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

If you're not sure which codename your system uses, run:

```bash
lsb_release -a
# or
cat /etc/os-release
```

Here’s a list of popular Debian-based distros and their current (or most relevant) release codenames, which you’d use in an APT repo string like our MongoDB example.

**Debian**

| Version | Codename   |
| ------- | ---------- |
| 12      | `bookworm` |
| 11      | `bullseye` |
| 10      | `buster`   |

**Ubuntu**

| Version   | Codename   |
| --------- | ---------- |
| 24.04 LTS | `noble`    |
| 22.04 LTS | `jammy`    |
| 20.04 LTS | `focal`    |

🛑 Ubuntu users should use `https://repo.mongodb.org/apt/ubuntu` instead, with codename:

```
jammy/mongodb-org/7.0
```

🛑 MongoDB only supports Ubuntu LTS releases officially for APT repositories. As of now (May 2025), MongoDB does not provide official .deb packages for non-LTS versions like oracular. You can force it by using the jammy repo on 24.10, like this:

```bash
echo "deb [signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

But be warned: You may run into incompatibilities with newer libraries (GLIBC, systemd, libssl, etc.). This setup is not supported by MongoDB Inc. It may work or it may break subtly. Test thoroughly before deploying.

**Linux Mint**

| Version | Codename                    |
| ------- | --------------------------- |
| 21.x    | `vanessa`, `victoria`, etc. |
| 20.x    | `ulyssa`, `ulyana`, etc.    |

Mint uses Ubuntu repos under the hood, so follow the Ubuntu mapping.

**Others**

* **Kali Linux**: Based on **Debian Testing** → Use `bookworm` or upcoming `trixie`
* **Raspberry Pi OS**: Based on **Debian** → `bullseye` or `bookworm`

### Step 3: Update your package database

```bash
sudo apt update
```

### Step 4: Install MongoDB

```bash
sudo apt install -y mongodb-org
```

This installs:

* `mongod` – MongoDB server
* `mongosh` – shell client
* `mongofiles` – CLI tool for GridFS
* other core utilities

---

## Step 5: Start MongoDB and enable it on boot

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```

To check status:

```bash
sudo systemctl status mongod
```

You should see it **active (running)**.

---

## Step 6: Test the installation

```bash
mongosh
```

This should give you a working interactive shell connected to your local MongoDB server.

---

## Step 7: Lock MongoDB to localhost (security best practice)

Open the MongoDB config file:

```bash
sudo nano /etc/mongod.conf
```

Look for this section and make sure it looks like this:

```yaml
net:
  bindIp: 127.0.0.1
```

This ensures the server is only accessible from your own machine.

Then restart MongoDB:

```bash
sudo systemctl restart mongod
```

## Next Step

🚀 [Start with Phase 0]
