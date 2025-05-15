# Phase 4: Local Security for Daily Use

This phase teaches you how to **secure your local MongoDB server** and **back up your data safely**.

By default, MongoDB is open and unauthenticated — good for learning, but risky for real data. We’ll now configure basic protections:

🔒 Require login credentials (authentication)
🔒 Assign specific user roles (read-only, admin, etc.)
🔒 Block outside access with a firewall
💾 Automate and encrypt local backups
📝 Understand how MongoDB logs user actions

---

## Step 1: Enable MongoDB Authentication

MongoDB starts with **no login required**. Let’s fix that.

### A. Create a MongoDB Admin User

Open the MongoDB shell **before** enabling authentication:

```bash
mongosh
```

Switch to the `admin` database:

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

To give full root access:

```js
db.grantRolesToUser("localadmin", [
  { role: "root", db: "admin" }
])
```

Exit:

```bash
exit
```

### B. Edit `mongod.conf` to Require Login

```bash
sudo nano /etc/mongod.conf
```

Ensure this is present:

```yaml
security:
  authorization: enabled
```

Restart MongoDB:

```bash
sudo systemctl restart mongod
```

---

## Step 2: Connect with Login Credentials

### In the MongoDB Shell

```bash
mongosh -u localadmin -p --authenticationDatabase admin
```

Or:

```bash
mongosh
use admin
db.auth("localadmin", "strong_password_here")
```

### In MongoDB Compass

Use **Username/Password** with:

* Username: `localadmin`
* Password: your password
* Auth DB: `admin`

Enable **Protect Connection String Secrets** for extra safety.

---

## Step 3: Create Limited Users

Log in as admin, then:

```js
use personal_archive
db.createUser({
  user: "archive_user",
  pwd: "another_strong_password",
  roles: [ { role: "readWrite", db: "personal_archive" } ]
})
```

Use this user for daily access to your personal archive.

---

## Step 4: Restrict Network Access (Firewall)

### A. Bind to Localhost Only

Edit:

```bash
sudo nano /etc/mongod.conf
```

Make sure this is present:

```yaml
net:
  bindIp: 127.0.0.1
  port: 27017
```

Restart MongoDB:

```bash
sudo systemctl restart mongod
```

### B. UFW Firewall (Optional but Recommended)

```bash
sudo ufw allow from 127.0.0.1 to any port 27017
sudo ufw deny 27017
sudo ufw status
```

---

## Step 5: Daily Encrypted Backups

A secure system always includes regular, **encrypted backups**.

### A. Create a Backup Script

```bash
mkdir -p ~/scripts
nano ~/scripts/backup_mongo.sh
```

Paste this:

```bash
#!/bin/bash

DATE=$(date +%Y-%m-%d)
BACKUP_DIR="/home/yourname/mongodb_backups/$DATE"

mongodump --db personal_archive --out "$BACKUP_DIR"

# Encrypt the backup (GPG)
gpg -c "$BACKUP_DIR"/personal_archive/*.bson
rm "$BACKUP_DIR"/personal_archive/*.bson

# Log the result
echo "Backup completed on $DATE" >> /var/log/mongo_backup.log
```

Make executable:

```bash
chmod +x ~/scripts/backup_mongo.sh
```

### B. Schedule Daily Cron Backup

Edit your crontab:

```bash
crontab -e
```

Add:

```cron
0 2 * * * /home/yourname/scripts/backup_mongo.sh
```

Backups now run automatically at 2:00 AM.

---

## Step 6: Understand Logs and Auditing

MongoDB logs activity here:

```bash
sudo less /var/log/mongodb/mongod.log
```

You can see:

* Auth attempts
* User sessions
* Commands and errors

For deeper auditing, see: [MongoDB Audit Logs](https://www.mongodb.com/docs/manual/tutorial/configure-audit-log/)

---

## Summary: What You Learned in Phase 4

✅ Require logins for MongoDB access
✅ Create admin and read/write users
✅ Limit access to localhost only
✅ Use `ufw` or `iptables` to block outside traffic
✅ Automate encrypted daily backups
✅ Read logs to monitor activity

You now have a MongoDB system that is safe, encrypted, and backed up — suitable for private use on a personal or shared machine.

---

## Next Step

🚀 [Phase 5 → Remote Access and Self-Hosting](https://github.com/tims-computer-academy/mongodb/blob/main/phase5.md)
