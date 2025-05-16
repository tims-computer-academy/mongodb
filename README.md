# Learning Path: MongoDB

**Objectives**

* Store structured and unstructured data (e.g., PDFs, images)
* Use MongoDB daily like in enterprise environments
* Stay fully open source (no paid services)
* Prioritize **information security**

**Environment**
* Ubuntu Linux

---

## Phase 0: Install & Secure MongoDB Locally

| Topic                   | Goal                                             |
| ----------------------- | ------------------------------------------------ |
| Install via `apt`       | Install the latest MongoDB Community Edition     |
| Verify service status   | Ensure MongoDB starts and runs as a systemd unit |
| Use `mongosh`           | Interact with MongoDB shell                      |
| Configure default paths | Verify data (`/var/lib/mongodb`) and logs        |
| Lock down local access  | Bind MongoDB to localhost only                   |

## Phase 1: Store & Retrieve Files with GridFS

| Topic           | Goal                                         |
| --------------- | -------------------------------------------- |
| What is GridFS? | Learn how MongoDB stores large files         |
| Store a PDF     | Upload a PDF file to MongoDB                 |
| Retrieve a PDF  | Download it back from the DB to disk         |
| Add metadata    | Attach tags, source info, descriptions, etc. |

## Phase 2: MongoDB CRUD & Querying Basics

| Topic                | Goal                                   |
| -------------------- | -------------------------------------- |
| Insert & update docs | Store structured data                  |
| Query with filters   | Find documents with conditions         |
| Projection & sorting | Return specific fields and sorted docs |
| Delete & bulk ops    | Remove or change large sets of data    |

## Phase 3: GUI Tools & Visual Data Management

| Topic                  | Goal                                  |
| ---------------------- | ------------------------------------- |
| Use MongoDB Compass    | GUI for document viewing & CRUD       |
| GridFS file inspection | View stored PDFs & metadata visually  |
| Visual query builder   | Create queries without writing code   |
| Optional: Mongoku      | Try a lightweight open-source web GUI |

## Phase 4: Local Security for Daily Use

| Topic                 | Goal                                        |
| --------------------- | ------------------------------------------- |
| Enable auth           | Require login for any MongoDB access        |
| User roles            | Limit privileges (read-only, admin, etc.)   |
| Local firewall rules  | Block external access, only allow localhost |
| Encrypted backups     | Dump and encrypt data regularly             |
| Scheduled backups     | Automate daily encrypted dumps              |
| Logging & audit trail | Track who did what in the DB                |

## Phase 5: Remote Access & Self-Hosting

| Topic                      | Goal                                      |
| -------------------------- | ----------------------------------------- |
| Network exposure config    | Allow secure access from other devices    |
| TLS with self-signed certs | Encrypt traffic between client/server     |
| SSH or VPN tunneling       | Harden access from remote systems         |
| Fail2Ban & firewall tuning | Prevent brute-force & unauthorized access |
| Monitoring usage           | Check DB health, performance, backups     |

---

## Phase 6: Capstone

| Topic                      | Goal                                     |
| -------------------------- | ---------------------------------------- |
| Remote/offsite sync        | Mirror backups to other systems or cloud |
| Logging and notifications  | Track success/failure, get alerts        |
| Retention and cleanup      | Manage storage space with auto-deletion  |
| Live monitoring (optional) | Observe DB performance and activity      |

---

🚀 [Let's begin: Install & secure MongoDB locally](https://github.com/tims-computer-academy/mongodb/blob/main/phase0.md)
