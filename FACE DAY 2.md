# MongoDB CRUD Practice Notes

**Database:** `PCEA24CY031`
**Collection:** `products`
**Environment:** MongoDB Atlas + `mongosh`

## 1. Product Data Creation

```javascript
for (let i = 1; i <= 50; i++) {
    db.products.insertOne({
        productId: i,
        productName: "Product " + i,
        category:
            i % 5 === 0 ? "Laptop" :
            i % 5 === 1 ? "Mobile" :
            i % 5 === 2 ? "Headphones" :
            i % 5 === 3 ? "Keyboard" :
                          "Mouse",
        brand:
            i % 3 === 0 ? "Dell" :
            i % 3 === 1 ? "Samsung" :
                          "HP",
        price: 1000 + (i * 500),
        stock: 10 + i,
        rating: 3 + ((i % 3) * 0.5),
        inStock: i % 4 !== 0,
        tags: [
            "electronics",
            i % 2 === 0 ? "featured" : "new"
        ],
        seller: {
            sellerId: 1000 + i,
            sellerName: "Seller " + i
        },
        createdAt: new Date()
    });
}
```

## 2. Read / Find

```javascript
db.products.find({ category: "Laptop" })
```

```javascript
db.products.find({ inStock: true })
```

```javascript
db.products.find({ price: { $gt: 20000 } })
```

```javascript
db.products.find({ price: { $lt: 10000 } })
```

```javascript
db.products.find({
    price: { $gte: 10000, $lte: 30000 }
})
```

```javascript
db.products.find({
    "seller.sellerName": "Seller 10"
})
```

## 3. Update One

```javascript
db.products.updateOne(
    { productId: 1 },
    { $set: { price: 10000 } }
)
```

```javascript
db.products.updateOne(
    { productId: 2 },
    { $set: { price: 15000, stock: 100 } }
)
```

```javascript
db.products.updateOne(
    { productId: 3 },
    { $set: { "seller.sellerName": "Premium Seller" } }
)
```

```javascript
db.products.updateOne(
    { productId: 4 },
    { $push: { tags: "sale" } }
)
```

```javascript
db.products.updateOne(
    { productId: 5 },
    { $addToSet: { tags: "electronics" } }
)
```

```javascript
db.products.updateOne(
    { productId: 6 },
    { $inc: { stock: 20 } }
)
```

```javascript
db.products.updateOne(
    { productId: 6 },
    { $inc: { stock: -5 } }
)
```

## 4. Update Many

```javascript
db.products.updateMany(
    { category: "Laptop" },
    { $set: { inStock: true } }
)
```

```javascript
db.products.updateMany(
    { brand: "Samsung" },
    { $inc: { price: 1000 } }
)
```

## 5. Multiple Operators

```javascript
db.products.updateOne(
    { productId: 10 },
    {
        $set: { price: 25000, inStock: true },
        $inc: { stock: 10 },
        $push: { tags: "sale" }
    }
)
```

## 6. Product 20 Practical

```javascript
db.products.updateMany(
    { productId: 20 },
    { $set: { price: 17000 } }
)
```

```javascript
db.products.updateOne(
    { productId: 20 },
    { $set: { "seller.sellerName": "Kartikey Store" } }
)
```

```javascript
db.products.updateOne(
    { productId: 20 },
    { $inc: { stock: 20 } }
)
```

```javascript
db.products.updateOne(
    { productId: 20 },
    { $unset: { rating: "" } }
)
```

```javascript
db.products.find({ productId: 20 })
```

## 7. Verification Commands

```javascript
db.products.countDocuments()
```

```javascript
db.products.find({ productId: 20 })
```

```javascript
db.products.findOne({ productId: 20 })
```

```javascript
db.products.find()
```

## 8. Delete Operations

```javascript
db.products.deleteOne({ productId: 10 })
```

```javascript
db.products.deleteMany({ category: "Laptop" })
```

```javascript
db.products.deleteMany({ price: { $gt: 30000 } })
```

```javascript
db.products.deleteMany({})
```

```javascript
db.products.drop()
```

`deleteMany({})` deletes all documents but keeps the collection.

`drop()` deletes the entire collection.

# Part 4 — Students Dataset

Collection: `students`

Fields:
- `rollNo`
- `name`
- `age`
- `department`
- `marks`
- `skills`
- `address`
- `scholarship`

## 9. Logical Operators

### CSE AND marks greater than 85

```javascript
db.students.find({
    $and: [
        { department: "CSE" },
        { marks: { $gt: 85 } }
    ]
})
```

### CSE OR IT

```javascript
db.students.find({
    $or: [
        { department: "CSE" },
        { department: "IT" }
    ]
})
```

### Marks NOT greater than 80

```javascript
db.students.find({
    marks: {
        $not: { $gt: 80 }
    }
})
```

### Neither CSE nor IT

```javascript
db.students.find({
    $nor: [
        { department: "CSE" },
        { department: "IT" }
    ]
})
```

## 10. Comparison Operators

| Operator | Meaning |
|---|---|
| `$eq` | Equal To |
| `$gt` | Greater Than |
| `$lt` | Less Than |
| `$gte` | Greater Than or Equal |
| `$lte` | Less Than or Equal |
| `$ne` | Not Equal |

### Department exactly CSE

```javascript
db.students.find({
    department: { $eq: "CSE" }
})
```

### Marks greater than 80

```javascript
db.students.find({
    marks: { $gt: 80 }
})
```

### Marks less than 70

```javascript
db.students.find({
    marks: { $lt: 70 }
})
```

### Marks greater than or equal to 85

```javascript
db.students.find({
    marks: { $gte: 85 }
})
```

### Marks less than or equal to 70

```javascript
db.students.find({
    marks: { $lte: 70 }
})
```

### Department not CSE

```javascript
db.students.find({
    department: { $ne: "CSE" }
})
```

## 11. Array Operators

### Department CSE or IT

```javascript
db.students.find({
    department: { $in: ["CSE", "IT"] }
})
```

### Department neither CSE nor IT

```javascript
db.students.find({
    department: { $nin: ["CSE", "IT"] }
})
```

### Both Python and MongoDB skills

```javascript
db.students.find({
    skills: { $all: ["Python", "MongoDB"] }
})
```

### Add subjects to Student 101

```javascript
db.students.updateOne(
    { rollNo: 101 },
    {
        $set: {
            subjects: [
                { name: "Java", marks: 90 },
                { name: "MongoDB", marks: 88 },
                { name: "Python", marks: 95 }
            ]
        }
    }
)
```

### MongoDB subject with marks greater than 85

```javascript
db.students.find({
    subjects: {
        $elemMatch: {
            name: "MongoDB",
            marks: { $gt: 85 }
        }
    }
})
```

### Exactly 3 skills

```javascript
db.students.find({
    skills: { $size: 3 }
})
```

## 12. Projection Operators

### Name, department and marks

```javascript
db.students.aggregate([
    {
        $project: {
            _id: 0,
            name: 1,
            department: 1,
            marks: 1
        }
    }
])
```

### Only name and marks

```javascript
db.students.find(
    {},
    {
        _id: 0,
        name: 1,
        marks: 1
    }
)
```

### Everything except address

```javascript
db.students.find(
    {},
    {
        address: 0
    }
)
```

### First two skills

```javascript
db.students.find(
    {},
    {
        name: 1,
        skills: { $slice: 2 }
    }
)
```

## 13. Element Operators

### Scholarship field exists

```javascript
db.students.find({
    scholarship: { $exists: true }
})
```

### Scholarship field does not exist

```javascript
db.students.find({
    scholarship: { $exists: false }
})
```

### Marks stored as integer

```javascript
db.students.find({
    marks: { $type: "int" }
})
```

### Names starting with A

```javascript
db.students.find({
    name: { $regex: "^A" }
})
```

# Operator Summary

## Update
```text
$set
$inc
$unset
$push
$addToSet
```

## Logical
```text
$and
$or
$not
$nor
```

## Comparison
```text
$eq
$gt
$lt
$gte
$lte
$ne
```

## Array
```text
$in
$nin
$all
$elemMatch
$size
```

## Projection / Element
```text
$project
$slice
$exists
$type
$regex
```
