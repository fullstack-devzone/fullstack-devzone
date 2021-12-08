# Fullstack DevZone

FullstackDevZone is an approach to learn various programming languages and frameworks by building a sample application.
This idea is heavily inspired by [RealWorld](https://github.com/gothinkster/realworld).

## DevZone application

DevZone is a web application where developers can register and post their favourite article/video links.
To enable the application to be build using different programming languages and frameworks, 
the Backend and Frontend will be developed as separate applications in separate repositories. 

### Features
* Authentication
    * Users can register, login
* User Profile
    * Provide name, email, bio, photo, tech skills, social media accounts etc
    * Change password
    * Change profile pic
* Any User Actions
    * Any user(including guest users) can view links sort by posted date/votes
* Authenticated User Actions
    * Submit a new article/video link with a set of tags associated to it
    * Delete own links
* Admin User Actions
    * Delete any link

### Run Postman collection

```shell
$ brew install newman //MacOS
$ npm install -g newman
$ newman run devzone-api.postman_collection.json
```

### Backend API Specs

#### Authentication Header:

`Authorization: Bearer jwt_token_here`

#### 1. Login
**URL:** POST /api/auth/login

**Authentication Required:** false

**Request Payload:** 
```json
{
  "username": "user@mail.com",
  "password": "password"
}
```
**Success Response:** 
```json
{
  "access_token": "jwt_token",
  "expires_at": "expiry_timestamp",
  "user":  {
      "id": 1,
      "name": "user",
      "email": "user@mail.com",
      "roles": ["ROLE_ADMIN", "ROLE_USER"]
    }
}
```

**Example:**
```shell script
curl --header "Content-Type: application/json" \
     --request POST \
     --data '{"username":"xyz","password":"xyz"}' \
     http://localhost:8080/api/auth/login
```

#### 2. Registration
**URL:** POST /api/users

**Authentication Required:** false

**Request Payload:** 
```json
{
  "name": "sivaprasad",
  "email": "sivaprasad@gmail.com",
  "password": "password"
}
```

**Success Response:** 
```json
{
    "id": 2,
    "name": "sivaprasad",
    "email": "sivaprasad@gmail.com",
    "roles": [
        "ROLE_USER"
    ]
}
```

#### 3. Change Password
**URL:** PUT /api/user/change-password

**Authentication Required:** yes

**Request Payload:** 

```json
{
  "oldPassword": "old_password",
  "newPassword": "new_password"
}
```

#### 4. Get Current Login User Info
**URL:** GET /api/user

**Authentication Required:** true

**Success Response:** 
```json
{
    "id": 1,
    "name": "user",
    "email": "user@mail.com",
    "roles": ["ROLE_ADMIN", "ROLE_USER"]
}
```

#### 5. Update User Profile
**URL:** PUT /api/user

**Authentication Required:** true

**Request Payload:** 
```json
{
    "name": "user",
    "bio": "",
    "location": "Hyderabad, India",
    "skills": ["Java", "Kotlin", "SpringBoot", "Docker"],
    "githubUsername": "https://github.com/sivaprasadreddy",
    "twitterUsername": "https://twitter.com/sivalabs"
}
```

#### 6. Upload User photo
**URL:** PUT /api/user/profile_pic

**Authentication Required:** true

#### 7. Get User Profiles
**URL:** GET /api/users

**Authentication Required:** false

**Success Response:** 
```json
[
    {
      "name": "user",
      "email": "user@mail.com",
      "bio": "",
      "location": "Hyderabad, India",
      "skills": ["Java", "Kotlin", "SpringBoot", "Docker"],
      "githubUsername": "https://github.com/sivaprasadreddy",
      "twitterUsername": "https://twitter.com/sivalabs"
    }
]
```

#### 8. Get User Profile by id
**URL:** GET /api/users/:user_id

**Authentication Required:** false

**Success Response:** 
```json
{
  "name": "user",
  "email": "user@mail.com",
  "bio": "",
  "location": "Hyderabad, India",
  "skills": ["Java", "Kotlin", "SpringBoot", "Docker"],
  "githubUsername": "https://github.com/sivaprasadreddy",
  "twitterUsername": "https://twitter.com/sivalabs"
}
```

#### 9. Add new link
**URL:** POST /api/links

**Authentication Required:** true

**Request Payload:** 
```json
{
  "title": "post tile",
  "url": "url",
  "tags": ["tag1", "tag2"]
}
```

#### 10. Get link
**URL:** GET /api/links/:link_id

**Authentication Required:** false

**Success Response:**

```json
{
  "id": 123,
  "title": "post tile",
  "url": "url",
  "tags": ["tag1", "tag2"],
  "created_user_id": 123,
  "created_user_name": "siva",
  "created_at": "",
  "updated_at": ""
}
```

#### 11. Update link
**URL:** PUT /api/links/:link_id

**Authentication Required:** true

**Request Payload:** 
```json
{
  "title": "post tile",
  "url": "url",
  "tags": ["tag1", "tag2"]
}
```

#### 12. Delete link
**URL:** DELETE /api/links/:link_id

**Authentication Required:** true

#### 13. Get links for a given page number and sort by created date

##### Get All Links
**URL:** GET /api/links?page=1&size=10&sort=createdAt&direction=DESC

##### Get links by tag
**URL:** GET /api/links?tag=spring&page=1&size=10&sort=createdAt&direction=DESC

##### Search links
**URL:** GET /api/links?query=spring&page=1&size=10&sort=createdAt&direction=DESC

**Authentication Required:** false

**Success Response:** 
```json
{
    "totalElements": 200,
    "totalPages": 10,
    "pageNumber": 1,
    "isFirst": true,
    "isLast": false,
    "hasNext": true,
    "hasPrevious": false,
    "data": [
          {
            "title": "post tile",
            "url": "url",
            "tags": ["tag1", "tag2"],
            "createdBy": {
                "id": 123,
                "name": "siva"
            }       
          },
        {
          "title": "post tile",
          "url": "url",
          "tags": ["tag1", "tag2"],
          "createdBy": {
                "id": 456,
                "name": "prasad"
            } 
        }
    ]
}
```

## Guidelines
* Implement unit and integration tests
* Decent code coverage is nice to have
* Implement continuous integration CI using GitHub Actions or Travis
* Postman collection for API is nice to have
