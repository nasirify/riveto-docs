# Riveto Database Design

This is both the development documentation and the architecture guideline for Riveto Application. Follow this instruction:

1. we use mongodb with mongoose.
2. here we define the schema & relationships

## User

```ts
{
  phoneNumber: String,
  firstName: String,
  lastName: String,
  avatar: String,
  createdAt: Date,
  lastLogin: Date,
  coinBalance: Number,
  otp: Number,
  role: 'pioneer' | 'user' | 'admin'
}
```

## Transactions

// TODO this does not match backend implementation

```ts
{
    user: {type: mongoose.Types.ObjectId, ref: "User"},
    amount: Number,
    price: Number,
    status: {
        type: 'Completed' | 'pending' | 'failed',
        default: 'pending',
    },
    metadata: {
    type: Object,       // data from Zarinpal you need to save
    },
    pendingAt: Date,    // date with exact time to the second
    updatedAt: Date,    // date with exact time to the second

}
```

## Lessons

```ts
{
    slug: String,
    title: String,
    description: String,
    order: Number,
    priceInCoins: Number,
    cover: String,
    video: String,
    views: Number,
    createdAt: Date,
    updatedAt: Date,
    excerpt: {/* tiptap */}
    body: {/* tiptap */}
    createdBy: {type: mongoose.Types.ObjectId, ref: "User"},
    courses: [{type: mongoose.Types.ObjectId, ref: "Course"}],
    bookmarkedBy: {type: mongoose.Types.ObjectId, ref: "User"},
    readBy: {type: mongoose.Types.ObjectId, ref: "User"},
    ownedBy: {type: mongoose.Types.ObjectId, ref: "User"},
}
```

## Courses

```ts
{
    slug: String,
    title: String,
    description: String,
    cover: String,
    video: String,
    topic: String,
    lessons: [{type: mongoose.Types.ObjectId, ref: "Lesson"}],
    bookmarkedBy: {type: mongoose.Types.ObjectId, ref: "User"},
}
```

## OwnedLessons

```ts
{
    lesson: {type: mongoose.Types.ObjectId, ref: "Lesson"},
    user: {type: mongoose.Types.ObjectId, ref: "User"},
    createdAt: Date,
    paidCoins: Number,
}
```

## BookmarkedCourses

```ts
{
    course: {type: mongoose.Types.ObjectId, ref: "Course"},
    user: {type: mongoose.Types.ObjectId, ref: "User"},
    createdAt: Date,
}
```

## BookmarkedLessons

```ts
{
    lesson: {type: mongoose.Types.ObjectId, ref: "Lesson"},
    user: {type: mongoose.Types.ObjectId, ref: "User"},
    createdAt: Date,
}
```

## ReadLessons

```ts
{
    lesson: {type: mongoose.Types.ObjectId, ref: "Lesson"},
    user: {type: mongoose.Types.ObjectId, ref: "User"},
    updatedAt: Date,
    isDeleted: Boolean,
}
```

## Topics

```ts
{
  title: string,
  slug: string,
  description: string,
  imageUrl: string,
}
```
