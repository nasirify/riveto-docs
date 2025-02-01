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


> completely changed. the pr should be reviewd befor updating this section.

## Topics

```ts
{
  slug: string;
  title: string;
  description: string;
  iconUrl: string;
}
```

## Courses

```ts
{
    slug: String,
    parentTopicSlug: String,
    title: String,
    description: String,
    thumbnailUrl: String,
    videoUrl: String,
    lessons: [{type: mongoose.Types.ObjectId, ref: "Lesson"}],
    bookmarkedBy: [{type: mongoose.Types.ObjectId, ref: "User"}],
}
```

## Lessons

```ts
{
    slug: string,
    title: string,
    description: string,
    status: 'Draft' | 'Published' | 'Deleted',
    access: 'Public' | 'LoggedIn' | 'Premium',
    hierarchy: number,
    requiredCoins: number,
    thumbnailUrl: string,
    videoUrl?: string,
    introduction: {/* tiptap */},
    content: {/* tiptap */},
    exercise: {/* tiptap */},
    exam: {/* tiptap */},
    author: {type: mongoose.Types.ObjectId, ref: "User"},
    courses: [{type: mongoose.Types.ObjectId, ref: "Course"}],
    bookmarkedBy: {type: mongoose.Types.ObjectId, ref: "User"},
    readBy: [{type: mongoose.Types.ObjectId, ref: "User"}],
    ownedBy: [{type: mongoose.Types.ObjectId, ref: "User"}],
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
