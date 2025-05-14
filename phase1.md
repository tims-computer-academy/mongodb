# Phase 1: Store & Retrieve Files with GridFS

This phase teaches you how to use MongoDB’s **GridFS** to store and retrieve binary files like PDFs, images, or videos.

---

## 💡 What Is GridFS?

GridFS stores files by breaking them into chunks:

| Collection  | Description                      |
| ----------- | -------------------------------- |
| `fs.files`  | File metadata (name, size, date) |
| `fs.chunks` | The binary file chunks           |

Files larger than 16MB are automatically split. But even small files can be stored to unify file + metadata.

---

## Step 1: Create the Database

In your terminal, run:

```bash
mongosh
```

Inside the shell, create a database named `personal_archive` with the following command (later you can create databases with other names):

```bash
use personal_archive
```

Confirm the current database:

```bash
db
```

Output should be:

```
personal_archive
```

---

## Step 2: Upload a File Using `mongofiles`

Exit the shell first:

```bash
exit
```

In your terminal, run the following:

```bash
mongofiles --db personal_archive put /full/path/to/your/document.pdf
```

(Replace `/full/path/to/your/document.pdf` with a real file on your system.)

This uploads the file into MongoDB under the `fs.files` and `fs.chunks` collections.

---

## Step 3: Verify Upload in `mongosh`

Reconnect to the shell:

```bash
mongosh
```

Switch to your DB again:

```bash
use personal_archive
```

Now list the files stored:

```bash
db.fs.files.find().pretty()
```

Output should look like this:

```json
{
  "_id" : ObjectId("..."),
  "length" : 12345,
  "chunkSize" : 261120,
  "uploadDate" : ISODate("..."),
  "filename" : "test.pdf"
}
```

You can also check how many chunks exist:

```bash
db.fs.chunks.countDocuments()
```

---

## Step 4: Download the File Back to Disk

Exit the shell again:

```bash
exit
```

Now restore the file from DB to your local machine:

```bash
mongofiles --db personal_archive get test.pdf
```

The file will be saved to your current working directory.

---

## Step 5: Add Custom Metadata (Optional)

Let’s attach metadata (e.g. category, source) to the uploaded file.

1. Reconnect to the shell:

```bash
mongosh
use personal_archive
```

2. Run this command to update the metadata (important: case sensitive!):

```bash
db.fs.files.updateOne(
  { filename: "test.pdf" },  // Replace with your real filename
  {
    $set: {
      metadata: {
        category: "osint",
        source: "local upload",
        reviewed: false,
        addedAt: new Date()
      }
    }
  }
)
```

The file now has searchable metadata inside the `metadata` field.

---

## Summary

By completing this phase, you now know how to:

* Upload binary files into MongoDB using `mongofiles`
* View stored files (`fs.files`) and chunks (`fs.chunks`)
* Download files from the database
* Add custom metadata to enhance organization and search

---

## Next Step

🚀 [Phase 2](https://github.com/tims-computer-academy/path_adv_mongodb/blob/main/phase2.md)
