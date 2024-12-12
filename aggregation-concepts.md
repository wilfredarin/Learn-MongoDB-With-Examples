**MongoDB Aggregation Pipeline Practice Document**  

### Dataset for Practice  

To follow along and practice the queries, click on the link below to access the datasets:  
- **Dataset Link**: [Dataset Link](https://gist.github.com/hiteshchoudhary/a80d86b50a5d9c591198a23d79e1e467)  

The link includes three JSON files:  
1. `authors.json`  
2. `data.json`  
3. `books.json`  

#### Creating Collections and Adding Data  

Run the following commands in your MongoDB shell to create and populate the collections:  

1. **Create `authors` collection and insert data**:  
   ```javascript
   db.authors.insertMany(
     // Copy and paste the entire `authors.json` data here
   )
   ```  

2. **Create `data` collection and insert data**:  
   ```javascript
   db.data.insertMany(
     // Copy and paste the entire `data.json` data here
   )
   ```  

3. **Create `books` collection and insert data**:  
   ```javascript
   db.books.insertMany(
     // Copy and paste the entire `books.json` data here
   )
   ```  

Once the data is inserted, you can use the collections to practice the queries in the tutorial.  

---

### What is the Aggregation Pipeline?  
The aggregation pipeline in MongoDB processes data by passing documents through a series of stages, each performing a specific operation. It’s ideal for complex queries, data analysis, and transformations.  

---

### Key Aggregation Stages  
- **$match**: Filters documents based on a condition (like WHERE in SQL).  
- **$group**: Groups documents by a field or expression, enabling aggregate calculations like SUM or AVG.  
- **$project**: Reshapes documents by selecting, modifying, or creating fields.  
- **$unwind**: Breaks arrays into multiple documents, one per array element.  
- **$sort**: Sorts documents in ascending or descending order.  
- **$limit**: Limits the number of documents returned.  

---

### Important Operators  

#### **Basic Operators**  

**$count**  
- **Syntax**:  
  `{ $count: "fieldName" }`  
- **Description**: Counts the number of documents in the pipeline.  
- **Example**:  
  ```javascript
  db.users.aggregate([
    { $match: { isActive: true } },
    { $count: "activeUsers" }
  ])
  ```  

**$sum**  
- **Syntax**:  
  `{ $sum: <expression> }`  
- **Description**: Calculates the sum of values in a group.  
- **Example**:  
  ```javascript
  { $group: { _id: null, totalAge: { $sum: "$age" } } }
  ```  

**$avg**  
- **Syntax**:  
  `{ $avg: <expression> }`  
- **Description**: Computes the average of values in a group.  
- **Example**:  
  ```javascript
  { $group: { _id: null, averageAge: { $avg: "$age" } } }
  ```  

---

#### **Array-Specific Operators**  

**$unwind**  
- **Syntax**:  
  `{ $unwind: "$arrayField" }`  
- **Description**: Deconstructs an array into individual documents, one for each element.  
- **Example**:  
  ```javascript
  { $unwind: "$tags" }
  ```  

**$addToSet**  
- **Syntax**:  
  `{ $addToSet: <expression> }`  
- **Description**: Collects unique values into an array.  
- **Example**:  
  ```javascript
  { $group: { _id: null, uniqueTags: { $addToSet: "$tags" } } }
  ```  

**$push**  
- **Syntax**:  
  `{ $push: <expression> }`  
- **Description**: Collects all values into an array, including duplicates.  
- **Example**:  
  ```javascript
  { $group: { _id: null, allTags: { $push: "$tags" } } }
  ```  

---

### Using $group and Accumulators  
The $group stage allows grouping of documents by a specific field or expression and performs operations using accumulators.  

**Syntax**:  
```javascript
{
  $group: {
    _id: <expression>,
    <field1>: { <accumulator>: <expression> },
    ...
  }
}
```  

**Common Accumulators**:  
- **$sum**: Adds numeric values in a group.  
- **$avg**: Computes the average of numeric values.  
- **$min**: Finds the smallest value.  
- **$max**: Finds the largest value.  
- **$addToSet**: Collects unique values in an array.  
- **$push**: Collects all values (including duplicates) in an array.  

---

### Questions and Queries  

1. **Count the Number of Active Users**  
   **Query**:  
   ```javascript
   db.users.aggregate([
     { $match: { isActive: true } },
     { $count: "activeUsers" }
   ])
   ```  

2. **Calculate Average Age**  
   **Query**:  
   ```javascript
   db.users.aggregate([
     { $group: { _id: null, averageAge: { $avg: "$age" } } }
   ])
   ```  

3. **Average Number of Tags per User (Using $unwind)**  
   **Query**:  
   ```javascript
   db.users.aggregate([
     { $unwind: "$tags" },
     { $group: { _id: "$_id", totalTags: { $sum: 1 } } },
     { $group: { _id: null, averageTagsPerUser: { $avg: "$totalTags" } } }
   ])
   ```  

4. **List Top 5 Favorite Fruits**  
   **Query**:  
   ```javascript
   db.users.aggregate([
     { $group: { _id: "$favoriteFruit", count: { $sum: 1 } } },
     { $sort: { count: -1 } },
     { $limit: 5 }
   ])
   ```  

5. **Find Total Males and Females**  
   **Query**:  
   ```javascript
   db.users.aggregate([
     { $group: { _id: "$gender", count: { $sum: 1 } } }
   ])
   ```  

6. **Find Users with "enim" as a Tag**  
   **Query**:  
   ```javascript
   db.users.aggregate([
     { $match: { tags: "enim" } },
     { $count: "usersWithEnim" }
   ])
   ```  

7. **Categorize Users by Favorite Fruit**  
   **Query**:  
   ```javascript
   db.users.aggregate([
     { $group: { _id: "$favoriteFruit", users: { $push: "$name" } } }
   ])
   ```  

8. **Users with "ad" as the Second Tag**  
   **Query**:  
   ```javascript
   db.users.aggregate([
     { $match: { "tags.1": "ad" } },
     { $count: "usersWithSecondTagAd" }
   ])
   ```  

9. **List Companies in the USA and User Counts**  
   **Query**:  
   ```javascript
   db.users.aggregate([
     { $match: { "company.location.country": "USA" } },
     { $group: { _id: "$company.title", userCount: { $sum: 1 } } }
   ])
   ```  

10. **Find Unique Eye Colors**  
    **Query**:  
    ```javascript
    db.users.aggregate([
      { $group: { _id: null, uniqueEyeColors: { $addToSet: "$eyeColor" } } }
    ])
    ```
11. **Match Books with Their Authors**  
Write a query to match books with their respective authors and display the book title along with the author's name.  

**Query**:  
```javascript
db.books.aggregate([
  {
    $lookup: {
      from: "authors", // The collection to join
      localField: "authorId", // Field from the books collection
      foreignField: "_id", // Field from the authors collection
      as: "authorDetails" // Name of the new array field
    }
  },
  {
    $project: {
      _id: 0,
      title: 1,
      "authorDetails.name": 1
    }
  }
])
```  

**Expected Output**:  
A list of book titles with their respective authors' names.  


### Links for Additional Learning  
- **YouTube Playlist for MongoDB Aggregation**: [YouTube Link](https://youtube.com/playlist?list=PLRAV69dS1uWQ6CZCehxKy0rjkqhQ2Z88t&si=JZN9xn8T9HIevjWT)  

