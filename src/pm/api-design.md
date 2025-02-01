# Riveto API Design

This is both the development documentation and the architecture guideline for Riveto Application. Follow this instruction:

1. Everything is Private and will go through the Nestjs's Guard Pipe, except the ones flagged as PUBLIC in this document.
2. Headers (like ) and quotes in this document should match the test description. Headers are `describe("Here", ...)` definition and quotes are `it("HERE", ...)` definition.
3. Database Design is in this file [Riveto Database Design](./db-design.md)

## filter

the filter allow us to filter any data.\
the routes supporting this feature will be flaged as `FILTER`.\
payload:

```ts
  {
    page: number,
    limit: number,
    select: string[], // if empty returns evey field. list of properties that may be inculded
    filters:
    [
  {
    field: 'date',
    type: 'is' | 'isNot' | 'startsWith' | 'endsWith' | 'contains'
           | 'doesNotContain' | 'greaterThan' | 'lessThan'
           | 'between' | 'isGivenDate' | 'afterGivenDate'
           | 'beforeGivenDate' | 'betweenGivenDates'
    from?: T,  // for number and date
    to?: T,    // for number and date
    value?: T, // for strings
  }
],
    sortBy: string,
    orderBy: "asc" | "desc",
  },
```

filter will return an object acording to the selective payload.

## auth

### 🔓 POST `/auth/otp`

req payload:

```json
{ "phoneNumber": "..." }
```

- if user in db
- true > otp generation & save opt in db & send SMS to user & return respective status
- false > return respective status

### 🔓 POST `/auth/login`

req payload:

```json
{ "phoneNumber": "...", "otp": "..." }
```

- if user in db
- true > proceed:
  - if otp in db / user === req.otp
  - true > generate JWT & return respective status + jwt
  - false > return respective status
- false > return respective status

### 🔓 HEAD `/auth/logout`

revoke jwt & return respective status

### 🔓 POST `auth/register`

req payload:

```json
{
  "phoneNumber": "",
  "firstName": "",
  "lastName": ""
}
```

- if user in db
- true > return respective status
- false > save user in db return respective status

## User

### 🔐 GET `/user`

```json
{
  "phoneNumber": "",
  "firstName": "",
  "lastName": "",
  "avatar": "",
  "createdAt": "",
  "lastLogin": "",
  "coinBalance": "",
  "role": ""
}
```

## Transaction

> completely changed. the pr should be reviewd befor updating this section.


## Admin

### 🔐 POST `/admin/users`

`FILTER` functionality

### 🔐 POST `/admin/transactions`

`FILTER` functionality

## Topics

topics group the courses.

### 🔓 GET `topics`

list of topics\
with this respond:

```ts
[
  {
  _id: string;
  slug: string;
  title: string;
  description: string;
  iconUrl: string;
  },
];
```

### 🔓 POST `topics`

### 🔓 PATCH `topics/{old-slug}`

payload:

```ts
[
  {
  slug: string, // for patch this is the new slug
  title: string;
  description: string;
  iconUrl: string;
  },
];
```

## Courses

courses group the lessons.\

### 🔓 GET `courses`

list of courses\
with this respond:

```ts
[
  {
    _id: string;
    slug: String,
    parentTopicSlug: String,
    title: String,
    description: String,
    thumbnailUrl: String,
    videoUrl: String,
    bookmarkedCount: number,
  },
];
```

### 🔓 PATCH `courses/{old-slug}`

### 🔓 POST `courses`

payload:

```ts
[
  {
    slug: string, // for patch this is the new slug
    parentTopicSlug: String,
    title: String,
    description: String,
    thumbnailUrl: String,
    videoUrl: String,
  },
];
```

---

## Lesson

the core of Riveto app.\
### 🔓 GET `/lesson/{slug}`
gets a single lesson
### 🔓 GET `/lessons/{course-slug}`
gets a list of lesson in a specific course
### 🔐 GET `/lessons/`
gets all lessons only for admins
### 🔐 Post `/lesson/`
post lessons only for admins
### 🔐 Patch `/lessons/`
patch lessons only for admins

more info on swagger

---

## TODO

### 🔓 GET `/course/{slug}`

the course\
list of lessons.\
with this respond:

```ts
[{}];
```

### 🔓 GET `/lesson/{slug}`

lesson\
content of a lesson.\
with this respond:

```ts
[
  {
    title: String,
    description: String,
    cover: String,
    video: String,
    topic: String,
    lessons: ["lessonSlug"],
    bookmarkedCount: number,
  },
];
```

GET /admin/user/{id} // all user data
PATCH /admin/user/{id} // edit role
POST admin/lesson/
PATCH admin/lesson/
POST admin/course/
PATCH admin/course/

### 🔓 GET lesson

GET /lesson/{slug}

---

[ ] - /course - POST
[ ] - /course/{id} - GET
[ ] - /course/{id} - PATCH
[ ] - /courses - GET

USER lists\
`"bookmarkedLesson" | "bookmarkedCourse" | "ownedLessons" | "readLesson"`
