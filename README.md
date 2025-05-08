# Mongo-Db

MongoDB Atlas: The fully managed service for MongoDB deployments in the cloud 

MongoDB Enterprise: The subscription-based, self-managed version of MongoDB

MongoDB Community: The source-available, free-to-use, and self-managed version of MongoDB


The advantages of using documents are:

Documents correspond to native data types in many programming languages.

Embedded documents and arrays reduce need for expensive joins.

Dynamic schema supports fluent polymorphism.


In addition to collections, MongoDB supports:

Read-only Views

On-Demand Materialized Views


MongoDB's replication facility, called replica set, provides:

automatic failover

data redundancy.

data types

String: "Hello, World!"
Integer: 42
Boolean: true or false
Double: 42.0
Array: [1, 2, 3]
Object: { "name": "John", "age": 30 }
Null: null
Date: ISODate("2023-10-01T00:00:00Z")
ObjectId: ObjectId("507f1f77bcf86cd799439011")

C-CREATE,insert
R-find
U-update,
D-delete
db.movies.insertOne(
  {
    title: "The Favourite",
    genres: [ "Drama", "History" ],
    runtime: 121,
    rated: "R",
    year: 2018,
    directors: [ "Yorgos Lanthimos" ],
    cast: [ "Olivia Colman", "Emma Stone", "Rachel Weisz" ],
    type: "movie"
  }
)

db.movies.find( { title: "The Favourite" } )

use sample_mflix

db.movies.insertMany([
   {
      title: "Jurassic World: Fallen Kingdom",
      genres: [ "Action", "Sci-Fi" ],
      runtime: 130,
      rated: "PG-13",
      year: 2018,
      directors: [ "J. A. Bayona" ],
      cast: [ "Chris Pratt", "Bryce Dallas Howard", "Rafe Spall" ],
      type: "movie"
    },
    {
      title: "Tag",
      genres: [ "Comedy", "Action" ],
      runtime: 105,
      rated: "R",
      year: 2018,
      directors: [ "Jeff Tomsic" ],
      cast: [ "Annabelle Wallis", "Jeremy Renner", "Jon Hamm" ],
      type: "movie"
    }
])


to retreive all  db.movies.find({})

Although you can express this query using the $or operator, use the $in operator rather than the $or operator when performing equality checks on the same field.

Both find methods accept a second parameter called projection

$size: Matches any array with the number of elements specified.
Example: { tags: { $size: 3 } }

$addToSet: Adds an item to an array only if it does not already exist.
Example: { $addToSet: { tags: "green" } }
$set: Sets the value of a field in a document.
Example: { $set: { age: 30 } }




Calculate the total number of customers by country.
javascript

Verify
Edit
Copy code
db.customers.aggregate([
    {
        $group: {
            _id: "$country",
            totalCustomers: { $sum: 1 }
        }
    }
]);

db.customers.aggregate([
    {
        $match: {
            country: { $in: ["UK", "Spain"] }
        }
    },
    {
        $group: {
            _id: null,
            totalCustomerId: { $sum: "$customerid" }
        }
    }
]);


$eq: Matches values that are equal to a specified value.
Example: { age: { $eq: 25 } }
$ne: Matches values that are not equal to a specified value.
Example: { status: { $ne: "A" } }
$gt: Matches values that are greater than a specified value.
Example: { age: { $gt: 18 } }
$gte: Matches values that are greater than or equal to a specified value.
Example: { age: { $gte: 18 } }
$lt: Matches values that are less than a specified value.
Example: { age: { $lt: 18 } }
$lte: Matches values that are less than or equal to a specified value.
Example: { age: { $lte: 18 } }
$in: Matches any of the values specified in an array.
Example: { status: { $in: ["A", "B"] } }
$nin: Matches none of the values specified in an array.
Example: { status: { $nin: ["A", "B"] } }


Logical Operators
$and: Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.
Example: { $and: [ { age: { $gt: 25 } }, { status: "A" } ] }
$or: Joins query clauses with a logical OR and returns all documents that match the conditions of either clause.
Example: { $or: [ { age: { $gt: 25 } }, { status: "A" } ] }
$not: Inverts the effect of a query expression.
Example: { age: { $not: { $gt: 25 } } }
$nor: Joins query clauses with a logical NOR and returns all documents that fail to match both clauses.
Example: { $nor: [ { age: { $gt: 25 } }, { status: "A" } ] }

Element Operators

$exists: Matches documents that have the specified field.
Example: { name: { $exists: true } }
$type: Selects documents if a field is of the specified type.
Example: { age: { $type: "int" } }


Array Operators

$all: Matches arrays that contain all elements specified in the query.
Example: { tags: { $all: ["red", "blue"] } }
$elemMatch: Matches documents that contain an array field with at least one element that matches all the specified query criteria.
Example: { results: { $elemMatch: { score: { $gt: 80 }, grade: "A" } } }
$size: Matches any array with the number of elements specified.
Example: { tags: { $size: 3 } }

Update Operators

$set: Sets the value of a field in a document.
Example: { $set: { age: 30 } }
$unset: Removes the specified field from a document.
Example: { $unset: { age: "" } }
$inc: Increments the value of a field by a specified amount.
Example: { $inc: { age: 1 } }
$push: Adds an item to an array.
Example: { $push: { tags: "blue" } }
$pull: Removes all instances of a value from an array.
Example: { $pull: { tags: "blue" } }
$addToSet: Adds an item to an array only if it does not already exist.
Example: { $addToSet: { tags: "green" } }


db.posts.updateOne( { title: "Post Title 1" }, { $set: { likes: 2 } } )

db.posts.updateMany({}, { $inc: { likes: 1 } })
  
db.posts.deleteOne({ title: "Post Title 5" }) 

db.posts.deleteMany({ category: "Technology" }) 

Aggregation pipeline

db.users.aggregate([
  { $match: { age: { $gt: 30 } } }
])


db.users.aggregate([
  { $group: { _id: "$city", totalUsers: { $sum: 1 } } }
])

db.users.aggregate([
  { $project: { name: 1, age: 1, ageInMonths: { $multiply: ["$age", 12] } } }
])

db.users.aggregate([
  { $sort: { age: -1 } }
])

db.users.aggregate([
  { $skip: 3 }
])

db.users.aggregate([
  { $unwind: "$hobbies" }
])

db.users.aggregate([
  { $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "userId",
      as: "userOrders"
    }
  }
])

db.users.aggregate([
  { $addFields: { fullName: { $concat: ["$firstName", " ", "$lastName"] } } }
])

Primary Index: Created on the _id field by default. This is a unique index for every document, which helps MongoDB quickly locate documents by their unique ID.
Single Field Index: Created on a single field to improve queries that only involve that field.
db.students.createIndex({ age: 1 }) 

Compound Index: Created on multiple fields, enabling more efficient querying on combinations of those fields.
db.students.createIndex({ grade: 1, age: 1 }) 


Multikey Index: Created on array fields to optimize queries involving array values.


Text Index: Used for text search, allowing search within string fields for words or phrases.


aggregation is used or array []
{$count:"count"}
db.users.aggregate([
   {
      $project: {
         fullName: { $concat: ["$firstName", " ", "$lastName"] }, // Concatenate firstName, a space, and lastName
         _id: 0 // Exclude the '_id' field (optional)
      }
   }
]);

Improves Readability: Makes JSON output easier to read during development and debugging.
Debugging Nested Structures: Useful when dealing with deeply nested documents.
Quick Formatting: No need for external tools to format raw MongoDB output.
Limitations
pretty() is only for formatting query results in the shell; it does not alter the structure of data in the database.
It’s specific to the MongoDB shell (not typically used in drivers like Node.js or Python


db.students.aggregate([
   {
      $addFields: {
         status: {
            $cond: { 
               if: { $gte: ["$marks", 50] }, // Condition: marks >= 50
               then: "Pass",                 // Value if condition is true
               else: "Fail"                  // Value if condition is false
            }
         }
      }
   }
]);

employees: { $push: "$name" }   

indexing

db.students.createIndex({ name: 1 }, { unique: true });

To ensure the indexes are being used efficiently, you can use the explain() method:

javascript
Copy code
db.students.find({ class: "10A" }).sort({ marks: 1 }).explain("executionStats");









