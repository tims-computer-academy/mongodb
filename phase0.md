## Phase 0: Install MongoDB (Community Edition) via `apt`

💡 The examples in this learning path are tested on Ubuntu 24.10 oracular. Where necessary, I make references to differences to other distributions and try to provide links to further resources.

### Step 1: Install GNU Privacy Guard & import the MongoDB public GPG key

Install GNU Privacy Guard:
```bash
sudo apt install -y wget gnupg
```

Add MongoDB's GPG key
```bash
wget -qO - https://www.mongodb.org/static/pgp/server-7.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-archive-keyring.gpg
```

### Step 2: Create the MongoDB source list file

Add the correct repository entry (e.g., in Ubuntu):
```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/mongodb-archive-keyring.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

For other Linux distribution, please see the instructions here: https://www.mongodb.com/docs/manual/administration/install-on-linux/

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

After the shell has started properly, you can close it again for the time being:

```bash
exit
```

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

If you had to make any changes, save & exit, then restart MongoDB:

```bash
sudo systemctl restart mongod
```

## Next Step

🚀 [Continue with Phase 1]
