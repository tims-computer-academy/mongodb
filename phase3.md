# Phase 3: Use MongoDB Compass (GUI for Your Database)

This phase teaches you how to explore and manage your MongoDB database visually — no code required.

You’ll use **MongoDB Compass**, a free, open-source desktop tool from the official MongoDB team. It lets you:

👍🏼 Browse collections like folders<br>
👍🏼 View and edit individual documents<br>
👍🏼 Run queries without writing code<br>
👍🏼 Inspect files and metadata stored in GridFS<br>

---

## Step 1: Install and Open MongoDB Compass

Download Compass from the official MongoDB website:

**🔗 [https://www.mongodb.com/try/download/compass](https://www.mongodb.com/try/download/compass)**

Choose the “Community” edition (free) and install it like any other desktop app; if necessary, change the file's permissions and install it again so _apt can access it. In Ubuntu, e.g.:

```bash
sudo apt install ./mongodb-compass_1.46.2_amd64.deb
sudo chmod 644 ./mongodb-compass_1.46.2_amd64.deb # If necessary
sudo apt install ./mongodb-compass_1.46.2_amd64.deb # If necessary
```

Then launch Compass: `mongodb-compass`

---

## Step 2: Connect to Your Local MongoDB Server

In Compass, click "**+ Add new connection**".

1. Under “URI”, check that the following connection string is configured:

   ```
   mongodb://localhost:27017
   ```

2. Click **Connect**.

You should now see a list of databases.

Click on **`personal_archive`** — the one you created in Phase 1.

---

## Step 3: Explore Your Collections

After you have clicked on your database `personal_archive`, you'll see two types of collections:

### A. GridFS Collections (for files)

* `fs.files`: Contains file metadata
* `fs.chunks`: Contains binary file chunks

These were created automatically when you uploaded files via `mongofiles`.

Click on `fs.files` to browse uploaded files. You’ll see the fields:

* `_id`
* `length` (in bytes)
* `chunkSize`
* `uploadDate
* `filename`
* `metadata` (if you added any)

### B. Metadata Collection (for structured data)

* `file_metadata`: The collection you created in Phase 2

Click on it to view the structured documents you inserted — descriptions, tags, dates, booleans, etc.

---

## Step 4: Use the Visual Query Builder

You can find specific documents without writing JavaScript.

1. Go to the **file\_metadata** collection
2. Click in the **filter field** (filter editor) and try one of these examples (without needing code):

### Find unreviewed documents

In the filter editor:

```json
{ "reviewed": false }
```

Click **Find**.

### Find documents with a certain tag

```json
{ "tags": "intel" }
```

Compass automatically searches arrays just like the shell.

---

## Step 5: Add or Edit Documents Visually

In **any collection** (e.g. `file_metadata` or `fs.files`):

* Click the **Edit document** button next to a document
* Change fields or add new ones using the visual editor
* Click **Update** to save your changes

You can also click **Add Data > Insert Document** to create a new entry manually.

This is useful if you're managing metadata by hand or adding notes to files you’ve uploaded.

---

## Step 6: Inspect GridFS Files Visually

Go to `fs.files`. Each document here represents a single file. This is the **structured metadata** that GridFS stores alongside each file.

> You **do not** need to inspect the `fs.chunks` collection directly — that’s where MongoDB stores the binary file data in pieces. It’s managed automatically and only useful for low-level troubleshooting.

---

## Step 7: (Optional) Try Mongoku – Web-Based GUI

If you prefer a **lightweight**, open-source, web-based GUI (instead of Compass), you can try [Mongoku](https://github.com/huggingface/Mongoku).

To install and run:

```bash
sudo npm install -g mongoku
mongoku
```

Then open your browser and go to:

```
http://localhost:3100
```

You’ll be able to:

* Connect to your local MongoDB server
* Browse collections
* View and edit documents
* Run queries in a browser tab

Mongoku is fast, minimal, and useful on remote servers if you want to expose a visual dashboard.

---

## Summary: What You Learned in Phase 3

✅ Install and launch MongoDB Compass<br>
✅ Connect to your database using a GUI<br>
✅ Browse documents and GridFS files visually<br>
✅ Use the visual query builder for searching<br>
✅ Edit or insert documents manually<br>
✅ Optionally try Mongoku (web-based GUI)

This phase gives you a **hands-on interface** to manage your data — especially helpful when inspecting or cleaning up information.

---

## Next Step

🚀 [Phase 4](https://github.com/tims-computer-academy/path_adv_mongodb/blob/main/phase4.md)
