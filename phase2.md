# Phase 2: MongoDB CRUD & Querying Basics

This phase introduces how to manipulate and query data in MongoDB. You'll learn how to **c**reate (store), **r**etrieve, **u**pdate, and **d**elete documents in the database (**CRUD**). You'll also learn how to filter results, project specific fields, and sort data.

---

## Step 1: Insert & Update Documents

### Insert a Document

You can store data (e.g., metadata) in MongoDB using `insertOne()` for one document or `insertMany()` for multiple documents.

For example, if you want to insert a new document to store metadata for a file:

```js
db.file_metadata.insertOne({
  filename: "another_test.pdf",
  description: "Another test document",
  tags: ["pdf", "test", "osint"],
  addedAt: new Date(),
  reviewed: false
})
```

* **Explanation**:

  * `insertOne()` adds a single document into the `file_metadata` collection.
  * Each field (like `filename`, `description`, etc.) corresponds to a key-value pair in the document.

If you want to insert multiple documents, you can do:

```js
db.file_metadata.insertMany([
  { filename: "file1.pdf", description: "File 1", tags: ["pdf"], addedAt: new Date(), reviewed: false },
  { filename: "file2.pdf", description: "File 2", tags: ["pdf", "archive"], addedAt: new Date(), reviewed: true }
])
```

### Update a Document

To update an existing document, you use `updateOne()` or `updateMany()`.

For example, to mark a file as reviewed in your metadata:

```js
db.file_metadata.updateOne(
  { filename: "another_test.pdf" },  // Query condition (search for document with this filename)
  { $set: { reviewed: true } }        // Update action (set 'reviewed' field to true)
)
```

* **Explanation**:

  * `updateOne()` will update the first document that matches the condition (`filename: "another_test.pdf"`).
  * `$set` tells MongoDB to update or add a field (`reviewed: true`).

If you want to update multiple documents at once, use `updateMany()`:

```js
db.file_metadata.updateMany(
  { reviewed: false },                // Condition (find all documents where 'reviewed' is false)
  { $set: { reviewed: true } }         // Update action (set 'reviewed' to true for all matching docs)
)
```

---

## Step 2: Query Documents with Filters

You can find documents using `find()`, which accepts optional filters.

### Basic Querying

For example, to find all documents where `reviewed` is `true`:

```js
db.file_metadata.find({ reviewed: true }).pretty()
```

* **Explanation**:

  * `find()` retrieves all documents that match the given filter.
  * `.pretty()` formats the result to be more readable.

### Using Operators

MongoDB supports operators to perform more advanced queries.

For example, to find documents where `tags` contain `"pdf"` and `reviewed` is `false`, use `$in`:

```js
db.file_metadata.find({ tags: { $in: ["pdf"] }, reviewed: false }).pretty()
```

* **Explanation**:

  * `$in` finds documents where the `tags` array contains `"pdf"`.
  * `reviewed: false` ensures only documents with `reviewed` set to `false` are returned.

You can use other operators like `$gt`, `$lt`, `$ne`, etc.

---

## Step 3: Projection & Sorting

### Projection: Return Specific Fields

To only return specific fields (e.g., `filename` and `tags`), you use a projection.

```js
db.file_metadata.find({}, { filename: 1, tags: 1 }).pretty()
```

* **Explanation**:

  * The first `{}` is the filter (an empty filter means return all documents).
  * The second `{ filename: 1, tags: 1 }` specifies which fields to include (1 means include, 0 means exclude).

If you wanted to exclude the `description` field, you could use:

```js
db.file_metadata.find({}, { description: 0 }).pretty()
```

This will return all fields except `description`.

### Sorting Results

To sort the results by `addedAt` in descending order:

```js
db.file_metadata.find().sort({ addedAt: -1 }).pretty()
```

* **Explanation**:

  * `.sort({ addedAt: -1 })` sorts the results by `addedAt` in descending order (`1` for ascending, `-1` for descending).

---

## Step 4: Delete Documents

### Delete a Single Document

To delete a single document, use `deleteOne()`:

```js
db.file_metadata.deleteOne({ filename: "another_test.pdf" })
```

* **Explanation**:

  * This deletes the first document that matches the given filter (`filename: "another_test.pdf"`).

### Delete Multiple Documents

To delete multiple documents at once, use `deleteMany()`:

```js
db.file_metadata.deleteMany({ reviewed: false })
```

* **Explanation**:

  * This deletes all documents where the `reviewed` field is `false`.

---

## Summary

By completing this phase, you should now be comfortable with the following operations in MongoDB:

* Inserting and updating documents using `insertOne()`, `insertMany()`, `updateOne()`, and `updateMany()`.
* Querying documents using basic filters, operators (`$in`, `$gt`, etc.), and sorting.
* Projecting specific fields and excluding others in query results.
* Deleting documents using `deleteOne()` and `deleteMany()`.

---

## Next Step

🚀 [Phase 3](https://github.com/tims-computer-academy/path_adv_mongodb/blob/main/phase3.md)
