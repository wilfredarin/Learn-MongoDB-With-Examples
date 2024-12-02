### **questions.md**  

# MongoDB Query Practice Questions  

This file contains various MongoDB query practice questions categorized by difficulty. Use the provided dataset (`dataset.json`) to practice each query.  

## **Basic Queries**  
1. Find all books written by the author "ABC".  
2. Find books with a rating of 4.5.  
3. Find books with more than 300 pages.  
4. Return only the `title` and `author` fields of all documents.  
5. Count the total number of books written by "ABC".  
6. Retrieve three books with the highest ratings.  
7. Find all books with exactly two genres.  

## **Intermediate Queries**  
8. Find all books with a rating greater than 7 and less than 9.  
9. Retrieve books whose `genres` array contains "fantasy".  
10. Find all books with ratings either 7, 8, or 9.  
11. Return all books that do **not** have the genre "sci-fi".  
12. Retrieve books with less than 300 or more than 500 pages.  
13. Return books that have both "magic" and "fantasy" in their genres.  
14. Find all reviews written by the reviewer named "John Doe".  
15. Find books that do not have a rating of 4 or less.  

## **Advanced Queries**  
16. Use `$nor` to find books that neither have the genre "Drama" nor more than 400 pages.  
17. Retrieve books where the `genres` array has at least one element matching "adventure".  
18. Find books with a rating exactly equal to 5 and more than 350 pages using `$and`.  
19. Find books with genres that include "mystery" and have at least three genres using `$size`.  
20. Use `$elemMatch` to find books with a review where the reviewer is "Jane Smith" and the review body contains "excellent".  

## **Update Operations**  
21. Add a new genre "classic" to all books with a rating above 8.  
22. Remove the genre "romance" from any book that contains it.  
23. Increase the page count by 50 for all books with more than 300 pages.  
24. Update the rating to 5 for all books written by "XYZ".  
25. Remove a review written by "Alex" from all books.  

## **Delete Operations**  
26. Delete all books with a rating less than 3.  
27. Delete all books that have only one genre.  
28. Remove all books written by the author "Unknown".  

## **Aggregation Queries**  
29. Find the average rating of all books grouped by the author.  
30. List the total number of books in each genre using the `$group` operator.  

## **Bonus Questions**  
31. Find books with a title that starts with the letter "A".  
32. Retrieve books where the page count is an even number.  
33. Return the top three longest books by page count.  

---  

These questions are designed to cover a variety of MongoDB operations, including CRUD operations, query operators, and aggregation techniques.
