## MongoDB Schema
### Embedding vs. Referencing
#### 1. Embedding
```
{
    "first_name": "Paul",
    "surname": "Miller",
    "cell": "447557505611",
    "city": "London",
    "location": [45.123, 47.232],
    "profession": ["banking", "finance", "trader"],
    "cars": [
        {
            "model": "Bentley",
            "year": 1973
        },
        {
            "model": "Rolls Royce",
            "year": 1965
        }
    ]
}
```
##### Advantages
- You can retrieve all relevant information in a single query.
- Avoid implementing joins in application code or using $lookup
- Update related information as a single atomic operation. By default, all CRUD operations on a single document are ACID compliant
- However, if you need a transaction across multiple operations, you can use the 
transaction operator
- Though transactions are available starting 4.0, however, I should add that it's an anti-pattern to be overly reliant on using them in your application.
##### Limitations
- Large documents mean more overhead if most fields are not relevant. You can increase query performance by limiting the size of the documents that you are sending over the wire for each query.
- There is a 16-MB document size limit in MongoDB. If you are embedding too much data inside a single document, you could potentially hit this limit.

#### 2. Referencing
```
{
    "name": "left-handed smoke shifter",
    "manufacturer": "Acme Corp",
    "catalog_number": "1234",
    "parts": ["ObjectID('AAAA')", "ObjectID('BBBB')", "ObjectID('CCCC')"]
}
```
```
{
    "_id" : "ObjectID('AAAA')",
    "partno" : "123-aff-456",
    "name" : "#4 grommet",
    "qty": "94",
    "cost": "0.94",
    "price":" 3.99"
}
```
##### Advantages
- By splitting up data, you will have smaller documents.
- Less likely to reach 16-MB-per-document limit
- Infrequently accessed information not needed on every query.
- Reduce the amount of duplication of data. However, it's important to note that data duplication should not be avoided if it results in a better schema.
##### Limitations
- In order to retrieve all the data in the referenced documents, a minimum of two queries or $lookup required to retrieve all the information.

### Type of Relationships
#### 1. One-to-One
```
{
    "_id": "ObjectId('AAA')",
    "name": "Joe Karlsson",
    "company": "MongoDB",
    "twitter": "@JoeKarlsson1",
    "twitch": "joe_karlsson",
    "tiktok": "joekarlsson",
    "website": "joekarlsson.com"
}
```
We can model all one-to-one data as key-value pairs in our database.

#### 2. One-to-Few
```
{
    "_id": "ObjectId('AAA')",
    "name": "Joe Karlsson",
    "company": "MongoDB",
    "twitter": "@JoeKarlsson1",
    "twitch": "joe_karlsson",
    "tiktok": "joekarlsson",
    "website": "joekarlsson.com",
    "addresses": [
        { "street": "123 Sesame St", "city": "Anytown", "cc": "USA" },  
        { "street": "123 Avenue Q",  "city": "New York", "cc": "USA" }
    ]
}
```
We might need to store several addresses associated with a given user. It's unlikely that a user for our application would have more than a couple of different addresses. For relationships like this, we would define this as a one-to-few relationship.

#### 3. One-to-Many
Products:
```
{
    "name": "left-handed smoke shifter",
    "manufacturer": "Acme Corp",
    "catalog_number": "1234",
    "parts": ["ObjectID('AAAA')", "ObjectID('BBBB')", "ObjectID('CCCC')"]
}
```
Parts:
```
{
    "_id" : "ObjectID('AAAA')",
    "partno" : "123-aff-456",
    "name" : "#4 grommet",
    "qty": "94",
    "cost": "0.94",
    "price":" 3.99"
}
```
We might have a Products collection with data about each product in our e-commerce store, and in order to keep that part data linked, we can keep an array of Object IDs that link to a document that has information about the part. These parts can be saved in the same collection or in a separate collection, if needed.

#### 4. One-to-Squillions (Một - liên kết - Hàng triệu)
Hosts:
```
{
    "_id": ObjectID("AAAB"),
    "name": "goofy.example.com",
    "ipaddr": "127.66.66.66"
}
```
Log Message:
```
{
    "time": ISODate("2014-03-28T09:42:41.382Z"),
    "message": "cpu is on fire!",
    "host": ObjectID("AAAB")
}
```
Instead of tracking the relationship between the host and the log message in the host document, let's let each log message store the host that its message is associated with. By storing the data in the log, we no longer need to worry about an unbounded array messing with our application!

#### 5. Many-to-Many
Users:
```
{
    "_id": ObjectID("AAF1"),
    "name": "Kate Monster",
    "tasks": [ObjectID("ADF9"), ObjectID("AE02"), ObjectID("AE73")]
}
```
Tasks:
```
{
    "_id": ObjectID("ADF9"),
    "description": "Write blog post about MongoDB schema design",
    "due_date": ISODate("2014-04-01"),
    "owners": [ObjectID("AAF1"), ObjectID("BB3G")]
}
```
Let's imagine that we are building a to-do application. In our app, a user may have many tasks and a task may have many users assigned to it.
From this example, you can see that each user has a sub-array of linked tasks, and each task has a sub-array of owners for each item in our to-do app.

### Recap:
- `One-to-One` - Prefer key value pairs within the document
- `One-to-Few` - Prefer embedding
- `One-to-Many` - Prefer embedding
- `One-to-Squillions` - Prefer Referencing
- `Many-to-Many` - Prefer Referencing

### General Rules for MongoDB Schema Design:
- `Rule 1`: Favor embedding unless there is a compelling reason not to.
- `Rule 2`: Needing to access an object on its own is a compelling reason not to embed it.
- `Rule 3`: Avoid joins and lookups if possible, but don't be afraid if they can provide a better schema design.
- `Rule 4`: Arrays should not grow without bound. If there are more than a couple of hundred documents on the many side, don't embed them; if there are more than a few thousand documents on the many side, don't use an array of ObjectID references. High-cardinality arrays are a compelling reason not to embed.
- `Rule 5`: As always, with MongoDB, how you model your data depends entirely on your particular application's data access patterns. You want to structure your data to match the ways that your application queries and updates it.


