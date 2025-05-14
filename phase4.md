# Phase 4: Local Security for Daily Use

This phase teaches you how to **secure your local MongoDB server** for responsible, long-term use on your own system.

By default, MongoDB is open and unauthenticated — good for learning, but risky for real data. We’ll now configure basic protections:

🔒 Require login credentials (authentication)<br>
🔒 Assign specific user roles (read-only, admin, etc.)<br>
🔒 Block outside access with a firewall<br>
🔒 Make encrypted backups<br>
🔒 Understand how MongoDB logs user actions

---

## Step 1: Enable MongoDB Authentication

MongoDB starts with **no login required**. Let’s fix that.

### A. Create a MongoDB Admin User

First, open the MongoDB shell **before** enabling authentication:

```bash
mongosh
```

Switch to the `admin` database (MongoDB’s system-wide config DB):

```js
use admin
```

Create a new admin user:

```js
db.createUser({
  user: "localadmin",
  pwd: "strong_password_here",
  roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
})
```

This user can create other users and manage access across all databases.

You can now exit `mongosh`:

```js
exit
```

### B. Edit `mongod.conf` to Require Login

Find your MongoDB config file — usually here:

```
/etc/mongod.conf
```

Open it in a text editor:

```bash
sudo nano /etc/mongod.conf
```

Scroll down (or add this section if missing):

```yaml
security:
  authorization: enabled
```

Save and close the file.

Restart MongoDB to apply the change:

```bash
sudo systemctl restart mongod
```

Now authentication is required for any operation.

---

## Step 2: Connect with Login Credentials

Try opening the MongoDB shell again:

```bash
mongosh
```

Now log in with your new admin user:

```js
use admin
db.auth("localadmin", "strong_password_here")
```

If successful, you’re now authenticated.

To skip `db.auth(...)` every time, you can also connect like this:

```bash
mongosh -u localadmin -p --authenticationDatabase admin
```

(MongoDB will prompt you for the password.)

---

## Step 3: Create Limited Users

You don’t want to use the `admin` user for everyday work. Let’s create a limited user for your database.

### Create a Read/Write User for Your Archive

In the shell, after logging in as admin:

```js
use personal_archive
db.createUser({
  user: "archive_user",
  pwd: "another_strong_password",
  roles: [ { role: "readWrite", db: "personal_archive" } ]
})
```

You can now log in as this user when working on your files and metadata.

---

## Step 4: Restrict Network Access (Firewall)

By default, MongoDB listens on **all interfaces** (e.g. `0.0.0.0`), but we want to limit access to **localhost only** — especially if your computer is ever on a public network.

### A. Limit Access to Localhost Only

Edit the config file:

```bash
sudo nano /etc/mongod.conf
```

Find the `net:` section and make sure it says:

```yaml
net:
  bindIp: 127.0.0.1
  port: 27017
```

This ensures that only local programs (on your machine) can connect.

Restart the server:

```bash
sudo systemctl restart mongod
```

### B. Add a Firewall Rule (optional but good)

If you use `ufw`:

```bash
sudo ufw allow from 127.0.0.1 to any port 27017
```

Then deny everything else:

```bash
sudo ufw deny 27017
```

Check your rules:

```bash
sudo ufw status
```

This makes sure nobody else can connect to your MongoDB server from the outside.

---

## Step 5: Make Encrypted Backups

You should back up your database regularly — and encrypt the backups in case someone else gains access.

### A. Dump Your Database

```bash
mongodump --db personal_archive --out /home/yourname/mongodb_backups
```

This creates a folder of `.bson` and `.json` files — the full dump.

### B. Encrypt the Backup (GPG or OpenSSL)

#### Option 1: GPG

```bash
gpg -c /home/yourname/mongodb_backups/personal_archive.bson
```

This encrypts the file with a password.

#### Option 2: OpenSSL

```bash
openssl enc -aes-256-cbc -salt -in backup.bson -out backup.bson.enc
```

Choose strong encryption and store passwords safely (e.g., password manager or offline).

---

## Step 6: Understand Logs and Auditing

MongoDB logs activity to a file — usually:

```
/var/log/mongodb/mongod.log
```

You can view it:

```bash
sudo less /var/log/mongodb/mongod.log
```

You’ll see events like:

* Connections opened
* Authentication attempts
* Commands run (e.g., queries, inserts)

If needed, you can enable deeper **audit logging**, but that’s more advanced and only used for strict monitoring.

---

## Summary: What You Learned in Phase 4

✅ Require logins for MongoDB access<br>
✅ Create admin and read/write users<br>
✅ Limit access to localhost<br>
✅ Use `ufw` or `iptables` to block outside traffic<br>
✅ Make secure, encrypted backups<br>
✅ Read logs to understand user actions

With these protections, your MongoDB database is now suitable for **private, daily use** — safe from unauthorized access, even on a shared network.

---

## Next Step

🚀 [Phase 5](https://github.com/tims-computer-academy/mongodb/blob/main/phase5.md)
