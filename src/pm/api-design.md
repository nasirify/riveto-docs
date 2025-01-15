# Riveto API Design

This is both the development documentation and the architecture guideline for Riveto Application. Follow this instruction:

1. Everything is Private and will go through the Nestjs's Guard Pipe, except the ones flagged as PUBLIC in this document.
2. Headers (like ) and quotes in this document should match the test description. Headers are `describe("Here", ...)` definition and quotes are `it("HERE", ...)` definition.
3. Database Design is in this file [Riveto Database Design](./db-design.md)

## auth

> POST /auth/login

PUBLIC

- payload: {“userMobilePhone”:”...”}
- if user exists in db
- . true > otp generation & save in db & send SMS & return truthy
- false > return falsy + “user does not exists”

> POST /auth/otp

PUBLIC

- payload: {“userMobilePhone”:”...”, “userLoginOtp”:”...”}
- if otp in db / user === req.otp
- true > generate JWT & return truthy + jwt
- false > return unauthorized access err

> HEAD /auth/logout

PUBLIC

- return HEAD to clean jwt cookie

> POST auth/register

PUBLIC

- payload: {
  "phoneNumber":"",
  "firstName":"",
  "lastName":""
  }
- if user in db
- true > return falsy
- false > save user in db and return true

## user

> GET /user/{id}

- if !isLoggedIn return:

{
"phoneNumber":"",
"firstName":"",
"lastName":"",
"profileUrl":"",
"createdAt":"",
"lastLogin":"",
"bookmarkedLesson":"",
"bookmarkCourses":"".
"coinBalance":""
}


## admin

> GET /admin/{id}

- if isAdmin
- true > return truthy
- false > return falsy
> GET /admin/users
- true > return truthy
- false > return falsy

[ ] - /admin/transactions - GET
[ ] - /admin/user/{id}/ - PATCH
[ ] - /admin/users - GET

[ ] - /course - POST
[ ] - /course/material - POST
[ ] - /course/material/{id} - GET
[ ] - /course/material/{id} - PATCH
[ ] - /course/materials - GET
[ ] - /course/{id} - GET
[ ] - /course/{id} - PATCH
[ ] - /courses - GET

[ ] - /user/{id}/transactions - GET
[ ] - /user/{id}/wallet - GET
[ ] - /user/{id}/wallet - POST
[ ] - /user/getUser - GET
