# Riveto UX Design

This is the flow of user interaction with Riveto app. REMEMBER the main purpose is to remove the distractions and help user focus on learning. Design patter CDD.

## navbar

- right: logo
- left: !isLoggedIn ? "Login" Button : avatar

## home

- !isLoggedIn ? landing : personal path comp.

## /account

main parts:

- avatar
- coin balance number
- first and last name
- read lessons sum number
- bookmarked lessons sum number

menu items:

- isAdmin ? "admin menu item" : null
- riveto coin vault
- lists
  - bookmarked lessons
  - bookmarked courses
  - read
  - coin log
  - transactions
- support

## /admin

menu items:

- users
  - admin should see data displayed in such a way that transfer an immediate picture of the overall activity of the use like lastLogin and sum.
- lessons
- courses
- transactions
  each list is a data table make it possible to sort and update data accordingly.

## course and lesson "list/edit/add" page

- add new on list
- on each list item bookmark and views are clear
- wee need sorting so data table is suggested
- based on the api design parts are clear.
  the page has 2 cols. they get separated respectively.

## buying coin

have 2 section.

- coin packs that has offs.
- custom number that will generate a number based on the formula

## lesson page

- has two cols. side & body. both is displayed in mobile.
- in body if hasVideo ? video header : cover-title-desc header
- we have 4 sections preview/body/exam/overview that
- in side:
  - bookmark toggle
  - read toggle
  - share

parts that are not yet developed will be marked as limited access. featured access will be hidden either to persuade them to login or to buy the course.
