# MongoDB Cheat Sheet

## MongoDB Servers and Shells

### mongod (MongoDB Server)

| Operation | Command | Description |
|-----------|---------|-------------|
| Start MongoDB server | `mongod` | Start MongoDB server with default settings |
| Start with config file | `mongod --config /path/to/mongod.conf` | Start using a configuration file |
| Specify data directory | `mongod --dbpath /data/db` | Start with custom data directory |
| Specify port | `mongod --port 27018` | Start MongoDB on custom port |
| Enable authentication | `mongod --auth` | Start with authentication enabled |
| Limit connections | `mongod --maxConns 100` | Limit number of connections |
| Start as replica set | `mongod --replSet "rs0"` | Start as a replica set member |
| Enable logging | `mongod --logpath /var/log/mongodb/mongod.log` | Log to a file |
| Enable verbose logging | `mongod --logpath /var/log/mongodb/mongod.log --verbose` | More detailed logging |
| Start as a Windows service | `mongod --install --serviceName "MongoDB"` | Install as Windows service |
| Enable sharding | `mongod --shardsvr` | Start as a shard |
| Start config server | `mongod --configsvr` | Start as a config server |
| Repair database | `mongod --repair` | Repair database files |
| Check MongoDB version | `mongod --version` | Display server version |

### mongosh (MongoDB Shell)

| Operation | Command | Description |
|-----------|---------|-------------|
| Launch mongosh | `mongosh` | Start the MongoDB Shell |
| Connect with URI | `mongosh "mongodb://user:pwd@host:port/db"` | Connect using connection string |
| Run script file | `mongosh <script.js>` | Execute JavaScript file in mongosh |
| Execute command | `mongosh --eval "db.stats()"` | Run a command without entering shell |
| Enable colorization | `mongosh --norc` | Start mongosh without loading ~/.mongoshrc.js |
| Connect with options | `mongosh --host <host> --port <port> --username <user>` | Connect with specific options |

#### mongosh Helpers

| Helper | Command | Description |
|--------|---------|-------------|
| List databases | `show dbs` | Display available databases |
| List collections | `show collections` | Display collections in current database |
| Show users | `show users` | Display users in current database |
| Show roles | `show roles` | Display roles in current database |
| Show profile | `show profile` | Display system.profile information |
| Show logs | `show logs` | Display available logs |
| Clear screen | `cls` | Clear the terminal screen |
| Help | `help` | Display help information |
| Exit | `exit` or `.exit` | Exit mongosh |

#### mongosh Editor Commands

| Command | Description |
|---------|-------------|
| `edit` | Open editor for multi-line input |
| `.editor` | Enter multi-line editor mode |
| Ctrl+C | Exit editor mode |

#### mongosh Utility Methods

| Method | Description |
|--------|-------------|
| `use('database')` | Switch database (alternative to `use database`) |
| `show('databases')` | Show databases (alternative to `show dbs`) |
| `db.getMongo()` | Get the current server connection |
| `db.getName()` | Get the current database name |
| `db.hostInfo()` | Get host information |
| `db.serverStatus()` | Get server status |
| `db.hello()` | Get replica set status and server information |
| `db.enableFreeMonitoring()` | Enable free monitoring |
| `db.disableFreeMonitoring()` | Disable free monitoring |
| `db.getFreeMonitoringStatus()` | Check free monitoring status |
| `db.serverBuildInfo()` | Get server build information |
| `db.serverCmdLineOpts()` | Get command line options used to start server |
| `db.setLogLevel(1, "query")` | Set specific logging level |
| `db.getLogComponents()` | Get current log components configuration |
| `load("file.js")` | Load and execute a JavaScript file |

#### mongosh CRUD Shortcuts

| Operation | Command | Description |
|-----------|---------|-------------|
| Insert | `db.collection.insert({...})` | Insert a document |
| Find | `db.collection.find({...})` | Find documents |
| Pretty print | `db.collection.find().pretty()` | Format output |
| Update | `db.collection.update({...}, {...})` | Update documents |
| Remove | `db.collection.remove({...})` | Remove documents |

#### mongosh Keyboard Shortcuts

| Shortcut | Description |
|----------|-------------|
| Up/Down Arrow | Navigate through command history |
| Ctrl+A | Move cursor to beginning of line |
| Ctrl+E | Move cursor to end of line |
| Ctrl+K | Delete from cursor to end of line |
| Ctrl+U | Delete from cursor to beginning of line |
| Ctrl+L | Clear screen |
| Tab | Auto-complete commands and field names |
| Ctrl+R | Search command history |

#### mongosh Configuration

| Operation | Command | Description |
|-----------|---------|-------------|
| Configure prompt | `config.set("prompt", "myDB> ")` | Change the mongosh prompt |
| Set editor | `config.set("editor", "vim")` | Set preferred editor |
| Show config | `config.get()` | Show current configuration |
| Set display batch size | `config.set("displayBatchSize", 50)` | Set number of documents displayed |
| Enable history | `config.set("historyLength", 1000)` | Set history length |
| Custom prompt function | `config.set("prompt", function() { return `${db.getName()}> `; })` | Set dynamic prompt |

## Connection

| Operation | Command | Description |
|-----------|---------|-------------|
| Connect to a local MongoDB | `mongosh` | Connect to MongoDB running on localhost:27017 |
| Connect to a specific host/port | `mongosh --host <hostname> --port <port>` | Connect to MongoDB at specified host and port |
| Connect with authentication | `mongosh --username <user> --password <pwd> --authenticationDatabase <db>` | Connect with credentials |
| Connect to a MongoDB URI | `mongosh "mongodb://user:password@hostname:port/database"` | Connect using a connection string |
| Connect with TLS/SSL | `mongosh --tls --tlsCertificateKeyFile <file>` | Connect using TLS/SSL |
| Connect with AWS IAM | `mongosh "mongodb+srv://<cluster>" --awsIamSessionToken` | Connect using AWS IAM authentication |
| Exit the shell | `exit` or `quit()` | Close MongoDB connection |

## Database Operations

| Operation | Command | Description |
|-----------|---------|-------------|
| Show all databases | `show dbs` | List all databases |
| Create/switch database | `use <database>` | Create or switch to specified database |
| Show current database | `db` | Display current database name |
| Drop database | `db.dropDatabase()` | Delete current database |
| Show database stats | `db.stats()` | Display database statistics |
| Check database version | `db.version()` | Show MongoDB server version |

## Collection Operations

| Operation | Command | Description |
|-----------|---------|-------------|
| Show all collections | `show collections` | List all collections in current database |
| Create collection | `db.createCollection("<name>")` | Create a new collection |
| Create capped collection | `db.createCollection("<name>", {capped: true, size: 10000})` | Create size-limited collection |
| Drop collection | `db.<collection>.drop()` | Delete a collection |
| Rename collection | `db.<collection>.renameCollection("<new_name>")` | Rename a collection |
| Show collection stats | `db.<collection>.stats()` | Display collection statistics |

## CRUD Operations

### Create

| Operation | Command | Description |
|-----------|---------|-------------|
| Insert one document | `db.<collection>.insertOne({field: value})` | Insert a single document |
| Insert multiple documents | `db.<collection>.insertMany([{field1: value1}, {field2: value2}])` | Insert multiple documents |
| Save document | `db.<collection>.save({_id: ObjectId(), field: value})` | Insert or update document (legacy) |

### Read

| Operation | Command | Description |
|-----------|---------|-------------|
| Find all documents | `db.<collection>.find()` | Retrieve all documents |
| Find with criteria | `db.<collection>.find({field: value})` | Find documents matching criteria |
| Find one document | `db.<collection>.findOne({field: value})` | Find first matching document |
| Limit results | `db.<collection>.find().limit(5)` | Limit number of results |
| Skip results | `db.<collection>.find().skip(5)` | Skip first N results |
| Sort results | `db.<collection>.find().sort({field: 1})` | Sort in ascending (1) or descending (-1) order |
| Count documents | `db.<collection>.countDocuments({field: value})` | Count matching documents |
| Format results | `db.<collection>.find().pretty()` | Format output for readability |

### Update

| Operation | Command | Description |
|-----------|---------|-------------|
| Update one document | `db.<collection>.updateOne({query}, {$set: {field: value}})` | Update first matching document |
| Update many documents | `db.<collection>.updateMany({query}, {$set: {field: value}})` | Update all matching documents |
| Replace document | `db.<collection>.replaceOne({query}, {newDocument})` | Replace entire document |
| Upsert | `db.<collection>.updateOne({query}, {$set: {field: value}}, {upsert: true})` | Update if exists, insert if not |

### Delete

| Operation | Command | Description |
|-----------|---------|-------------|
| Delete one document | `db.<collection>.deleteOne({field: value})` | Delete first matching document |
| Delete many documents | `db.<collection>.deleteMany({field: value})` | Delete all matching documents |
| Delete all documents | `db.<collection>.deleteMany({})` | Remove all documents |

## Query Operators

| Category | Operator | Example | Description |
|----------|----------|---------|-------------|
| Comparison | `$eq` | `{field: {$eq: value}}` | Equals |
| Comparison | `$ne` | `{field: {$ne: value}}` | Not equals |
| Comparison | `$gt` | `{field: {$gt: value}}` | Greater than |
| Comparison | `$gte` | `{field: {$gte: value}}` | Greater than or equal |
| Comparison | `$lt` | `{field: {$lt: value}}` | Less than |
| Comparison | `$lte` | `{field: {$lte: value}}` | Less than or equal |
| Comparison | `$in` | `{field: {$in: [value1, value2]}}` | In array |
| Comparison | `$nin` | `{field: {$nin: [value1, value2]}}` | Not in array |
| Logical | `$and` | `{$and: [{field1: value1}, {field2: value2}]}` | AND condition |
| Logical | `$or` | `{$or: [{field1: value1}, {field2: value2}]}` | OR condition |
| Logical | `$not` | `{field: {$not: {$eq: value}}}` | NOT condition |
| Logical | `$nor` | `{$nor: [{field1: value1}, {field2: value2}]}` | NOR condition |
| Element | `$exists` | `{field: {$exists: true}}` | Field exists |
| Element | `$type` | `{field: {$type: "string"}}` | Field is of specified type |
| Array | `$all` | `{field: {$all: [value1, value2]}}` | Array contains all values |
| Array | `$elemMatch` | `{field: {$elemMatch: {subfield: value}}}` | Element in array matches |
| Array | `$size` | `{field: {$size: 3}}` | Array is of specified size |

## Update Operators

| Category | Operator | Example | Description |
|----------|----------|---------|-------------|
| Fields | `$set` | `{$set: {field: value}}` | Set field value |
| Fields | `$unset` | `{$unset: {field: ""}}` | Remove field |
| Fields | `$rename` | `{$rename: {"old": "new"}}` | Rename field |
| Fields | `$inc` | `{$inc: {field: 5}}` | Increment field value |
| Fields | `$mul` | `{$mul: {field: 2}}` | Multiply field value |
| Fields | `$min` | `{$min: {field: value}}` | Update if value is less than current |
| Fields | `$max` | `{$max: {field: value}}` | Update if value is greater than current |
| Fields | `$currentDate` | `{$currentDate: {field: true}}` | Set field to current date |
| Array | `$push` | `{$push: {field: value}}` | Add element to array |
| Array | `$pop` | `{$pop: {field: 1}}` | Remove last (1) or first (-1) element |
| Array | `$pull` | `{$pull: {field: value}}` | Remove matching elements from array |
| Array | `$pullAll` | `{$pullAll: {field: [value1, value2]}}` | Remove all matching values |
| Array | `$addToSet` | `{$addToSet: {field: value}}` | Add to array if not exists |

## Aggregation

| Stage | Example | Description |
|-------|---------|-------------|
| `$match` | `{$match: {field: value}}` | Filter documents |
| `$group` | `{$group: {_id: "$field", total: {$sum: "$amount"}}}` | Group by field |
| `$sort` | `{$sort: {field: 1}}` | Sort results |
| `$limit` | `{$limit: 5}` | Limit results |
| `$skip` | `{$skip: 10}` | Skip results |
| `$project` | `{$project: {field: 1, _id: 0}}` | Include/exclude fields |
| `$unwind` | `{$unwind: "$arrayField"}` | Deconstruct array field |
| `$lookup` | `{$lookup: {from: "collection", localField: "field", foreignField: "_id", as: "joined"}}` | Join with another collection |
| `$count` | `{$count: "totalCount"}` | Count documents |
| `$sample` | `{$sample: {size: 5}}` | Random sample |
| Pipeline | `db.<collection>.aggregate([{$match: {status: "active"}}, {$group: {_id: "$category", count: {$sum: 1}}}])` | Example pipeline |

## Indexing

| Operation | Command | Description |
|-----------|---------|-------------|
| Create index | `db.<collection>.createIndex({field: 1})` | Create ascending (1) or descending (-1) index |
| Create compound index | `db.<collection>.createIndex({field1: 1, field2: -1})` | Create multiple field index |
| Create unique index | `db.<collection>.createIndex({field: 1}, {unique: true})` | Create index with unique constraint |
| Create TTL index | `db.<collection>.createIndex({createdAt: 1}, {expireAfterSeconds: 3600})` | Create index that expires documents |
| Create text index | `db.<collection>.createIndex({field: "text"})` | Create full-text search index |
| Create geospatial index | `db.<collection>.createIndex({location: "2dsphere"})` | Create index for geo queries |
| List indexes | `db.<collection>.getIndexes()` | Show all indexes |
| Drop index | `db.<collection>.dropIndex("index_name")` | Delete specific index |
| Drop all indexes | `db.<collection>.dropIndexes()` | Delete all indexes |

## User Management

| Operation | Command | Description |
|-----------|---------|-------------|
| Create user | `db.createUser({user: "name", pwd: "password", roles: ["readWrite"]})` | Create new user |
| Update user | `db.updateUser("name", {roles: ["dbAdmin"]})` | Update user roles |
| Change password | `db.changeUserPassword("name", "newPassword")` | Change user password |
| Drop user | `db.dropUser("name")` | Delete user |
| Show users | `db.getUsers()` | List all users |
| Current user | `db.runCommand({connectionStatus: 1})` | Show current user info |

## Backup and Restore

| Operation | Command | Description |
|-----------|---------|-------------|
| Export data | `mongoexport --db=<db> --collection=<coll> --out=<file.json>` | Export collection to JSON |
| Import data | `mongoimport --db=<db> --collection=<coll> --file=<file.json>` | Import JSON to collection |
| Export database | `mongodump --db=<db> --out=<directory>` | Backup entire database |
| Import database | `mongorestore --db=<db> <directory>` | Restore database backup |

## Performance and Monitoring

| Operation | Command | Description |
|-----------|---------|-------------|
| Server status | `db.serverStatus()` | View server information |
| Current operations | `db.currentOp()` | List current operations |
| Kill operation | `db.killOp(<op_id>)` | Terminate operation |
| Connection info | `db.hostInfo()` | Display host information |
| Monitor replica set | `rs.status()` | Show replica set status |
| Check sharding status | `sh.status()` | Display sharding information |
| Print elapsed time | `db.setProfilingLevel(1, 100)` | Log operations taking > 100ms |
| Get profiling status | `db.getProfilingStatus()` | Check profiling configuration |
| View profiler data | `db.system.profile.find().pretty()` | See logged operations |
| Explain query plan | `db.collection.find().explain("executionStats")` | Show query execution details |

## MongoDB Compass

MongoDB Compass is the GUI for MongoDB. Here are some useful mongosh commands related to MongoDB Compass:

| Operation | Command | Description |
|-----------|---------|-------------|
| Generate connection string | `db.getMongo().getURI()` | Get URI for Compass connection |
| Export current query | `JSON.stringify(db.collection.find({...}).toArray())` | Export query results as JSON |
| Get collection validation rules | `db.getCollectionInfos({name: "collection"})` | View collection validation rules |
| View collection schema | `db.collection.aggregate([{$sample: {size: 100}}, {$project: {_id: 0}}])` | Sample documents for schema analysis |
