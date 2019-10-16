# Anywhere Fitness
# API description

Base URL: https://lambda-anywhere-fitness.firebaseio.com/

## Authentication
We'll use Firebase Authentication to let the user sign in. Only implement the "Password Authentication" for now, as it looks the simplest. We may add others as a stretch goal. [Read the docs here](https://firebase.google.com/docs/auth/?authuser=0)

Web: https://firebase.google.com/docs/auth/web/password-auth?authuser=0
iOS: https://firebase.google.com/docs/auth/ios/password-auth?authuser=0

## Data Structure

Full descriptions of the data model is below. Sample JSON files are included in the /JSON folder.

### Users

| Field | Type | Description |
| ----- | ---- | ----------- |
| **uid** | String | The user's ID number, provided by the auth system. |
| **firstName** | String | First name. |
| **lastName** | String | Last name. |
| **email** | String | Email address. |
| **type** | String | Either `client` or `instructor`. |

**Path:** /users/`uid`.json

*Sample JSON not ready yet*

### Classes

| Field | Type | Description |
| ----- | ---- | ----------- |
| **classID** | UUID | An ID number for the class. |
| **instructor** | UUID | The UID of the instructor leading the class |
| **startTime** | timestamp | The time the class starts |
| **duration** | Int | The length of the class in minutes |
| **title** | String | A title for the class |
| **type** | String | A string representing the type of the class. *Do we need to enumerate the options??* |
| **intensity** | String | `Light`, `Medium`, or `Intense` -- *or other terms?* |
| **location** | ??? | *I don't know the best way to describe a location* |
| **price** | Double | The price of the class |
| **maxRegistrants** | Int or null | The maximum number of people who can register |
| **registrants** | [UUID] | Array of users who are registered to take the class. |

**Path:** /classes/`classID`.json

*Sample JSON not ready yet*

