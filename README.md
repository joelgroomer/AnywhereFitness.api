# Anywhere Fitness - Firebase API description

Base URL: https://lambda-anywhere-fitness.firebaseio.com/

# Authentication
We'll use Firebase Authentication to let the user sign in. Only implement the "Password Authentication" for now, as it looks the simplest. We may add others as a stretch goal. [Read the docs here](https://firebase.google.com/docs/auth/?authuser=0)

Web: https://firebase.google.com/docs/auth/web/password-auth?authuser=0

iOS: https://firebase.google.com/docs/auth/ios/password-auth?authuser=0

# Data Structure

Full descriptions of the data model is below. Sample JSON files are included in the /JSON folder.

## Users

| Field | Type | Description |
| ----- | ---- | ----------- |
| **uid** | `String` | The user's ID number, provided by the auth system. |
| **firstName** | `String` | First name. |
| **lastName** | `String` | Last name. |
| **email** | `String` | Email address. |
| **userType** | `String` | Either `client` or `instructor`. |
| **registrations** | [`UUID` as String] | An array of classes the user has registered for. |
| **punchcards** | [`UUID` as String] | An array of punchcard IDs for cards held by the user. |
| **metro** | `String` | The metro area the user lives in for location filtering default. |

### Endpoints
| Method | Endpoint | Description |
| ------ | -------- | ----------- |
| **PUT** | /users/`uid`.json | Create a new user or update existing user. Use the UID supplied by firebase auth. If updating, send the entire object. |

### Sample JSON
[All Users](/JSON/allUsers.json): /users.json

[A Single User](/JSON/singleUser.json): /users/00f8c84a-8e99-4732-b11c-a0d3cfc8b10d.json

## Classes

| Field | Type | Description |
| ----- | ---- | ----------- |
| **classID** | `UUID` as String | An ID number for the class. |
| **instructor** | `UUID` as String | The UID of the instructor leading the class. |
| **startTime** | `timestamp` | The time the class starts. |
| **duration** | `Int` | The length of the class in minutes. |
| **title** | `String` | A title for the class. |
| **category** | `String` | A string representing the type of the class, enumerated in `categories`. |
| **intensity** | `String` | `light`, `medium`, or `intense` -- *or other terms?* |
| **metro** | `String` | The metro area the class is in, to help with searching for local classes. |
| **addr1** | `String` | Address line 1. |
| **addr2** | `String` | Address line 2. |
| **city** | `String` | City name. |
| **state** | `String` | State. |
| **zip** | `String` | Zip code. |
| **price** | `Double` | The price of the class. |
| **maxRegistrants** | `Int` or `null` | The maximum number of people who can register. |
| **registrants** | [`UUID` as String] | Array of users who are registered to take the class. |
| **offerPunchcard** | `Bool` | If `true`, allows clients who do not already have a card started for this instructor and category to create a new one. If `false`, a new card can't be created, but **this should not affect clients who already have punchcards** from getting a punch for this class. |

### Endpoints
| Method | Endpoint | Description |
| ------ | -------- | ----------- |
| **GET** | /classes | Returns a list of all future classes. |
| **GET** | /classes/`:classID`.json | Returns a particular class. |
| **PUT** | /classes | Create a class. |
| **PUT** | /classes/`:classID`.json | Create or update a class. Should only be updated by an instructor with a UID matching the one in the `instructor` field of the class. When creating a new class, generate a UUID first. Send entire object when updating. |
| **DELETE** | /classes/`:classID`.json | Delete a class. |

### Sample JSON
[All Classes](/JSON/allClasses.json): /classes.json

[A Single Class](/JSON/singleClass.json): /classes/ffeb3806-91af-4e63-9baa-531d2b318267.json 

## Categories

| Field | Type | Description |
| ----- | ---- | ----------- |
| **uuid** | `UUID` as String | An ID number for the category. |
| **name** | `String` | The name of the category. |
| **desc** | `String` or `null` | A description of the category. |

### Endpoints
| Method | Endpoint | Description |
| ------ | -------- | ----------- |
| **GET** | /categories | Returns a list of categories. |
| **GET** | /categories/`:categoryID`.json | Returns a particular category. |
| **PUT** | /categories/`:categoryID`.json | Create or update a category. When creating, generate a UUID first. When updating, send entire object. |
| **DELETE** | /categories/`:categoryID`.json | Delete a category. |

### Sample JSON
[All Categories](/JSON/allCategories.json): /categories.json

[A Single Category](/JSON/singleCategory.json): /categories/9b4fa838-d7f6-40d3-bb23-75b5b8db668e.json

## Punchcards

| Field | Type | Description |
| ----- | ---- | ----------- |
| **uuid** | `UUID` as String | An ID number for the punchcard. |
| **instructor** | `UUID` as String | The ID number for the instructor the card applies to. |
| **category** | `UUID` as String | The ID number for the class category the card applies to. |
| **spots** | `Int` | The total number of spots that need to be punched. *We should have a max value* |
| **punched** | `Int` | The number of spots that have been punched. |
| **redeemed** | `Bool` | Whether or not the reward has been redeemed. |

### Endpoints
| Method | Endpoint | Description |
| ------ | -------- | ----------- |
| **GET** | /punchards/`:punchcardID`.json | Returns a punchcard. |
| **PUT** | /punchards/`:punchcardID`.json | Create or update a punchcard. Send entire object. |

*Sample JSON not ready yet*

# How it works

Instructors can create classes and modify details of their classes.
- At the time of class creation, instructors can choose if they want to offer a punchcard.
  - This should default to `false`
  - Punchcards are probably a stretch goal. The implementation might be a little weird to meet the requirements we were given, so let's not worry about it unless we have time. **Just set this to false for now.**
- **Stretch:** We should notify registrants via push or email when a class is cancelled.
Both Clients and Instructors can register for classes (no need to check userType).
- When a user registers for a class
  - Add the classID to the user's `registrations` array.
  - Add the userID to the class' `registrants` array.
- When cancelling a registration, remove both of the above.
