Excellent. Let's begin **Phase 1: Store & Retrieve Files with GridFS**.
This phase focuses on **uploading, downloading, and managing files** (like PDFs) in MongoDB using **GridFS**, which is built for handling large binary files.

---

## Phase 1: Store & Retrieve Files with GridFS

### What is GridFS?

GridFS is MongoDB’s **file storage specification**. It breaks large files into chunks and stores them in two collections:

* `fs.files`: metadata (filename, upload date, size, etc.)
* `fs.chunks`: actual binary file content

Files over 16MB are automatically split into chunks. But even small files like PDFs can be stored using GridFS for unified file+metadata handling.

---

## Step 1: Create a test database

Let’s create a dedicated database for personal archive use.

```bash
mongosh
```

In the shell:

```js
use personal_archive
```

(Optional but good practice: confirm your current DB)

```js
db
```

---

## Step 2: Upload a PDF using `mongofiles`

Exit the shell (`Ctrl+D` or `exit`) and run this in the terminal:

```bash
mongofiles --db personal_archive put /full/path/to/your/document.pdf
```

Example:

```bash
mongofiles --db personal_archive put ~/Documents/test.pdf
```

This uploads the file to the `personal_archive` database, under default GridFS collections: `fs.files` and `fs.chunks`.

---

## Step 3: Verify the upload in `mongosh`

Reconnect to your database:

```bash
mongosh
use personal_archive
```

Check the `fs.files` collection:

```js
db.fs.files.find().pretty()
```

You should see an entry like:

```json
{
  "_id" : ObjectId("..."),
  "length" : 12345,
  "chunkSize" : 261120,
  "uploadDate" : ISODate("..."),
  "filename" : "test.pdf"
}
```

---

## Step 4: Download the file back to disk

In the terminal:

```bash
mongofiles --db personal_archive get test.pdf
```

It will save the file to your current directory.

---

## Step 5: Add custom metadata (optional)

GridFS allows you to embed metadata with each file. Example:

```bash
mongofiles --db personal_archive put ~/Documents/test.pdf --type application/pdf
```

Then update it via `mongosh`:

```js
db.fs.files.updateOne(
  { filename: "test.pdf" },
  { $set: { "metadata": { source: "personal", category: "OSINT", addedBy: "you" } } }
)
```

---

## Summary of What You Now Know

* Upload files to MongoDB using GridFS (`mongofiles`)
* View metadata in `fs.files`, binary chunks in `fs.chunks`
* Retrieve files from DB to disk
* Add structured metadata to enhance searchability

---

Let me know once you've successfully uploaded and retrieved a PDF, and I’ll guide you into **Phase 2: MongoDB CRUD & Querying Basics**, where we start building document-based data around your stored files.
