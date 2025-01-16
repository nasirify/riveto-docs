# Riveto API Design

This is both the development documentation and the architecture guideline for Riveto Application. Follow this instruction:

1. Everything is Private and will go through the Nestjs's Guard Pipe, except the ones flagged as PUBLIC in this document.
2. Headers (like ) and quotes in this document should match the test description. Headers are `describe("Here", ...)` definition and quotes are `it("HERE", ...)` definition.
3. Database Design is in this file [Riveto Database Design](./db-design.md)

## auth

### 🔓 PUBLIC POST /auth/otp

req payload:

```json
{ "phoneNumber": "..." }
```

- if user in db
- true > otp generation & save opt in db & send SMS to user & return respective status
- false > return respective status

### 🔓 PUBLIC POST /auth/login

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

### 🔓 PUBLIC HEAD /auth/logout

- revoke jwt & return respective status

### 🔓 PUBLIC POST auth/register

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

## user

### 🔐 GET /user/{id}

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

### 🔐 GET /user/list

query params:

```ts
  {
    type: "bookmarkedLesson" | "bookmarkedCourse" | "ownedLessons" | "readLesson" | "transactions",
    page: number,
    limit: number,
    filter: {
     field: 'date'
     from: T,  // Generic Type
     to: T,    // Generic Type
    }
    sortBy: "ace" | "dcs",
  },
```

it returns:

```json
{
  "list": [{ "slug": "", "title": "", "createdAt": "" }],
  "total": number,
  "page": number,
  "limit": number,
}
```

## admin

GET /admin/users
GET /admin/transactions
GET /admin/user/{id} // all user data
GET /admin/user/{id}/list // like /user/list
PATCH /admin/user/{id} // edit role
POST admin/lesson/
PATCH admin/lesson/
POST admin/course/
PATCH admin/course/

### 🔓 GET lesson

GET /lesson/{slug}

### 🔓 GET Course

GET /course/{slug}

---

[ ] - /course - POST
[ ] - /course/material - POST
[ ] - /course/material/{id} - GET
[ ] - /course/material/{id} - PATCH
[ ] - /course/materials - GET
[ ] - /course/{id} - GET
[ ] - /course/{id} - PATCH
[ ] - /courses - GET

[ ] - /user/transactions - GET
[ ] - /user/wallet - GET
[ ] - /user/wallet - POST
