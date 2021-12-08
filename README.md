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

Assuming the backend API application is running on http://localhost:8080/ and there are 2 users exists with following credentials:
* **Admin**: admin/admin
* **Demo User**: demo/demo

we can run smoke tests using the postman collection as follows:

```shell
$ brew install newman //MacOS
$ npm install -g newman
$ newman run devzone-api.postman_collection.json
```

### Backend API Specs

#### Authentication Header:

`Authorization: Bearer jwt_token_here`

#### 1. Login
* **URL:** POST /api/auth/login
* **Authentication Required:** false
<details>
  <summary>Click to expand!</summary>

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
curl --location --request POST 'http://localhost:8080/api/auth/login' \
--header 'Content-Type: application/json' \
--data-raw '{
	"username": "admin@gmail.com",
	"password": "admin"
}'
```
</details>


#### 2. Registration
* **URL:** POST /api/users
* **Authentication Required:** false
<details>
  <summary>Click to expand!</summary>

**Request Payload:** 
```json
{
  "name": "sivaprasad",
  "email": "sivaprasad@gmail.com",
  "password": "password"
}
```

```shell
curl --location --request POST 'http://localhost:8080/api/users' \
--header 'Content-Type: application/json' \
--data-raw '{
	"name":"newuser",
	"email": "newuser@gmail.com",
	"password": "secret"
}'
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
</details>

#### 3. Change Password
* **URL:** PUT /api/user/change-password
* **Authentication Required:** yes
<details>
  <summary>Click to expand!</summary>

**Request Payload:** 

```json
{
  "oldPassword": "old_password",
  "newPassword": "new_password"
}
```

```shell
curl --location --request PUT 'http://localhost:8080/api/user/change-password' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
  "oldPassword": "secret",
  "newPassword": "secret"
}'
```
</details>

#### 4. Get Current Login User Info
* **URL:** GET /api/user
* **Authentication Required:** true
<details>
  <summary>Click to expand!</summary>

**Success Response:** 
```json
{
    "id": 1,
    "name": "user",
    "email": "user@mail.com",
    "roles": ["ROLE_ADMIN", "ROLE_USER"]
}
```

```shell
curl --location --request GET 'http://localhost:8080/api/user' \
--header 'Authorization: Bearer JWT_TOKEN'
```
</details>

#### 5. Update User Profile
* **URL:** PUT /api/user
* **Authentication Required:** true
<details>
  <summary>Click to expand!</summary>

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

```shell
curl --location --request PUT 'http://localhost:8080/api/user' \
--header 'Authorization: Bearer JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "newuser-newname"
}'
```
</details>

#### 6. Upload User photo
* **URL:** PUT /api/user/profile_pic
* **Authentication Required:** true

<details>
  <summary>Click to expand!</summary>

```shell
curl --location --request PUT 'http://localhost:8080/api/user/profile_pic' \
--header 'Authorization: Bearer JWT_TOKEN' \
--form 'file=@"/Users/siva/Downloads/images/siva.png"'
```
</details>

#### 7. Get User Profiles
* **URL:** GET /api/users
* **Authentication Required:** false
<details>
  <summary>Click to expand!</summary>

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

```shell
curl --location --request GET 'http://localhost:8080/api/users'
```
</details>

#### 8. Get User Profile by id
* **URL:** GET /api/users/:user_id
* **Authentication Required:** false
<details>
  <summary>Click to expand!</summary>

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

```shell
curl --location --request GET 'http://localhost:8080/api/users/1'
```
</details>


#### 9. Add new link
* **URL:** POST /api/links
* **Authentication Required:** true
<details>
  <summary>Click to expand!</summary>

**Request Payload:** 
```json
{
  "title": "post tile",
  "url": "url",
  "tags": ["tag1", "tag2"]
}
```

```shell
curl --location --request POST 'http://localhost:8080/api/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
	"url": "https://sivalabs.in",
	"title": "SivaLabs blog",
	"tags":[
		"spring",
		"rest-api"
	]
}'
```
</details>

#### 10. Get link
* **URL:** GET /api/links/:link_id
* **Authentication Required:** false
<details>
  <summary>Click to expand!</summary>

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

```shell
curl --location --request GET 'http://localhost:8080/api/links/21' \
--header 'Content-Type: application/json'
```
</details>

#### 11. Update link
* **URL:** PUT /api/links/:link_id
* **Authentication Required:** true
<details>
  <summary>Click to expand!</summary>

**Request Payload:** 
```json
{
  "title": "post tile",
  "url": "url",
  "tags": ["tag1", "tag2"]
}
```

```shell
curl --location --request PUT 'http://localhost:8080/api/links/1' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN' \
--data-raw '{
	"url": "https://sivalabs.in",
	"title": "SivaLabsBlog",
	"tags":[
		"springboot",
		"rest-api"
	]
}'
```
</details>

#### 12. Delete link
* **URL:** DELETE /api/links/:link_id
* **Authentication Required:** true
<details>
  <summary>Click to expand!</summary>

```shell
curl --location --request DELETE 'http://localhost:8080/api/links/21' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN'
```
</details>

#### 13. Get links for a given page number and sort by created date
* **Authentication Required:** false

##### 14. Get All Links
* **URL:** GET /api/links?page=1&size=10&sort=createdAt&direction=DESC

```shell
curl --location --request GET 'http://localhost:8080/api/links?page=1' \
--header 'Content-Type: application/json'
```

##### 15. Get links by tag
* **URL:** GET /api/links?tag=spring&page=1&size=10&sort=createdAt&direction=DESC

```shell
curl --location --request GET 'http://localhost:8080/api/links?tag=jackson' \
--header 'Content-Type: application/json'
```

##### 16. Search links
* **URL:** GET /api/links?query=spring&page=1&size=10&sort=createdAt&direction=DESC

```shell
curl --location --request GET 'http://localhost:8080/api/links?query=spring' \
--header 'Content-Type: application/json'
```

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
