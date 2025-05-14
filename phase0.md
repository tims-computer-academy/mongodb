## Phase 0: Install MongoDB (Community Edition) via `apt`

### Step 1: Import the MongoDB public GPG key

```bash
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
```

### Step 2: Create the MongoDB source list file

Replace `bookworm` with your distro codename if needed (e.g., `bullseye`, `buster`):

```bash
echo "deb [signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg] https://repo.mongodb.org/apt/debian bookworm/mongodb-org/7.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

To check your codename:

```bash
lsb_release -cs
```

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
