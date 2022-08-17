# Fullstack DevZone

FullstackDevZone is an approach to learn various programming languages and frameworks by building a sample application.
This idea is heavily inspired by [RealWorld](https://github.com/gothinkster/realworld).

## DevZone application

DevZone is a web application where developers can register and post their favourite article/video links.
To enable the application to be build using different programming languages and frameworks, 
the Backend and Frontend will be developed as separate applications in separate repositories. 

### Features
* Users can register and login
* Authenticated user can create a new link with a set of tags associated to it
* Authenticated user can delete own links
* Admin user can delete any link
* Any user(including guest users) can view links with pagination
  * sort by posted date desc (default)
  * by tag
  * by searching for a keyword

### Run Postman collection

Assuming the backend API application is running on http://localhost:8080/ and there are 2 users exists with following credentials:
* **Admin**: admin@gmail.com/admin
* **Normal User**: siva@gmail.com/siva

we can run smoke tests using the postman collection as follows:

```shell
$ brew install newman //MacOS
$ npm install -g newman
$ newman run devzone-api.postman_collection.json
```

### Backend API Specs

#### Authentication Header:

`Authorization: Bearer JWT_TOKEN_HERE`

#### 1. Login
* **URL:** POST /api/auth/login
* **Authentication Required:** false

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
      "role": "ROLE_USER"
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

#### 2. Registration
* **URL:** POST /api/users
* **Authentication Required:** false

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
    "role": "ROLE_USER"
}
```

#### 3. Get Current Logged-in User Info
* **URL:** GET /api/auth/me
* **Authentication Required:** true

**Success Response:** 
```json
{
    "id": 1,
    "name": "user",
    "email": "user@mail.com",
    "role": "ROLE_ADMIN"
}
```

```shell
curl --location --request GET 'http://localhost:8080/api/auth/me' \
--header 'Authorization: Bearer JWT_TOKEN_HERE'
```

#### 5. Get all tags
* **URL:** GET /api/tags
* **Authentication Required:** false

**Response Payload:**
```json
[
  {
    "id": 1,
    "name": "java"
  },
  {
    "id": 2,
    "name": "spring"
  }
]
```

```shell
curl --location --request GET 'http://localhost:8080/api/tags' \
--header 'Content-Type: application/json'
```

#### 6. Add new link
* **URL:** POST /api/links
* **Authentication Required:** true

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
--header 'Authorization: Bearer JWT_TOKEN_HERE' \
--data-raw '{
	"url": "https://sivalabs.in",
	"title": "SivaLabs blog",
	"tags":[
		"spring",
		"rest-api"
	]
}'
```

#### 7. Get link by Id
* **URL:** GET /api/links/:link_id
* **Authentication Required:** false

**Success Response:**

```json
{
  "id": 123,
  "title": "post tile",
  "url": "url",
  "tags": ["tag1", "tag2"],
  "createdBy": {
    "id": 123,
    "name": "siva"
  },
  "created_at": "",
  "updated_at": ""
}
```

```shell
curl --location --request GET 'http://localhost:8080/api/links/1' \
--header 'Content-Type: application/json'
```

#### 8. Delete link
* **URL:** DELETE /api/links/:link_id
* **Authentication Required:** true

```shell
curl --location --request DELETE 'http://localhost:8080/api/links/1' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer JWT_TOKEN_HERE'
```

##### 9. Get All Links
* **URL:** GET /api/links?page=1

```shell
curl --location --request GET 'http://localhost:8080/api/links?page=1' \
--header 'Content-Type: application/json'
```

##### 10. Get links by tag
* **URL:** GET /api/links?tag=spring&page=1

```shell
curl --location --request GET 'http://localhost:8080/api/links?tag=jackson&page=1' \
--header 'Content-Type: application/json'
```

##### 11. Search links
* **URL:** GET /api/links?query=spring&page=1

```shell
curl --location --request GET 'http://localhost:8080/api/links?query=spring&page=1' \
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
            "id": 1,
            "title": "post tile",
            "url": "url",
            "tags": ["tag1", "tag2"],
            "createdBy": {
                "id": 123,
                "name": "siva"
            }       
          },
        {
          "id": 2,
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
