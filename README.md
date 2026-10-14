
prac 4  nosql mongodb and crud operation

1. Create the college database and insert student/course records
// Select database
use college

// Create collections
db.createCollection("student")
db.createCollection("course")

// Insert student records
db.student.insertOne({
  RollNo: 83,
  Name: "Neelam",
  City: "Nerul",
  Age: 20,
  Gender: "Female",
  Dob: "19/01/2005",
  PhoneNo: 12345,
  Hobbies: ["Reading", "Writing", "Drawing"]
})

db.student.insertOne({
  RollNo: 90,
  Name: "Rohit",
  City: "Kharghar",
  Age: 22,
  Gender: "Male",
  Dob: "14/09/2002",
  PhoneNo: 67891,
  Hobbies: ["Playing", "Reading", "Singing"]
})

// Display student records
db.student.find()
db.student.find().pretty()

// Insert course records
db.course.insertOne({
  CourseId: "CO1",
  CourseName: "DST",
  Credit: 4,
  NoOfStudentEnrolled: 100
})

db.course.insertOne({
  CourseId: "CO2",
  CourseName: "DMT",
  Credit: 2,
  NoOfStudentEnrolled: 144
})

// Display course records
db.course.find()
db.course.find().pretty()

The following command appears in your notebook, but it deletes the entire college database, including both collections:

// Run only if database deletion is required
db.dropDatabase()
2. Create the users collection and insert sample data
use college

db.createCollection("users")

// Users 1 to 5
for (var i = 1; i < 6; i++) {
  db.users.insertOne({
    Username: "User" + i,
    Age: i + 10,
    Gender: "Female",
    City: "Nerul",
    Country: "India"
  })
}

// Users 7 to 15
for (var i = 7; i < 16; i++) {
  db.users.insertOne({
    Username: "User" + i,
    Age: i + 5,
    Gender: "Male",
    City: "Kapt(a)",
    Country: "Paris"
  })
}

// Users 10 to 24
for (var i = 10; i < 25; i++) {
  db.users.insertOne({
    Username: "User" + i,
    Age: i + 2,
    Gender: "Other",
    City: "London",
    Country: "US"
  })
}

// Display all records
db.users.find()
db.users.find().pretty()

// Find a user with London city and age 25
db.users.find({
  City: "London",
  Age: 25
}).pretty()

// Show only Username and City
db.users.find(
  { City: "London", Age: 25 },
  { Username: 1, City: 1, Age: 0 }
).pretty()

// Limit results
db.users.find().limit(5).pretty()

// Skip first 10 records
db.users.find().skip(10)

// Sort by Age
db.users.find().sort({ Age: 1 })
db.users.find().sort({ Age: -1 })

// Sort by multiple fields
db.users.find().sort({ Country: 1, Age: -1 })
db.users.find().sort({ Country: -1, Age: 1 })
db.users.find().sort({ Gender: 1, Age: 1 })

// Find India or Switzerland
db.users.find({
  $or: [
    { Country: "India" },
    { Country: "Switzerland" }
  ]
})

Note: I've formatted the handwritten examples into runnable commands. A few original values and commands are unclear, so this is a cleaned-up version rather than a guaranteed character-for-character transcription.

3. Update documents using $set
// Update one female user's country
db.users.updateOne(
  { Gender: "Female" },
  { $set: { Country: "Argentina" } }
)

db.users.find().limit(1)

// Add zipcode to one female user
db.users.updateOne(
  { Gender: "Female" },
  { $set: { zipcode: 90909090 } }
)

db.users.find().limit(1)

// Update all female users
db.users.updateMany(
  { Gender: "Female" },
  { $set: { Country: "Argentina" } }
)

db.users.find({ Gender: "Female" })

// Add P.O. box to all female users
db.users.updateMany(
  { Gender: "Female" },
  { $set: { "p.o.box": "abc" } }
)

db.users.find({ Gender: "Female" })

// Update a nested field
db.users.updateMany(
  { Gender: "Female" },
  { $set: { "p.o.box": { o: { boy: "abc" } } }
  }
)

// Add a fax number
db.users.updateMany(
  { Gender: "Female" },
  { $set: { "Fax number": 450987 } }
)

db.users.find()

// Remove the fax number from female users
db.users.updateMany(
  { Gender: "Female" },
  { $unset: { "Fax number": "" } }
)

db.users.find()
4. Remove documents and use cursors
// Remove users whose Gender is Other
db.users.deleteMany({ Gender: "Other" })

db.users.find()

// Remove all documents from the collection
// CAUTION: This empties the users collection
db.users.deleteMany({})

// Drop the collection
// CAUTION: This deletes the collection itself
db.users.drop()

// Find one female user
db.users.findOne({ Gender: "Female" })

// Display all male users using a cursor
var c = db.users.find({ Gender: "Male" })

while (c.hasNext()) {
  printjson(c.next())
}

// Display the second male user (index starts at 0)
var c = db.users.find({ Gender: "Male" })

while (c.hasNext()) {
  printjson(c.next())
}

// Explain the query execution plan
db.users.find({ Gender: "Female" })
  .explain("allPlansExecution")

Remember: findOne() returns one matching document. A cursor lets you process multiple documents one by one.

5. Comparison operators
// Age less than 30
db.users.find({ Age: { $lt: 30 } })

// Age less than or equal to 20
db.users.find({ Age: { $lte: 20 } }).pretty()

// Age greater than 15
db.users.find({ Age: { $gt: 15 } })

// Age greater than or equal to 20
db.users.find({ Age: { $gte: 20 } })

// Update hobbies for all female users
db.users.updateMany(
  { Gender: "Female" },
  {
    $set: {
      Hobbies: ["Singing", "Dancing", "Kho Kho"]
    }
  }
)

db.users.find({ Gender: "Female" })

// Update hobbies for users whose age is 11
db.users.updateMany(
  { Age: 11 },
  {
    $set: {
      Hobbies: ["Singing", "Dancing", "Kho Kho"]
    }
  }
)

db.users.find({ Age: 11 })

// Update hobbies for all users
db.users.updateMany(
  {},
  {
    $set: {
      Hobbies: [
        "Singing",
        "Dancing",
        "Kho Kho",
        "Pottery",
        "Cooking"
      ]
    }
  }
)
6. $in, $nin, $or and $and
// Hobbies include Cooking or Pottery
db.users.find({
  Hobbies: { $in: ["Cooking", "Pottery"] }
})

// Hobbies do not include Reading
db.users.find({
  Hobbies: { $nin: ["Reading"] }
})

// Age greater than 15 OR hobby is Cooking/Pottery
db.users.find({
  $or: [
    { Age: { $gt: 15 } },
    { Hobbies: { $in: ["Cooking", "Pottery"] } }
  ]
})

// Age greater than 15 AND hobby is Cooking/Pottery
db.users.find({
  $and: [
    { Age: { $gt: 15 } },
    { Hobbies: { $in: ["Cooking", "Pottery"] } }
  ]
})
7. Regular expressions
// Insert additional sample users
for (var i = 60; i < 69; i++) {
  db.users.insertOne({
    Username: "User" + i,
    Age: i + 10,
    Gender: "Female",
    City: "Nerul",
    Country: "Switzerland"
  })
}

for (var i = 80; i < 100; i++) {
  db.users.insertOne({
    Username: "User" + i,
    Age: i + 10,
    Gender: "Female",
    City: "Nerul",
    Country: "Nigeria"
  })
}

// Display all users
db.users.find()

// Country starts with "Sw"
db.users.find({ Country: /^Sw/i })

// Country contains "sw", case-insensitive
db.users.find({ Country: /sw/i })

// Country ends with "a"
db.users.find({ Country: /a$/i })

// Country has exactly two characters
db.users.find({ Country: /^.{2}$/i })

// Country contains "t" followed by another character
db.users.find({ Country: /t./i })
8. Indexes
// Ensure an index exists on Age
db.users.createIndex({ Age: 1 })

// Create a unique ascending index on Username
db.users.createIndex(
  { Username: 1 },
  { unique: true }
)

// View indexes
db.users.getIndexes()

// Drop the index on Age
db.users.dropIndex({ Age: 1 })

Important for your practical exam: Run the commands in the same order as the relevant exercise. Commands such as dropDatabase(), drop(), and deleteMany({}) delete data, so don't execute them unless your teacher asks you to demonstrate deletion. Also, because the original notebook uses older MongoDB Shell syntax, I have modernized some commands for current mongosh.

This might also interest you.

BOSTON INSTITUTE OF ANALYTICS GLOBAL EDUCATION PRIVATE LIMITED

Become Job Ready in Data Science, AI & GenAI

Learn in-demand AI skills through practical training, projects and industry experience.

Ad
More options

