## MongoDB Operations (CLI - Mongo Shell)
### 1. Databases
Show databases (Only show non-empty databases)
```
show dbs
```
Use database
```
use <database name>
```
Create database
```
use <non exist database name>
```
EX:
```
use school
```
Create collection (collection is like a table)
```
db.createCollection("students")
```
Drop (Delete) current database
```
db.dropDatabase()
```

### 2. Insert Documents
Insert One
```
db.students.insertOne({name:"Sarah", age:25, gpa:3.5})
```
Insert Many
```
db.students.insertMany([
{name:"Sarah", age:25, gpa:3.5}, 
{name:"Sandy", age:23, gpa:3.8}, 
{name:"Bob", age:37, gpa:2.5}
])
```
### 3. Data Types
String
```
name: "Larry"
```
Integer
```
age: 28
```
Double
```
gpa: 2.9
```
Boolean
```
fullTime: true
```
```
fullTime: false
```
Date
```
registerDate: new Date()
```
```
registerDate: new Date("2024-06-02T00:00:00")
```
Null
```
graduationDate: null
```
Array
```
courses: ["Biology", "Chemistry", "Math"]
```
Nested Document
```
address: {street: "123 Fake St.",
          city: "Valentine",
          zip: 50000}
```


### 4. Sorting & Limiting
Sorting Ascending
```
db.students.find().sort(name:1)
```
Sorting Descending
```
db.students.find().sort(name:-1)
```
Limiting
```
db.students.find().limit(3)
```
Sorting & Limiting
```
db.students.find().sort(name:-1).limit(2)
```

### 5. Find
```
.find({query}, {projection})
```
#### Find without Projection
```
db.students.find({name:"Sandy"})
```
```
db.students.find({gpa:4.0, fullTime:true})
```
```
db.students.find({gpa:4.0, fullTime:true})
```
#### Find with Projection (If not specify query parameters, return all documents)
Get all documents with only the _id field and name field
```
db.students.find({}, {name:true})
```
Get all documents with only the name field
```
db.students.find({}, {_id:false, name:true})
```
Get all documents with only the name field and gpa field
```
db.students.find({}, {_id:false, name:true, gpa: true})
```

### 6. Update
```
.update(filter, update)
```
#### Update One
##### $set operator
This operations may update more than one documents because we may have multiple students name "Sarah"
```
db.students.updateOne({name:"Sarah"}, {$set:{fullTime:true}})
```
This operations will update only one document because we filter by the _id instead the name
```
db.students.updateOne({_id:ObjectId("af3473asdas42314a"}, {$set:{fullTime:true}})
```
##### $unset operator
Unset will remove the field of the document
```
db.students.updateOne({_id:ObjectId("af3473asdas42314a"}, {$unset:{fullTime:""}})
```
#### Update Many
Update all documents
```
db.students.updateMany({}, {$set:{fullTime:true}})
```
Update all the documents that not have the fullTime field, set the fullTime field to true
```
db.students.updateMany({fullTime:{$exists:false}}, {$set:{fullTime:true}})
```

### 7. Delete
Delete One
```
db.students.deleteOne({name:"Sarah"})
```
Delete Many
```
db.students.deleteMany({fullTime:true})
```
Delete Many (filter by documents that not have registerDate field)
```
db.students.deleteMany({registerDate:{$exists:false}})
```

### 8. Comparison Operators
#### Less than: $lt
```
db.students.find({age:{$lt:30}})
```
#### Less than or equal: $lte
```
db.students.find({age:{$lte:30}})
```
#### Greater than: $gt
```
db.students.find({age:{$gt:20}})
```
#### Greater than or equal: $gte
```
db.students.find({age:{$gte:20}})
```
#### Combine
```
db.students.find({age:{$gte:20, $lte:38}})
```
#### In: $in
```
db.students.find({name:{$in:["Sarah", "Sandy", "Bob"]}})
```
#### Not In: $nin
```
db.students.find({name:{$nin:["Sarah", "Sandy", "Bob"]}})
```

### 9. Logical Operators
#### $and
```
db.students.find({$and: [{age:{$gte:20}}, {fullTime:true}]})
```
#### $or
```
db.students.find({$or: [{age:{$gte:20}}, {fullTime:true}]})
```
#### $nor
```
db.students.find({$nor: [{age:{$gte:20}}, {fullTime:true}]})
```
#### $not
This is difference from $lt because $not return the documents that have null value too
```
db.students.find({$age:{$not:{$gte:30}}})
```

### 10. Indexes
Create Index (Ascending Order)
```
db.students.createIndex({name: 1})   -->   name_1
```
Get Indexes
```
db.students.getIndexes()
```
Drop Index
```
db.students.dropIndex("name_1")
```

### 11. Collections
Show Collections
```
show collections
```
Create Collection (collection name is teachers, limited size is 10Mb, max documents/teacher is 100 and autoIndexId is false - this is set to true be default)
```
db.createCollection("teachers", {capped:true, size:10000000, max:100}, {autoIndexId:false})
```


