# Learning Path: MongoDB

**Objectives**

* Store structured and unstructured data (e.g., PDFs, images)
* Use MongoDB daily like in enterprise environments
* Stay fully open source (no paid services)
* Prioritize **information security**

**Requirements**
* Debian-based Linux
* Use `apt` (package manager) for installation

---

## Phase 0: Install & Secure MongoDB Locally

| Topic                   | Goal                                             | Tools                       |
| ----------------------- | ------------------------------------------------ | --------------------------- |
| Install via `apt`       | Install the latest MongoDB Community Edition     | `apt`, MongoDB APT repo     |
| Verify service status   | Ensure MongoDB starts and runs as a systemd unit | `systemctl`, `mongod` logs  |
| Use `mongosh`           | Interact with MongoDB shell                      | `mongosh`                   |
| Configure default paths | Verify data (`/var/lib/mongodb`) and logs        | `mongod.conf`, `journalctl` |
| Lock down local access  | Bind MongoDB to localhost only                   | `mongod.conf`               |

## Phase 1: Store & Retrieve Files with GridFS

| Topic           | Goal                                         | Tools                   |
| --------------- | -------------------------------------------- | ----------------------- |
| What is GridFS? | Learn how MongoDB stores large files         | GridFS                  |
| Store a PDF     | Upload a PDF file to MongoDB                 | `mongofiles`, `mongosh` |
| Retrieve a PDF  | Download it back from the DB to disk         | `mongofiles`            |
| Add metadata    | Attach tags, source info, descriptions, etc. | BSON/JSON documents     |

## Phase 2: MongoDB CRUD & Querying Basics

| Topic                | Goal                                   | Tools                                    |
| -------------------- | -------------------------------------- | ---------------------------------------- |
| Insert & update docs | Store structured data                  | `insertOne()`, `updateOne()`             |
| Query with filters   | Find documents with conditions         | `find()`, operators (`$gt`, `$in`, etc.) |
| Projection & sorting | Return specific fields and sorted docs | `{ field: 1 }`, `.sort()`                |
| Delete & bulk ops    | Remove or change large sets of data    | `deleteMany()`, `updateMany()`           |

## Phase 3: GUI Tools & Visual Data Management

| Topic                  | Goal                                  | Tools                         |
| ---------------------- | ------------------------------------- | ----------------------------- |
| Use MongoDB Compass    | GUI for document viewing & CRUD       | MongoDB Compass (open source) |
| GridFS file inspection | View stored PDFs & metadata visually  | Compass file browser tab      |
| Visual query builder   | Create queries without writing code   | Compass query builder         |
| Optional: Mongoku      | Try a lightweight open-source web GUI | Mongoku                       |

## Phase 4: Local Security for Daily Use

| Topic                 | Goal                                        | Tools                           |
| --------------------- | ------------------------------------------- | ------------------------------- |
| Enable auth           | Require login for any MongoDB access        | `mongod.conf`, `createUser()`   |
| User roles            | Limit privileges (read-only, admin, etc.)   | MongoDB roles system            |
| Local firewall rules  | Block external access, only allow localhost | `ufw`, `iptables`               |
| Encrypted backups     | Keep data and files safe                    | `mongodump`, GPG or OpenSSL     |
| Logging & audit trail | Track who did what in the DB                | MongoDB logs, optional auditing |

## Phase 5: Remote Access & Self-Hosting

| Topic                      | Goal                                      | Tools                               |
| -------------------------- | ----------------------------------------- | ----------------------------------- |
| Network exposure config    | Allow secure access from other devices    | `mongod.conf`, IP binding           |
| TLS with self-signed certs | Encrypt traffic between client/server     | OpenSSL                             |
| SSH or VPN tunneling       | Harden access from remote systems         | `ssh`, `openvpn`, `tailscale`       |
| Fail2Ban & firewall tuning | Prevent brute-force & unauthorized access | `fail2ban`, `ufw`                   |
| Monitoring usage           | Check DB health, performance, backups     | `mongotop`, `mongostat`, shell logs |

---

## Phase 6: Capstone

| Topic                      | Goal                                          | Tools                                |
| -------------------------- | --------------------------------------------- | ------------------------------------ |
| Automated backups          | Ensure reliable daily data backups            | `cron`, `mongodump`, shell scripts   |
| Encrypted backup storage   | Protect backup files from unauthorized access | `gpg`, `openssl`                     |
| Remote/offsite sync        | Mirror backups to other systems or cloud      | `rsync`, `rclone`, SFTP              |
| Logging and notifications  | Track success/failure, get alerts             | `mailutils`, `cron logs`, shell logs |
| Retention and cleanup      | Manage storage space with auto-deletion       | `find`, `rm`, shell script logic     |
| Live monitoring (optional) | Observe DB performance and activity           | `mongotop`, `mongostat`              |

---

## Security Principles Throughout

* MongoDB will be **auth-protected** before any external access
* **Only localhost bound by default**
* Files and backups will be **encrypted**
* Only **open-source, offline tools** will be used

---

🚀 [Let's begin!](https://github.com/tims-computer-academy/mongodb/blob/main/phase0.md)
