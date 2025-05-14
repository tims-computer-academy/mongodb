# Phase 2: MongoDB CRUD & Querying Basics

**Goal**: Learn how to manage structured data in collections using MongoDB's document-based model.

You’ll use these skills to:

* Store and query metadata about your files (e.g., PDF source, tags, status)
* Structure personal project data (e.g., OSINT results, logs, journal entries)

---

## Step 1: Create a collection and insert a document

Connect to your test database:

```bash
mongosh
use personal_archive
```

Insert a document describing a file:

```js
db.file_metadata.insertOne({
  filename: "test.pdf",
  description: "Test document for GridFS upload",
  tags: ["test", "pdf", "archive"],
  addedAt: new Date(),
  reviewed: false
})
```

Verify it:

```js
db.file_metadata.find().pretty()
```

---

## Step 2: Insert multiple documents

```js
db.file_metadata.insertMany([
  {
    filename: "report_2023.pdf",
    description: "Annual report",
    tags: ["finance", "pdf"],
    reviewed: true,
    addedAt: new Date("2024-01-01")
  },
  {
    filename: "leak_dump.zip",
    description: "OSINT leak data",
    tags: ["osint", "zip", "confidential"],
    reviewed: false,
    addedAt: new Date("2025-04-01")
  }
])
```

---

## Step 3: Query documents with filters

Basic filters:

```js
db.file_metadata.find({ reviewed: false })
```

Filter by tag:

```js
db.file_metadata.find({ tags: "osint" })
```

Advanced filter:

```js
db.file_metadata.find({
  reviewed: false,
  tags: { $in: ["confidential", "pdf"] }
})
```

---

## Step 4: Project specific fields (select columns)

```js
db.file_metadata.find(
  { reviewed: false },
  { filename: 1, tags: 1, _id: 0 }
)
```

---

## Step 5: Sort and limit results

Sort by most recent first:

```js
db.file_metadata.find().sort({ addedAt: -1 }).limit(2)
```

---

## Step 6: Update documents

Mark a file as reviewed:

```js
db.file_metadata.updateOne(
  { filename: "leak_dump.zip" },
  { $set: { reviewed: true } }
)
```

---

## Step 7: Delete documents

```js
db.file_metadata.deleteOne({ filename: "test.pdf" })
```

---

## Step 8: Search by text

Create a text index on description and tags:

```js
db.file_metadata.createIndex({ description: "text", tags: "text" })
```

Search:

```js
db.file_metadata.find({ $text: { $search: "osint" } })
```

---

## Summary of What You Now Know

* Insert one or many documents
* Query documents with simple and complex filters
* Project specific fields, sort, and paginate results
* Update and delete records
* Perform text searches

---

## Next Step

🚀 [Phase 3](https://github.com/tims-computer-academy/path_adv_mongodb/blob/main/phase3.md)
