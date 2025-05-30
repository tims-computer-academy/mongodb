# Phase 1: Store and Retrieve Files with GridFS

This phase teaches you how to store and retrieve **binary files** (like PDFs, images, videos) in MongoDB using **GridFS**.

You’ll also learn how to add **metadata** (e.g. tags, descriptions) to make those files easier to organize and search later.

---

## What Is GridFS?

Normally, MongoDB documents are limited to 16MB. To handle **large files**, MongoDB offers **GridFS**, a special system for storing and managing files in **chunks** (pieces).

When you use GridFS, MongoDB creates two special collections inside your database:

| Collection  | Purpose                                     |
| ----------- | ------------------------------------------- |
| `fs.files`  | Stores file metadata (filename, size, date) |
| `fs.chunks` | Stores actual binary data in small chunks   |

Even small files can be stored this way, so your app can treat all files the same.

Below is a simple **visual diagram of GridFS**. It shows how a file flows through GridFS: how it's split into chunks and how metadata and data are stored in two linked collections.

```mermaid
graph TD
    A[Original File] --> B(Split into 3 Chunks)
    B --> C{GridFS Storage}
    C --> D[fs.chunks Collection]
    C --> E[fs.files Collection]

    D --> D1[Chunk 0]
    D --> D2[Chunk 1]
    D --> D3[Chunk 2]

    D1 --> S1["{
        _id: ObjectId,
        files_id: ObjectId,
        n: 0,
        data: BinData
    }"]
    D2 --> S2["{
        _id: ObjectId,
        files_id: ObjectId,
        n: 1,
        data: BinData
    }"]
    D3 --> S3["{
        _id: ObjectId,
        files_id: ObjectId,
        n: 2,
        data: BinData
    }"]

    E -->|Stores metadata| H["{
        _id: ObjectId,
        filename: String,
        length: Number,
        chunkSize: Number,
        uploadDate: Date,
        metadata: Object,
        md5: String
    }"]

    style A fill:#d4f1f9,stroke:#333
    style D fill:#ffdddd,stroke:#333
    style D1 fill:#ffe6e6,stroke:#333
    style D2 fill:#ffe6e6,stroke:#333
    style D3 fill:#ffe6e6,stroke:#333
    style E fill:#ddffdd,stroke:#333
```

---

## Step 1: Create a New Database

You will first create a new MongoDB database called `personal_archive`.

1. Open your terminal and start the MongoDB shell:

   ```bash
   mongosh
   ```

2. In the shell, create (or switch to) your new database:

   ```js
   use personal_archive
   ```

   MongoDB will create this database the first time you store data in it.

3. Confirm which database you're using:

   ```js
   db
   ```

   It should return:

   ```
   personal_archive
   ```

---

## Step 2: Upload a File into GridFS

Now you'll upload a file (like a PDF) into the database.

> **Important:** This does **not** use the MongoDB shell. You’ll use a command-line tool called `mongofiles` that comes with MongoDB.

1. First, **exit the shell**:

   ```bash
   exit
   ```

2. Then use this command to upload a file:

   ```bash
   mongofiles --db personal_archive put /full/path/to/your/document.pdf
   ```

   Replace `/full/path/to/your/document.pdf` with the actual path to a file on your system.

### What this command does:

* `mongofiles`: The tool used to interact with GridFS from the terminal.
* `--db personal_archive`: Tells it which database to use.
* `put`: Uploads the file into GridFS.
* The file gets split into chunks and stored across the `fs.files` and `fs.chunks` collections.

---

## Step 3: Confirm the File Was Uploaded

Now you'll return to the MongoDB shell to inspect what was stored.

1. Start the shell again:

   ```bash
   mongosh
   ```

2. Switch back to your database:

   ```js
   use personal_archive
   ```

3. View the list of uploaded files:

   ```js
   db.fs.files.find().pretty()
   ```

   You should see output like this:

   ```js
   {
     _id: ObjectId("..."),
     length: 12345,
     chunkSize: 261120,
     uploadDate: ISODate("..."),
     filename: "document.pdf"
   }
   ```

4. To check how many chunks the file was split into:

   ```js
   db.fs.chunks.countDocuments()
   ```

---

## Step 4: Download the File Back to Your Computer

You can also retrieve the file from GridFS and save it back to disk.

1. Exit the MongoDB shell:

   ```bash
   exit
   ```

2. Use this command to download the file:

   ```bash
   mongofiles --db personal_archive get document.pdf
   ```

   Replace `document.pdf` with the filename you uploaded earlier.

   The file will be saved to your current terminal directory.

---

## Step 5: Add Metadata to the File (Optional)

You can add extra information (metadata) to a file stored in GridFS. This is useful for searching, tagging, or categorizing files.

> MongoDB allows you to update the `fs.files` document for the file and attach your own fields.

1. Go back into the MongoDB shell:

   ```bash
   mongosh
   use personal_archive
   ```

2. Update the document for your file:

   ```js
   db.fs.files.updateOne(
     { filename: "document.pdf" },  // Change to your filename
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

3. To confirm the metadata was added:

   ```js
   db.fs.files.findOne({ filename: "document.pdf" })
   ```

   This should now show a new `metadata` field with your custom values.

---

## Summary: What You Learned in Phase 1

You now know how to:

✅ Start a new MongoDB database (`personal_archive`)<br>
✅ Use `mongofiles` to upload binary files (like PDFs) into GridFS<br>
✅ Inspect uploaded files and their chunks<br>
✅ Download a stored file from the database back to disk<br>
✅ Add custom metadata to describe your files<br>

This prepares you to store, organize, and retrieve large or small files inside MongoDB — and sets the foundation for working with structured metadata in the next phases.

## Next Step

🚀 [Phase 2: MongoDB CRUD & querying basics](https://github.com/tims-computer-academy/mongodb/blob/main/phase2.md)
