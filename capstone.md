# Phase 6: Capstone

This final phase turns your MongoDB setup into a **professional, self-sustaining data archive** — secure, automated, and built for the long haul.

You’ve already built a local, secure, authenticated server. Now you’ll learn how to:

⚙️ Schedule recurring tasks with `cron`<br>
📦 Sync encrypted data to other devices or the cloud<br>
📡 Get notified when something goes wrong<br>
🗃️ Manage long-term data retention<br>
📊 Monitor system activity and performance

> **Note:** Backup creation and encryption were covered in **Phase 4: Local Security**. This phase assumes you already generate backups and store them safely.

---

## Step 1: Sync Backups to Another Location

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

## Step 2: Add Logging and Alerts

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

## Step 3: Cleanup and Retention

You don’t need hundreds of old backups sitting around forever.

### Auto-delete Old Backups

In your script, remove backups older than 30 days:

```bash
find /home/yourname/mongodb_backups/* -type d -mtime +30 -exec rm -rf {} \;
```

This keeps your disk space under control.

---

## Step 4: Monitor the Database (Optional)

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

Your MongoDB archive is now:

✅ Secure<br>
✅ Backed up<br>
✅ Synced<br>
✅ Automated<br>
✅ Self-monitoring

This setup is stable enough to run unattended for months or years — ideal for private archives, project data, personal records, or long-term documentation.

---

## Finish

🏁 Click here to reach the finish: [Finish](https://github.com/tims-computer-academy/mongodb/blob/main/finish.md)
