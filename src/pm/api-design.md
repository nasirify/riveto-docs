# Riveto API Design

This is both the development documentation and the architecture guideline for Riveto Application. Follow this instruction:

1. Everything is Private and will go through the Nestjs's Guard Pipe, except the ones flagged as PUBLIC in this document.
2. Headers (like ) and quotes in this document should match the test description. Headers are `describe("Here", ...)` definition and quotes are `it("HERE", ...)` definition.
3. Database Design is in this file [Riveto Database Design](./db-design.md)

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

### 🔐 POST `/transaction/coin-purchase`

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

### 🔐 GET `/user/list`

query params:

```ts
  {
    type: "transactions",
    page: number,
    limit: number,
    filters:
    [
  {
    field: 'date',
    type: 'is' | 'isNot' | 'startsWith' | 'endsWith' | 'contains'
           | 'doesNotContain' | 'greaterThan' | 'lessThan'
           | 'between' | 'isDate' | 'isNotDate' | 'afterDate'
           | 'beforeDate' | 'betweenDates' | 'isTime' | 'isNotTime',
    from?: T,  // Generic Type
    to?: T,    // Generic Type
    value?: T, // Generic Type
  }
]
    sortBy: "ace" | "dcs",
  },
```

it returns:

```ts
{
  list: [{ "slug": "", "title": "", "createdAt": "" }],
  total: number,
  page: number,
  limit: number,
}
```

## Transaction

### 🔐 POST `/transaction/coin-purchase`

FrontEnd sends a req this this backend route `/transaction/coin-purchase`

```ts
{
    amount: number,
    coinAmount: number,
    gateway: 'zatinpal',
    callbackUrl: string,
    description: string,
  }
```

and backend responses the gateway url like this:

```ts
{
  gatwayURL: string;
}
```

and frontend forward the user to `gatwayURL`

### 🔐 GET `/transaction/purchase-verify/{id,gateway}`

now the user is in the gatway.\
user might abort payment. if not, the user will be redirected to the `callbackURL` with this pattern of url query param: `?Authority=XXX&Status=XXX` then we send a request to this route of the backend: `/transaction/purchase-verify/{id}` as param

the backend will return this object:

```ts
{
    transactionId: string,
}
```

## Admin

### 🔐 POST `/admin/users`

### 🔐 POST `/admin/transactions`

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

it returns:

```ts
{
  list: [{ "slug": "", "title": "", "createdAt": "" }],
  total: number,
  page: number,
  limit: number,
}
```

## Topics

topics group the courses.

### 🔓 GET `topics`

list of topics\
with this response:

```ts
[
  {
  slug: string;
  title: string;
  description: string;
  iconUrl: string;
  },
];
```

### 🔓 POST `topics/{slug}`

### 🔓 PATCH `topics/{slug}`

payload:

```ts
[
  {
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
with this response:

```ts
[
  {
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

### 🔓 PATCH `courses/{slug}`

### 🔓 POST `courses/{slug}`

payload:

```ts
[
  {
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




---

## TODO

### 🔓 GET `/course/{slug}`

the course\
list of lessons.\
with this response:

```ts
[{}];
```

### 🔓 GET `/lesson/{slug}`

lesson\
content of a lesson.\
with this response:

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
