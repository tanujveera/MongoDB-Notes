# MongoDB
--------
>A record in MongoDB is a document, which is a data structure composed of field and value pairs. MongoDB documents are similar to JSON objects. The values of fields may include other documents, arrays, and arrays of documents.

>MongoDB stores data records as documents (specifically BSON documents) which are gathered together in collections. A database stores one or more collections of documents.

**NOTE:**
>	  A database is a group of collections

>	  A collection is a group of Documents

>	  A document is a BSON object

# Installation:
------------
Install MongoDB Community Edition
Follow this link and search above heading
https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-windows/

## Ways to access MongoDB:
----------------------
1) Using MongoDB Compass (easiest as it is a GUI application)
2) Using MongoDB extension in VSCoded

## Steps to connect to MongoDB via VSCode:
---------------------------------------
>Go to MongoDB Compass app and start a new connection in localhost

>After creating a connection, you find the green bar saying "Local host"

>Click options tab and copy connection string

>After installing MongoDB extension in VSCode

>You can find a leaf symbol in left vertical tab

>Add connection -> add the copied connection string.

>A terminal will be started

# Commands:
---------
>Open MongoCompass and create a DB
Copy Connection String: mongodb://localhost:27017
## This command will connect to DB

```sh 
mongosh mongodb://localhost:27017 
```

## We use db command to get all the databases

```sh 
db
```

## Show databases

```sh
show dbs
```

## Select a database we use, 
If database doesn't exist then it will create a collection
```sh
use {dbname}
```

## Insert one document
***NOTE***
Here school is database and students is a collection of documents

```sh
use school
```

```sh
db.students.insertOne({name:"Jack",age:2, gpa:3.23})
```

In this command "students" is the collection and "school" is the database
insertOne() command creates both database and collection if they don't exist

## When we insert a document, 
1) We get acknowledged: true or false if entry is added or not
2) ObjectId

>We can create database explicitly with various parameters

```sh
db.createCollection("{Collection name}")
```

## Drop a database

```sh
db.dropDatabase()
```

## Find the document in DB

```sh
db.{Database Name}.find()
```

To find a particular set of key values

Syntax: 

```sh
db.{collection Name}.find({query},{projection})
```

Example: 

```sh 
db.{collection name}.find({},{keyTobeSearched:true})
```

Only returns the keyTobeSearched of the documents

```sh
db.{collection name}.find({},{_id:false,keyTobeSearched:true})
```

Here _id:false will remove _id: ObjectId() and only gives keyTobeSearched
We can have multiple filters in find method to get a specific record

```sh
db.{collection name}.find({key:value,key1:value1},{_id:false})
```

returns all the records with key1 &2 and value 1&2

## Insert many documents
```sh
db.{collection name}.insertMany([{},{},{}])
```

## Mongo Datatypes

```sh
Strings are series of characters 

numbers in strings like "john324234" are considered as characters

Whole numbers are Integers

Decimal numbers are Double

Boolean : true or false

Date: new Date("2023-01-28T00:20:00")

null

[]-Arrays
```
We can have documents within Documents

Example: 
```sh
db.students.insertOne({name:"John",age:23,gpa:2.9,fulltime:false,register:new Date(), graduation:null, courses:["Maths","Physics","Chemistry"],address:{street:"US",city:"New York",pincode:254264}})
```

## To sort (using function chaining)

To sort in ascending order
```sh
db.{collection name}.find().sort({keyToBeSorted:1})
```

To sort in descending order
```sh
db.{collection name}.find().sort({keyToBeSorted:-1})
```

## Limit number of documents
```sh
db.{collection name}.find().limit(noOfrecords to be limited)
```

## Limit documents based on sorted document
example: 
```sh
db.students.find().sort({field:1}).limit(1)
```

## Update one document
```sh
db.{collection name}.updateOne({filter},{update})
```

```sh
db.{collection name}.updateOne(filter,update)
```

Example:

NOTE: $set and $unset are update operators

To set values or add
```sh
> db.{collection name}.updateOne({keytosearch:value},{$set:{keytosearch: valuetoinsert}})
```
To unset values or remove	
```sh
db.{collection name}.updateOne({keytosearch:value},{$unset:{keytosearch: valuetoinsert}})
```

## Update many documents
Selects all documents and adds $set key value
```sh
db.{collection name}.updateMany({},{$set:{fulltime:false}})
```
update to all those documents who does not have a key using $exists query operator

NOTE: $exists query operator not a update operator
```sh
db.{collection name}.updateMany({key:{$exists:false}},{$set:{key:value}})
```
## Delete one documents
```sh
deleteOne()
```
-----------
```sh
db.{database name}.deleteOne({key:value})
```

## Delete many documents

Only deletes the document which matches the filter
```sh
deleteMany()
```
If we want to delete all fields if the key exists or not
```sh
db.{collection name}.deleteMany({key:{$exists:true}}) if exists
```
```sh
db.{collection name}.deleteMany({key:{$exists:false}}) if doesn't exists
```

## Comparision Query Operators

It returns all the documents, key's value is less than the value
```sh
db.{collection name}.find({key:{$lt:value}})
```
It returns all the documents key's value is greater than the value
```sh
db.{collection name}.find({key:{$gt:value}})
```
It returns all the documents key's value is greater than or equal to value
```sh
db.{collection name}.find({key:{$gte:value}})
```
It returns all the documents key's value is less than or equal to value
```sh
db.{collection name}.find({key:{$lte:value}})
```
It returns all the documents with key's value is between value1 & value2
```sh
db.{collection name}.find({key:{$gte:value1, $lte:value2}})
```
It returns all the documents with key's value are matching array of values
```sh
db.{collection name}.find({key:{$in:["value1","value2"]}})
```
It returns all the documents except with key's value that are matching
```sh
db.{collection name}.find({key:{$nin:["value1","value2"]}})
```

## Logical Query Operators
>AND operator
returns all documents that match the conditions of both clauses
```sh
db.{collection name}.find({$and:[{key1:value1},{key2:{$lte:value2}}]})
```
Example:
```sh
db.students.find({$and:[{fulltime:true},{age:{$lte:23}}]})
```
>OR Operator
returns all documents that match the conditions of both clauses
```sh
db.{collection name}.find({$or:[{key1:value1},{key2:{$lte:value2}}]})
```
Example:
```sh
db.students.find({$and:[{fulltime:true},{age:{$lte:23}}]})
```
>NOR Operator
returns all documents that fail to match the clauses
```sh
db.{collection name}.find({$nor:[{key1:value1},{key2:{$lte:value2}}]})
```
Example:
```sh
db.students.find({$nor:[{fulltime:false},{age:{$gte:30}}]})
```
>NOT Operator
returns all documents that do not match the query expresion
```sh
db.{collection name}.find({key:{$not:{$comparision: value}}})
```
Example:
```sh
db.students.find({age:{$not:{$gte:30}}})
```

## Indexes
Indexes allows for quick lookup for the fields

It takes a lot of memory and slows insert update and remove

Reduces the query o/p time

>create a index
```sh
db.{collection name}.createIndex({key:1}) - 1 means ascending
```

>NOTE: to get stats of the query (time taken, look in no of docs)
```sh
db.{collection name}.find({key:value}).explain("executionStats")
```

>After creating an Index for key, query will be quick
```sh
db.{collection name}.find({key:"value"})
```

>Get the indexes
```sh
db.{collection}.getIndexes()
```
## Collections
A collection is a group of Documents

A database is a group of collections

>Show collections
```sh
show collections
```
>Create a collection
```sh
db.createCollection("dbName",{capped:true,size:10000000,max:100},{autoIndexId:false})
```
  dbName -  Name of the collection
  
  capped:true - It tells mongodb that this should have max size
  
  size - size in bytes (10MB - 10 *1024kb *1024 bytes) 
  
  max - maximum no of documents
  
  autoIndexId - doesn't create index automatically

>To drop a collection
```sh
db.{collection name}.drop()
```
