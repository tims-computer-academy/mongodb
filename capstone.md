Absolutely — here is the finalized version of **Phase 6**, now titled **Capstone**, with wording adjusted accordingly:

---

# Phase 6: Capstone

This final phase turns your MongoDB setup into a **professional, self-sustaining data archive** — secure, automated, and built for the long haul.

You’ve already built a local, secure, authenticated server. Now you’ll learn how to:

⚙️ Automate backups and maintenance
📦 Sync encrypted data to other devices or the cloud
📅 Schedule recurring tasks with `cron`
📡 Get notified when something goes wrong
🗃️ Manage long-term data retention

---

## Step 1: Automate Daily Backups with `cron`

You shouldn’t have to remember to back up your data — your system should do it for you.

### A. Create a Backup Script

Make a file called `backup_mongo.sh`:

```bash
nano ~/scripts/backup_mongo.sh
```

Paste in:

```bash
#!/bin/bash

DATE=$(date +%Y-%m-%d)
BACKUP_DIR="/home/yourname/mongodb_backups/$DATE"

mongodump --db personal_archive --out "$BACKUP_DIR"

# Optional: Encrypt backup
gpg -c "$BACKUP_DIR"/personal_archive/*.bson
rm "$BACKUP_DIR"/personal_archive/*.bson

# Log completion
echo "Backup completed on $DATE" >> /var/log/mongo_backup.log
```

Make it executable:

```bash
chmod +x ~/scripts/backup_mongo.sh
```

### B. Run It Automatically

Edit your crontab:

```bash
crontab -e
```

Add this to run daily at 2:00 AM:

```bash
0 2 * * * /home/yourname/scripts/backup_mongo.sh
```

Now your backups happen quietly and automatically.

---

## Step 2: Sync Backups to Another Location

To protect against hardware failure, theft, or accidental deletion, mirror your encrypted backups elsewhere.

### Option A: Sync to a NAS or Remote Server

```bash
rsync -avz /home/yourname/mongodb_backups user@192.168.1.100:/backups/mongo
```

Schedule this via `cron` or add it to your backup script.

### Option B: Sync to the Cloud (e.g., SFTP or rclone)

Install and configure `rclone`:

```bash
sudo apt install rclone
rclone config
```

Upload your backups:

```bash
rclone copy /home/yourname/mongodb_backups remote:my_mongo_backups
```

Add this to your `cron` job or script.

---

## Step 3: Add Logging and Alerts

Backups that fail silently are a big risk. Add visibility.

### A. Log Success and Failure

In your backup script:

```bash
echo "[$(date)] Backup finished" >> /var/log/mongo_backup.log
```

For errors:

```bash
|| echo "[$(date)] Backup failed!" >> /var/log/mongo_backup.log
```

### B. Optional: Email on Failure

Install:

```bash
sudo apt install mailutils
```

Add:

```bash
if mongodump --db personal_archive --out "$BACKUP_DIR"; then
  echo "Backup succeeded"
else
  echo "MongoDB backup FAILED" | mail -s "MongoDB Backup Failed" your@email.com
fi
```

---

## Step 4: Cleanup and Retention

You don’t need hundreds of old backups sitting around forever.

### Auto-delete Old Backups

In your script, remove backups older than 30 days:

```bash
find /home/yourname/mongodb_backups/* -type d -mtime +30 -exec rm -rf {} \;
```

This keeps your disk space under control.

---

## Step 5: Monitor the Database (Optional)

Use MongoDB CLI tools to get live stats and check health.

### `mongotop` — Monitor Collection Activity

```bash
mongotop
```

### `mongostat` — Live Performance Snapshot

```bash
mongostat
```

These tools help with diagnosis and tuning but aren’t needed for casual use.

---

## Capstone Summary

✅ Automated daily backups
✅ Offsite backup syncing
✅ Logging and error alerts
✅ Retention and cleanup policies
✅ Live database monitoring (optional)

Your MongoDB archive is now:

✔️ Secure
✔️ Backed up
✔️ Synced
✔️ Automated
✔️ Self-monitoring

This setup is stable enough to run unattended for months or years — ideal for private archives, project data, personal records, or long-term documentation.

---

## Next?

You’ve completed the full MongoDB path for local self-hosting and personal archiving. From here, you can:

* Add cloud hosting or replication
* Set up user-friendly web apps on top of your data
* Explore advanced tooling like Grafana, Ansible, or Kubernetes
* Contribute your setup guide to others

Let me know if you’d like a post-capstone roadmap or just want to stop here.
