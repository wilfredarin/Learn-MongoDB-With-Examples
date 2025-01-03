##Mongoose Documentation

## Overview

This document serves as a comprehensive guide to using Mongoose in a **Node.js** application with **MongoDB**. It covers key concepts such as defining schemas, field validations, setters/getters, virtuals, instance and static methods, hooks (pre/post), and relationship types (One-to-One, One-to-Many, Many-to-Many). We'll use a **BlogPost** application with **Users** and **BlogPosts** as examples to demonstrate these concepts.

---

## 1. **Understanding Relationships in Mongoose**

### **One-to-One Relationship**

A **One-to-One relationship** in Mongoose means that one document in a collection is associated with exactly one document in another collection.

For example, in the context of the **User** and **Profile** scenario, each user could have only one profile, and each profile is associated with exactly one user.

#### Example: **User and Profile**

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

// Profile Schema
const profileSchema = new Schema({
  bio: String,
  age: Number,
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  }
});

const Profile = mongoose.model('Profile', profileSchema);

// User Schema
const userSchema = new Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true }
});

const User = mongoose.model('User', userSchema);

// Example: Create a new User and associated Profile
async function createProfile() {
  const user = new User({
    name: 'Jane Doe',
    email: 'jane.doe@example.com'
  });
  await user.save();

  const profile = new Profile({
    bio: 'I am a passionate developer.',
    age: 28,
    user: user._id
  });
  await profile.save();

  console.log('Profile Created:', profile);
}
```

In this case, each user has exactly one profile, and a profile is linked to only one user.

### **One-to-Many Relationship**

A **One-to-Many relationship** occurs when a document in one collection is associated with multiple documents in another collection. 

In our **BlogPost** example, a **User** can create many **BlogPosts**, but each **BlogPost** has only one **author** (User).

#### Example: **User and BlogPost (One-to-Many)**

```javascript
const blogPostSchema = new Schema({
  title: { type: String, required: true },
  content: { type: String, required: true },
  author: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  comments: [{
    user_id: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    comment: { type: String },
    createdAt: { type: Date, default: Date.now }
  }]
});

const BlogPost = mongoose.model('BlogPost', blogPostSchema);

// Example: Create a BlogPost for a User
async function createBlogPost(userId) {
  const blogPost = new BlogPost({
    title: 'Mongoose Relationships',
    content: 'Mongoose helps us manage relationships between models.',
    author: userId // The User who created the blog post
  });
  await blogPost.save();
  console.log('Blog Post Created:', blogPost);
}
```

In this case, a **User** can have many **BlogPosts**, and each **BlogPost** has exactly one **author** (User).

### **Many-to-Many Relationship**

A **Many-to-Many relationship** occurs when multiple documents in one collection are related to multiple documents in another collection. This is typically modeled using arrays of references to other documents.

In our example, each **BlogPost** can have multiple **comments**, and each **comment** could belong to different **Users**.

#### Example: **BlogPost and Comments (Many-to-Many)**

```javascript
const commentSchema = new Schema({
  user_id: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  comment: { type: String },
  createdAt: { type: Date, default: Date.now }
});

blogPostSchema.add({ comments: [commentSchema] });  // Add comments field to BlogPost

const BlogPost = mongoose.model('BlogPost', blogPostSchema);

// Example: Add a comment to a BlogPost
async function addCommentToBlogPost(blogPostId, userId, commentText) {
  const blogPost = await BlogPost.findById(blogPostId);
  blogPost.comments.push({ user_id: userId, comment: commentText });
  await blogPost.save();
  console.log('Comment Added:', blogPost.comments);
}
```

In this case, a **BlogPost** can have many **comments**, and each **comment** is associated with a **User**, creating a **Many-to-Many** relationship between **BlogPosts** and **Users**.

---

## 2. **Mongoose Features and Best Practices**

### **Setters and Getters**

Setters and getters allow you to manipulate the data before it’s saved or when it’s accessed.

- **Setter**: A function that modifies the value of a field before saving.
- **Getter**: A function that is executed when retrieving a field.

#### Example: **Setter and Getter on User Schema**

```javascript
userSchema
  .path('name')
  .set(function (value) {
    return value.charAt(0).toUpperCase() + value.slice(1); // Capitalize first letter
  })
  .get(function (value) {
    return value.toLowerCase(); // Get the name in lowercase
  });
```

### **Virtual Fields**

Virtuals are fields that are not stored in MongoDB but can be calculated dynamically.

#### Example: **Virtual for BlogPost Comment Count**

```javascript
blogPostSchema.virtual('commentCount').get(function () {
  return this.comments.length;
});
```

### **Field Validations**

You can define custom validators for fields.

#### Example: **Password Validation**

```javascript
userSchema.path('password').validate(function (value) {
  return /[A-Z]/.test(value);  // Password must contain at least one uppercase letter
}, 'Password must contain at least one uppercase letter');
```

### **Instance Methods**

Instance methods are functions that are available on instances of the model.

#### Example: **Adding a Comment on BlogPost**

```javascript
blogPostSchema.methods.addComment = function (userId, commentText) {
  this.comments.push({ user_id: userId, comment: commentText });
  return this.save();
};
```

### **Static Methods**

Static methods are functions that are available on the model itself (not on instances).

#### Example: **Get Posts by User**

```javascript
blogPostSchema.statics.getPostsByUser = function (userId) {
  return this.find({ author: userId });
};
```

### **Pre and Post Hooks**

Pre and Post hooks are functions that run before or after certain operations like save or remove.

#### Example: **Pre and Post Save Hooks for BlogPost**

```javascript
// Pre-save hook: Automatically set createdAt date
blogPostSchema.pre('save', function (next) {
  if (!this.createdAt) {
    this.createdAt = Date.now();
  }
  next();
});

// Post-save hook: Log after saving a blog post
blogPostSchema.post('save', function (doc) {
  console.log('Blog Post has been saved:', doc.title);
});
```

---

## 3. **Example Usage: Full Blog Application**

### **Creating a User**

```javascript
async function createUser() {
  const user = new User({
    name: 'Alice',
    email: 'alice@example.com',
    password: 'Password123'
  });
  await user.save();
  console.log('User Created:', user);
}
```

### **Creating a BlogPost for a User**

```javascript
async function createBlogPostForUser(userId) {
  const blogPost = new BlogPost({
    title: 'Understanding MongoDB Relationships',
    content: 'Learn about One-to-One, One-to-Many, and Many-to-Many relationships in MongoDB.',
    author: userId
  });
  await blogPost.save();
  console.log('Blog Post Created:', blogPost);
}
```

### **Adding Comments to a BlogPost**

```javascript
async function addComment(blogPostId, userId, commentText) {
  const blogPost = await BlogPost.findById(blogPostId);
  await blogPost.addComment(userId, commentText);
  console.log('Comment Added:', blogPost.comments);
}
```

---

## 4. **Conclusion**

This comprehensive guide to Mongoose in Node.js covers key concepts for building a full-featured application with MongoDB. We’ve explored **One-to-One, One-to-Many, and Many-to-Many** relationships, as well as Mongoose features such as **validation**, **setters/getters**, **virtuals**, **instance methods**, **static methods**, **pre/post hooks**, and more.

By understanding and applying these concepts, you will be able to build efficient and well-structured applications that take full advantage of MongoDB's flexibility and Mongoose's powerful features.

