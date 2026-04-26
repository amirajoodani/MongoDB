```markdown
# MongoDB Basics: A Command Guide

This guide covers essential MongoDB commands for installation, database operations, CRUD operations, indexing, aggregation, and Python integration.

---

## 📦 Installation & Setup

```bash
# Install MongoDB packages (after downloading .deb files)
sudo dpkg -i *.deb

# Check MongoDB service status
sudo systemctl status mongod

# Start MongoDB service
sudo systemctl start mongod

# Enable MongoDB to start on boot
sudo systemctl enable mongod

# Check mongosh version
mongosh --version
```

---

## 🚀 Starting MongoDB Shell

```bash
# Start MongoDB shell
mongosh
```

---

## 🗄️ Database Operations

```javascript
// Show all databases
show dbs

// Create or switch to a database (database is created when data is inserted)
use amir

// Show current active database
db

// Create a collection explicitly
db.createCollection("users")

// Show all collections in current database
show collections
```

---

## 📝 CRUD Operations (Create, Read, Update, Delete)

### Insert Documents

```javascript
// Insert a single document
db.users.insertOne({name: "amir", age: 31})

// Insert multiple documents
db.security.insertMany([
    {name: "amir", age: 31},
    {name: "jafar", age: 36}
])
```

### Query Documents

```javascript
// Find all documents in a collection
db.users.find()

// Find first document matching the filter
db.users.findOne({name: "amir"})

// Find all documents matching the filter
db.users.find({name: "amir"})

// Projection: Exclude a field (age will not be shown)
db.users.find({}, {age: 0})

// Projection: Include only a field (only age will be shown)
db.users.find({}, {age: 1})

// Count documents
db.users.find().count()

// Limit results to first 2 documents
db.users.find().limit(2)

// Get execution stats for a query
db.users.find().explain()

// Map to extract specific fields
db.users.find().map(function(input) {
    return input.age
})

// Sort ascending (min to max)
db.users.find().sort({age: 1})

// Sort descending (max to min)
db.users.find().sort({age: -1})
```

### Update Documents

```javascript
// Update a single document (add $set to update specific fields)
db.users.updateOne(
    {name: "jafar"},
    {$set: {age: 25}}
)
```

### Delete Documents

```javascript
// Delete a single document (first matching "amir")
db.security.deleteOne({name: "amir"})

// Delete all documents matching "amir"
db.security.deleteMany({name: "amir"})

// Delete all documents in a collection
db.security.remove({})

// Drop an entire collection
db.users.drop()
```

---

## 🔧 Capped Collections

Capped collections have fixed size and follow FIFO (First In, First Out) behavior.

```javascript
// Create a capped collection
// max: maximum number of documents (5)
// size: maximum size in bytes (20)
// capped: true enables FIFO behavior
db.createCollection("security", {
    max: 5,
    size: 20,
    capped: true
})

// Check if a collection is capped
db.security.isCapped()
```

**Behavior:** When the 6th document is inserted, the oldest document is automatically removed.

---

## 📊 Nested Documents

```javascript
// Insert a document with nested structure
db.security.insertOne({
    name: "amir",
    age: 25,
    contact: {
        id: 1,
        phone: 12345667,
        gmail: "amir@gmail.com"
    }
})
```

---

## 📁 Loading External JavaScript Files

```javascript
// Load and execute a JavaScript file in mongosh
load("/home/amir/it.js")
```

---

## 🔄 Aggregation Pipeline

Aggregation allows complex data processing and transformation.

### Basic Aggregation

```javascript
// Match (filter) documents where name is "amir"
db.users.aggregate([
    {$match: {name: "amir"}}
])

// Group by name and count total documents per group
db.users.aggregate([
    {$group: {_id: "$name", total: {$sum: 1}}}
])

// Match and then sort
db.users.aggregate([
    {$match: {name: "amir"}},
    {$sort: {age: 1}}
])

// Match where age is greater than 25
db.users.aggregate([
    {$match: {age: {$gt: 25}}}
])

// Unset/remove a field from all documents
db.users.aggregate([
    {$unset: "name"}
])
```

### Aggregation Operators Reference

| Operator | Purpose |
|----------|---------|
| `$match` | Filter documents |
| `$group` | Group documents by a key |
| `$sort` | Sort documents |
| `$unset` | Remove a field |
| `$gt` | Greater than |

---

## 🐍 Python Integration with PyMongo

### Install PyMongo

```bash
# Install pip for Python 3
apt install python3-pip

# Install PyMongo package
python3 -m pip install pymongo
```

### Insert Document from Python

Create `mongo.py`:

```python
import pymongo

# Connect to MongoDB (default: localhost:27017)
client = pymongo.MongoClient()

# Select or create database 'amir'
db = client.amir

# Select collection 'users'
collection = db.users

# Document to insert
d1 = {"name": "moein", "age": 21}

# Insert the document
collection.insert_one(d1)
```

Run the script:

```bash
python3 mongo.py
```

### Read Documents from Python

Create `mongolist.py`:

```python
import pymongo

client = pymongo.MongoClient()
db = client.amir
collection = db.users

# Retrieve all documents from users collection
data = collection.find()

# Convert cursor to list and print
print(list(data))
```

Run the script:

```bash
python3 mongolist.py
```

---

## 📋 Quick Reference Table

| Operation | Command |
|-----------|---------|
| Show databases | `show dbs` |
| Switch/create DB | `use dbname` |
| Show current DB | `db` |
| Create collection | `db.createCollection("name")` |
| Insert one | `db.collection.insertOne({...})` |
| Insert many | `db.collection.insertMany([...])` |
| Find all | `db.collection.find()` |
| Find with filter | `db.collection.find({key: value})` |
| Update one | `db.collection.updateOne({filter}, {$set: {field: value}})` |
| Delete one | `db.collection.deleteOne({filter})` |
| Delete many | `db.collection.deleteMany({filter})` |
| Drop collection | `db.collection.drop()` |
| Count documents | `db.collection.find().count()` |
| Limit results | `db.collection.find().limit(n)` |
| Sort ascending | `db.collection.find().sort({field: 1})` |
| Sort descending | `db.collection.find().sort({field: -1})` |

---

## 💡 Pro Tips

1. **Databases are created lazily** - `use amir` doesn't create the database until you insert data
2. **`_id` field** - MongoDB automatically adds a unique `_id` field to each document
3. **Capped collections** - Great for logs, caches, or real-time data streaming
4. **Aggregation** - More powerful than `find()` for complex queries and data transformation
5. **Indexes** - Use `createIndex()` to improve query performance (not covered in basics)

---

Happy coding with MongoDB! 🍃
```

You can save this content as `mongodb_guide.md`. Would you like me to add more examples for indexes, backups, or replica sets?
