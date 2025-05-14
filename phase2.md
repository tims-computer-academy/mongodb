# Phase 2: MongoDB CRUD & Querying Basics

This phase teaches you how to **store structured information** (like text data, metadata, tags, dates) in MongoDB and how to search, filter, and modify it.

You'll learn the basics of **CRUD**:
**C**reate, **R**ead, **U**pdate, **D**elete — the fundamental operations in any database.

We’ll use a new collection in your `personal_archive` database to store **metadata documents** — think of these as text-based records to describe files, research notes, or anything else.

---

## What Is a "Document" in MongoDB?

In MongoDB, a **document** is a **JSON-like data record**. It's stored inside a **collection**, which is like a folder for related documents.

Example document (metadata for a PDF file):

```js
{
  filename: "report_2023.pdf",
  description: "Annual OSINT report",
  tags: ["report", "osint", "2023"],
  reviewed: false,
  addedAt: ISODate("2025-03-12T09:00:00Z")
}
```

This document is stored in a collection you choose (e.g. `file_metadata`). Unlike GridFS, we’re not storing the file itself here — only **structured info about the file**.

---

## Step 1: Insert Documents

### Insert a Single Document

Start the MongoDB shell if it’s not already running:

```bash
mongosh
```

Switch to your archive database:

```js
use personal_archive
```

Now insert a document into a new collection called `file_metadata`:

```js
db.file_metadata.insertOne({
  filename: "another_test.pdf",
  description: "Another test document",
  tags: ["pdf", "test", "osint"],
  addedAt: new Date(),
  reviewed: false
})
```

**What this does:**

* `db.file_metadata` refers to a collection called `file_metadata`.
* `insertOne(...)` creates a new document (record) inside that collection.
* You can add any fields you want: strings, arrays, booleans, dates, etc.

> In **Phase 1**, you learned to upload files using GridFS (`mongofiles`). In **Phase 2**, we're only working with structured metadata in a regular MongoDB collection (`file_metadata`). There’s no connection to an actual file on disk or in GridFS unless you choose to link them later.

> MongoDB collections are flexible — no need to define a schema up front.

### Insert Many Documents at Once

You can insert multiple documents using `insertMany()`:

```js
db.file_metadata.insertMany([
  {
    filename: "intelligence_briefing.pdf",
    tags: ["briefing", "intel"],
    reviewed: true,
    addedAt: new Date("2025-01-10")
  },
  {
    filename: "field_notes.txt",
    tags: ["notes", "text"],
    reviewed: false,
    addedAt: new Date("2025-01-12")
  }
])
```

This will add both documents in a single command.

---

## Step 2: Find and Filter Documents

### Show All Documents

To list everything in the `file_metadata` collection:

```js
db.file_metadata.find().pretty()
```

Use `.pretty()` to make the output easier to read.

### Find with a Filter

Find a single document where the filename is `"another_test.pdf"`:

```js
db.file_metadata.findOne({ filename: "another_test.pdf" })
```

Find documents that are not reviewed yet:

```js
db.file_metadata.find({ reviewed: false }).pretty()
```

Find documents that have the tag "intel":

```js
db.file_metadata.find({ tags: "intel" }).pretty()
```

> MongoDB automatically matches array values when you provide a single value (like `"intel"`).

---

## Step 3: Use Advanced Queries

### Match Documents with Operators

Find files added **after a certain date**:

```js
db.file_metadata.find({
  addedAt: { $gt: new Date("2024-01-01") }
}).pretty()
```

* `$gt` = "greater than"
* Other operators include `$lt` (less than), `$eq` (equal), `$ne` (not equal), `$in` (in array)

Example: Find files tagged with either `"intel"` or `"osint"`:

```js
db.file_metadata.find({
  tags: { $in: ["intel", "osint"] }
}).pretty()
```

---

## Step 4: Return Only Selected Fields (Projection)

Sometimes you only want to return specific fields (not the entire document).

Example: Show only filenames and tags:

```js
db.file_metadata.find(
  {},
  { filename: 1, tags: 1, _id: 0 }
)
```

Explanation:

* `{}` means "no filter" — return all documents
* `{ filename: 1, tags: 1 }` selects those fields
* `_id: 0` hides the default `_id` field (shown unless explicitly hidden)

---

## Step 5: Sort Results

You can sort documents using `.sort()`.

Example: Sort files by date (most recent first):

```js
db.file_metadata.find().sort({ addedAt: -1 }).pretty()
```

* `1` = ascending (oldest first)
* `-1` = descending (newest first)

You can also sort by multiple fields if needed.

---

## Step 6: Update Documents

### Update One Document

Mark a file as reviewed:

```js
db.file_metadata.updateOne(
  { filename: "another_test.pdf" },
  { $set: { reviewed: true } }
)
```

### Update Multiple Documents

Mark all unreviewed files as reviewed:

```js
db.file_metadata.updateMany(
  { reviewed: false },
  { $set: { reviewed: true } }
)
```

You can also add new fields at any time:

```js
db.file_metadata.updateOne(
  { filename: "field_notes.txt" },
  { $set: { category: "raw text" } }
)
```

MongoDB doesn’t require a predefined structure — you can add fields as needed.

---

## Step 7: Delete Documents

### Delete One Document

Remove a document by filename:

```js
db.file_metadata.deleteOne({ filename: "field_notes.txt" })
```

### Delete Many Documents

Remove all documents with a certain tag:

```js
db.file_metadata.deleteMany({ tags: "test" })
```

---

## Summary: What You Learned in Phase 2

You now know how to:

✅ Insert one or many structured documents into a MongoDB collection<br>
✅ Search documents with filters and operators<br>
✅ Return specific fields and sort the results<br>
✅ Update documents with `$set`<br>
✅ Delete documents when they’re no longer needed

This skillset is essential for working with **structured information** in MongoDB — metadata, notes, logs, records, and more.

---

## Next Step

🚀 [Phase 3](https://github.com/tims-computer-academy/path_adv_mongodb/blob/main/phase3.md)
