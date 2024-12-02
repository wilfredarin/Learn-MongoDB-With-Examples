### **answers.md**  

# MongoDB Query Practice Answers  

This file contains the answers to the MongoDB query practice questions. Refer to the corresponding question number to check the solution.  

---

## **Basic Queries**  

**1. Find all books written by the author "ABC".**  
```javascript
db.books.find({ author: "ABC" })
```

**2. Find books with a rating of 4.5.**  
```javascript
db.books.find({ rating: 4.5 })
```

**3. Find books with more than 300 pages.**  
```javascript
db.books.find({ pages: { $gt: 300 } })
```

**4. Return only the `title` and `author` fields of all documents.**  
```javascript
db.books.find({}, { title: 1, author: 1 })
```

**5. Count the total number of books written by "ABC".**  
```javascript
db.books.countDocuments({ author: "ABC" })
```

**6. Retrieve three books with the highest ratings.**  
```javascript
db.books.find().sort({ rating: -1 }).limit(3)
```

**7. Find all books with exactly two genres.**  
```javascript
db.books.find({ genres: { $size: 2 } })
```

---

## **Intermediate Queries**  

**8. Find all books with a rating greater than 7 and less than 9.**  
```javascript
db.books.find({ rating: { $gt: 7, $lt: 9 } })
```

**9. Retrieve books whose `genres` array contains "fantasy".**  
```javascript
db.books.find({ genres: "fantasy" })
```

**10. Find all books with ratings either 7, 8, or 9.**  
```javascript
db.books.find({ rating: { $in: [7, 8, 9] } })
```

**11. Return all books that do **not** have the genre "sci-fi".**  
```javascript
db.books.find({ genres: { $ne: "sci-fi" } })
```

**12. Retrieve books with less than 300 or more than 500 pages.**  
```javascript
db.books.find({ $or: [{ pages: { $lt: 300 } }, { pages: { $gt: 500 } }] })
```

**13. Return books that have both "magic" and "fantasy" in their genres.**  
```javascript
db.books.find({ genres: { $all: ["magic", "fantasy"] } })
```

**14. Find all reviews written by the reviewer named "John Doe".**  
```javascript
db.books.find({ "reviews.name": "John Doe" })
```

**15. Find books that do not have a rating of 4 or less.**  
```javascript
db.books.find({ rating: { $gt: 4 } })
```

---

## **Advanced Queries**  

**16. Use `$nor` to find books that neither have the genre "Drama" nor more than 400 pages.**  
```javascript
db.books.find({
    $nor: [
        { genres: "Drama" },
        { pages: { $gt: 400 } }
    ]
})
```

**17. Retrieve books where the `genres` array has at least one element matching "adventure".**  
```javascript
db.books.find({ genres: { $elemMatch: { $eq: "adventure" } } })
```

**18. Find books with a rating exactly equal to 5 and more than 350 pages using `$and`.**  
```javascript
db.books.find({
    $and: [
        { rating: 5 },
        { pages: { $gt: 350 } }
    ]
})
```

**19. Find books with genres that include "mystery" and have at least three genres using `$size`.**  
```javascript
db.books.find({
    genres: { $all: ["mystery"] },
    genres: { $size: { $gte: 3 } }
})
```

**20. Use `$elemMatch` to find books with a review where the reviewer is "Jane Smith" and the review body contains "excellent".**  
```javascript
db.books.find({
    reviews: {
        $elemMatch: {
            name: "Jane Smith",
            body: /excellent/i
        }
    }
})
```

---

## **Update Operations**  

**21. Add a new genre "classic" to all books with a rating above 8.**  
```javascript
db.books.updateMany(
    { rating: { $gt: 8 } },
    { $push: { genres: "classic" } }
)
```

**22. Remove the genre "romance" from any book that contains it.**  
```javascript
db.books.updateMany(
    { genres: "romance" },
    { $pull: { genres: "romance" } }
)
```

**23. Increase the page count by 50 for all books with more than 300 pages.**  
```javascript
db.books.updateMany(
    { pages: { $gt: 300 } },
    { $inc: { pages: 50 } }
)
```

**24. Update the rating to 5 for all books written by "XYZ".**  
```javascript
db.books.updateMany(
    { author: "XYZ" },
    { $set: { rating: 5 } }
)
```

**25. Remove a review written by "Alex" from all books.**  
```javascript
db.books.updateMany(
    { "reviews.name": "Alex" },
    { $pull: { reviews: { name: "Alex" } } }
)
```

---

## **Delete Operations**  

**26. Delete all books with a rating less than 3.**  
```javascript
db.books.deleteMany({ rating: { $lt: 3 } })
```

**27. Delete all books that have only one genre.**  
```javascript
db.books.deleteMany({ genres: { $size: 1 } })
```

**28. Remove all books written by the author "Unknown".**  
```javascript
db.books.deleteMany({ author: "Unknown" })
```

---

## **Aggregation Queries**  

**29. Find the average rating of all books grouped by the author.**  
```javascript
db.books.aggregate([
    { $group: { _id: "$author", avgRating: { $avg: "$rating" } } }
])
```

**30. List the total number of books in each genre using the `$group` operator.**  
```javascript
db.books.aggregate([
    { $unwind: "$genres" },
    { $group: { _id: "$genres", count: { $sum: 1 } } }
])
```

---

## **Bonus Questions**  

**31. Find books with a title that starts with the letter "A".**  
```javascript
db.books.find({ title: /^A/ })
```

**32. Retrieve books where the page count is an even number.**  
```javascript
db.books.find({ pages: { $mod: [2, 0] } })
```

**33. Return the top three longest books by page count.**  
```javascript
db.books.find().sort({ pages: -1 }).limit(3)
```  

---  

These are the answers for the MongoDB query practice questions.
