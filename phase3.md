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

Choose the “Community” edition (free) and install it like any other desktop app.

Then launch Compass.

---

## Step 2: Connect to Your Local MongoDB Server

In the connection dialog:

1. Under “Connection String”, enter:

   ```
   mongodb://localhost:27017
   ```

2. Click **Connect**.

You should now see a list of databases.

Click on **`personal_archive`** — the one you created in Phase 1.

---

## Step 3: Explore Your Collections

Click on the database `personal_archive`.

You’ll see two types of collections:

### A. GridFS Collections (for files)

* `fs.files`: Contains file metadata
* `fs.chunks`: Contains binary file chunks

These were created automatically when you uploaded files via `mongofiles`.

Click on `fs.files` to browse uploaded files. You’ll see fields like:

* `filename`
* `length` (file size)
* `chunkSize`
* `uploadDate`
* `metadata` (if you added any)

### B. Metadata Collection (for structured data)

* `file_metadata`: The collection you created in Phase 2

Click on it to view the structured documents you inserted — descriptions, tags, dates, booleans, etc.

---

## Step 4: Use the Visual Query Builder

You can find specific documents without writing JavaScript.

1. Go to the **file\_metadata** collection
2. Click **"Filter"**

Try one of these examples (without needing code):

### Find unreviewed documents

In the filter editor:

```json
{ "reviewed": false }
```

Click **Apply**.

### Find documents with a certain tag

```json
{ "tags": "intel" }
```

Compass automatically searches arrays just like the shell.

---

## Step 5: Add or Edit Documents Visually

In **any collection** (e.g. `file_metadata` or `fs.files`):

* Click the **Edit** button next to a document
* Change fields or add new ones using the visual editor
* Click **Update** to save your changes

You can also click **Add Data > Insert Document** to create a new entry manually.

This is useful if you're managing metadata by hand or adding notes to files you’ve uploaded.

---

## Step 6: Inspect GridFS Files Visually

Go to `fs.files` and click on a document.

You’ll see file info like:

* `filename`
* `length` (in bytes)
* `uploadDate`
* any custom `metadata` you added in Phase 1

GridFS chunks (`fs.chunks`) are not meant to be read manually — but they’re stored in the background and linked via `files_id`.

---

## Step 7: (Optional) Try Mongoku – Web-Based GUI

If you prefer a **lightweight**, open-source, web-based GUI (instead of Compass), you can try [Mongoku](https://github.com/huggingface/Mongoku).

To install and run:

```bash
npm install -g mongoku
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

✅ Install and launch MongoDB Compass
✅ Connect to your database using a GUI
✅ Browse documents and GridFS files visually
✅ Use the visual query builder for searching
✅ Edit or insert documents manually
✅ Optionally try Mongoku (web-based GUI)

This phase gives you a **hands-on interface** to manage your data — especially helpful when inspecting or cleaning up information.

---

## Next Step

🚀 [Phase 4](https://github.com/tims-computer-academy/path_adv_mongodb/blob/main/phase4.md)

> You’re ready to start designing your own metadata schemas, apply indexing for faster search, or even start building front-ends that connect to MongoDB.
