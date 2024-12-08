### **Title**: MongoDB Query Practice CRUD 

---

### **1. Introduction to MongoDB Operators**

MongoDB operators allow you to filter, update, and manipulate data effectively. Operators are grouped into several categories:

- **Comparison Operators**
- **Logical Operators**
- **Array Operators**
- **Update Operators**

Each section includes:
- **Description**
- **Syntax**
- **Practice Questions with Examples**

---

### **2. Books Collection Data**

```json
[
  {
    "title": "To Kill a Mockingbird",
    "author": "Harper Lee",
    "year": 1960,
    "rating": 4.27,
    "tags": ["classic", "fiction"],
    "details": { "pages": 281, "publisher": "J.B. Lippincott & Co." }
  },
  {
    "title": "1984",
    "author": "George Orwell",
    "year": 1949,
    "rating": 4.17,
    "tags": ["dystopian", "classic", "fiction"],
    "details": { "pages": 328, "publisher": "Secker & Warburg" }
  },
  {
    "title": "The Great Gatsby",
    "author": "F. Scott Fitzgerald",
    "year": 1925,
    "rating": 3.91,
    "tags": ["classic", "novel"],
    "details": { "pages": 180, "publisher": "Charles Scribner's Sons" }
  }
]
```

---

### **3. Comparison Operators**

#### **Description:**
Comparison operators filter documents based on field values.

#### **Operators and Syntax**:
1. **`$eq`**: Matches values that are equal.
   ```bash
   db.books.find({ "year": { "$eq": 1960 } })
   ```

2. **`$ne`**: Matches values that are not equal.
   ```bash
   db.books.find({ "rating": { "$ne": 4.17 } })
   ```

3. **`$lt`** / **`$lte`**: Matches values less than or less than or equal to.
   ```bash
   db.books.find({ "year": { "$lt": 1950 } })
   db.books.find({ "year": { "$lte": 1960 } })
   ```

4. **`$gt`** / **`$gte`**: Matches values greater than or greater than or equal to.
   ```bash
   db.books.find({ "rating": { "$gt": 4.0 } })
   db.books.find({ "rating": { "$gte": 4.17 } })
   ```

5. **`$in`** / **`$nin`**: Matches any value in or not in an array.
   ```bash
   db.books.find({ "tags": { "$in": ["dystopian", "classic"] } })
   db.books.find({ "tags": { "$nin": ["fiction"] } })
   ```

---

### **4. Logical Operators**

#### **Description:**
Logical operators combine multiple conditions in a query.

#### **Operators and Syntax**:
1. **`$and`**
   ```bash
   db.books.find({ "$and": [{ "rating": { "$gt": 4 } }, { "year": { "$lt": 1970 } }] })
   ```

2. **`$or`**
   ```bash
   db.books.find({ "$or": [{ "year": { "$lt": 1940 } }, { "rating": { "$gt": 4 } }] })
   ```

3. **`$not`**
   ```bash
   db.books.find({ "rating": { "$not": { "$gte": 4.0 } } })
   ```

4. **`$nor`**
   ```bash
   db.books.find({ "$nor": [{ "tags": "classic" }, { "year": { "$lt": 1950 } }] })
   ```

---

### **5. Array Operators**

#### **Description:**
Array operators perform operations on array fields.

#### **Operators and Syntax**:
1. **`$all`**: Matches arrays containing all specified elements.
   ```bash
   db.books.find({ "tags": { "$all": ["classic", "fiction"] } })
   ```

2. **`$size`**: Matches arrays with a specific number of elements.
   ```bash
   db.books.find({ "tags": { "$size": 3 } })
   ```

3. **`$elemMatch`**: Matches arrays where at least one element satisfies all conditions.
   ```bash
   db.books.find({ "details": { "$elemMatch": { "pages": { "$gte": 300 } } } })
   ```

4. **`$push`** (with `$each`): Adds elements to an array.
   ```bash
   db.books.updateOne({ "title": "1984" }, { "$push": { "tags": { "$each": ["award-winning", "political"] } } })
   ```

5. **`$pull`**: Removes elements from an array.
   ```bash
   db.books.updateOne({ "title": "1984" }, { "$pull": { "tags": "fiction" } })
   ```

---

### **6. Update Operators**

#### **Description:**
Update operators modify fields in documents.

#### **Operators and Syntax**:
1. **`$set`**: Updates the value of a field.
   ```bash
   db.books.updateOne({ "title": "1984" }, { "$set": { "rating": 4.2 } })
   ```

2. **`$unset`**: Removes a field from a document.
   ```bash
   db.books.updateOne({ "title": "1984" }, { "$unset": { "details.publisher": "" } })
   ```

3. **`$inc`**: Increments the value of a field.
   ```bash
   db.books.updateOne({ "title": "The Great Gatsby" }, { "$inc": { "details.pages": 20 } })
   ```

4. **`$push`**: Adds an element to an array.
   ```bash
   db.books.updateOne({ "title": "The Great Gatsby" }, { "$push": { "tags": "award-winning" } })
   ```

5. **`$pull`**: Removes an element from an array.
   ```bash
   db.books.updateOne({ "title": "1984" }, { "$pull": { "tags": "fiction" } })
   ```

---

### **7. Dot Notation**

#### **Description:**
Dot notation allows querying and updating nested fields in MongoDB.

#### **Practice Examples**:
1. Query books where the `pages` in `details` are greater than 200.
   ```bash
   db.books.find({ "details.pages": { "$gt": 200 } })
   ```

2. Update the publisher of "The Great Gatsby" in the nested `details` field.
   ```bash
   db.books.updateOne({ "title": "The Great Gatsby" }, { "$set": { "details.publisher": "Updated Publisher" } })
   ```

---
