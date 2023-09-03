# MongoDB
### MongoDB is a NoSQL Database Management System that can manage humongous amount of data.
#### SQL --> Structured Query Language
- It stores data in the form of tables.
#### NoSQL --> Not only Structured Query Language
- It stores data in the form of documents, data in each document stored as key-value pairs similar to JSON data but here it is called BSON(Binary JavaSript Object Notation), the advantage is data which is frequently accessed together is stored together rather than in seperate tables like in SQL because some times SQL Joins would feel like little bit confuse to work with.

Database -> Tables -> Records\
Database -> Collections -> Documents

### creating databases & collections
{show dbs} shows the available databases.

{use <database_name>} switches to the database <database_name> if it already exists or creates it if it doesn't exit.

{db.dropDatabase()} drops the current database.

{db.createCollection('collection_name')} creates a new collection named 'collection_name'

{show collections} to show all the collections in the current db

{db.createCollection('collection_name', {capped : true, size : 10000, max: 100}, {autoIndexID : false})}\will create a collection with some upper limit of size and number of documents in the collection, autoIndexID to false will tell mongoDB to not create the default index on the _ id field in this collection.
By default mongoDB creates an index on the _ id field in a collection.

{db.collection_name.drop()} to drop the collection.

### insert method
{db.collection_name.insertOne({key value pairs here})} inserts a document into the collection named 'collection_name'

{db.collection_name.insertMany([{key value pairs here}, {key value pairs here}, {key value pairs here}])} inserts many documents into the collection named 'collection_name'
- we can insert various datatypes like string, integer, doubles(decimal numbers), boolean(true or false), date object (using new Date()), null(we can give null as value to a key), arrays(my-array: [ ]), objects(my_object: { }).

### find method
{db.collection_name.find()} show or lists all the documents of the collection named 'collection_name'

{db.collection_name.find().sort({key value pairs})} to sort all the listed documents wrt to key value pairs provided.

Eg - 
- db.students.find().sort({name : 1}) sorts the documents in alphb. order
- db.students.find().sort({name : -1}) sorts the documents in reverse alphb. order

{db.collection_name.find().limit(x)} limits the documents listed to a number x, i.e., total documents lisited will be x.
we can also use multiple methods like
- {db.collection_name.find().sort({}).limit(x)}

{db.collection_name.find({query}, {projection})} 
- {query} -> filters the documents wrt the query provided, query is just a document or a collection of key value pairs
- {projection} -> is used to show only some specific fields of all the documents after filtering, projection is just a document or a collection of key value pairs.\
Ex -
- db.students.find({id:1})
- db.students.find({grade:4.0, age:16})
- db.students.find({}, {id:false,name:true, gpa:true})\
MongoDB will give you _id automatically so we are giving false to it.\
No filters given, but attributes to be listed are given

### update method
{db.collection_name.updateOne({filter}, {update})}
- {query} -> filters the documents to select the document whose data to update
- {update} -> actually updates the document 
Ex - 
db.students.updateOne({name: "Sam"}, {$ set : {age : 20}})
Instead of updating an attribute with $ set, an attribute can also be remove using $ unset
Ex - 
db.students.updateOne({name: "Sam"}, {$ unset : {age : ""}})

{db.collection_name.updateMany({query}, {update})}
- {query} -> same as updateOne()
- {update} -> same as updateOne()
Ex -
db.students.updateMany({}, {$ set : {fulltime: false}})
Updates everyone's fulltime attribute to false
Similarly $ unset can be used here

Ex - 
db.students.updateMany({fulltime : {$ exists:false}}, {$ set : {fulltime: true}})

{db.collection_name.deleteOne({query})}
- {query} -> filters the document to select the document to delete

{db.collection_name.deleteMany({query})}
- {query} -> filters the document to select all the documents to delete

{db.collection_name.deleteMany({registerDate:{$exists:false}})}
- we are deleting the documents where registerDate doesn't exist. 

### comparison operators
Comparision Operators -
ne -> not equal
lt -> less than
lte -> less than equals to
gt -> greater than
gte -> greater than equals to

{db.collection_name.find({name : {$ ne : 'NAME'}})} 
to find all the documents whose name attribute is not equal to 'NAME'

{db.collection_name.find({gpa : {$ lt: value}})}
to find all the documents whose gpa attribute's value is less than 'value'

{db.collection_name.find({gpa : {$ gte : 3, $ lte : 4}})}
to find all the documents whose gpa attribute's value is between 3 and 4

{db.collection_name.find({name : {$ in : ['value 1', 'value 2', 'value n']}})}
to find all the documents whose name attribute's value is one of 'value 1', 'value 2', 'value n'
$ in -> in
$ nin -> not in

db.students.find({ $ and : [{fullTime:true}, {age : {$ gte : 18}  }]  }, {_ id:false, name:true, age:true})
to find all the documents where the value of fullTime attribute is true and age is less than or equal to 'value'

### Logical operators
Logical Operators - 
$ and
$ or
$ nor
$ not

db.collection_name.find({$ and: [{fulltime:true},{age:{$ gte : value}}]})

db.collection_name.find({age : {$ not: {$ gte : value}}})
to find all the documents where the value of the age attribute is not greater than or equal to 'value', i.e., is less than 'value'

### Indexes
{db.collection_name.find({name:'value'}).explain('executionStats')}
to get info about the execution of the query

{db.collection_name.createIndex({name : 1})}
will return the name of the index created.
Index will be created on the 'name' field

{db.collection_name.getIndexes()}
will return all the indexes created for the collection

{db.collection_name.dropIndex("name_of_index")}
to drop the index

Some Examples -
db.students.find({ gpa : {$ gte:5, $ lte:9}, fullTime : { $ exists: true } }, {_ id:false, name:true, gpa: true})

db.students.find({name : {$ ne : "Sam"}}, {_ id:false,name:true, gpa:true}).sort({gpa:-1}).limit(3)

Complex -> 
 db.students.find({ $ and : [{fullTime:true}, {age : {$ gte : 18}  }], $ or : [{name : { $ in : ["Sam", "Patric"] }  }, {age: { $ gte : 10 }  }]  }, {_ id:false, name:true, age:true})
