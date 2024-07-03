# Fullstack DevZone

FullstackDevZone is an approach to learning various programming languages and frameworks by building a sample application.
This idea is heavily inspired by [RealWorld](https://github.com/gothinkster/realworld).

## DevZone application

DevZone is a web application where developers can register and post their favourite article/video as posts.
To enable the application to be build using different programming languages and frameworks, 
the Backend and Frontend will be developed as separate applications in separate repositories. 

### Features
* Users can register and login
* Authenticated user can create a new post
* Authenticated users can delete their own posts
* Admin user can delete any post
* Any user(including guest users) can view posts with pagination
  * sort by posted date desc (default)
  * by searching for a keyword

### Run Postman collection

Assuming the backend API application is running on http://localhost:8080/ and 
there are two users exists with the following credentials:

* **Admin**: admin@gmail.com/admin
* **Normal User**: siva@gmail.com/siva

we can run smoke tests using the Postman collection as follows:

```shell
$ brew install newman //MacOS
$ npm install -g newman
$ newman run devzone-api.postman_collection.json
```

### Backend API Specs

#### 1. Login
* **URL:** `POST /api/login`

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
curl -X POST 'http://localhost:8080/api/login' \
-H 'Content-Type: application/json' \
-d '{
	"username": "admin@gmail.com",
	"password": "admin"
}'
```

#### 2. Registration
* **URL:** `POST /api/users`

**Request Payload:** 
```json
{
  "name": "sivaprasad",
  "email": "sivaprasad@gmail.com",
  "password": "password"
}
```

```shell
curl -X POST 'http://localhost:8080/api/users' \
-H 'Content-Type: application/json' \
-d '{
	"name":"newuser",
	"email": "newuser@gmail.com",
	"password": "secret"
}'
```

**Success Response:** 
```json
{
    "id": 2,
    "name": "newuser",
    "email": "newuser@gmail.com",
    "role": "ROLE_USER"
}
```

#### 3. Create a new post
* **URL:** `POST /api/posts`
* `Authorization: Bearer JWT_TOKEN_HERE`

**Request Payload:** 
```json
{
  "title": "post tile",
  "url": "url",
  "content": "content"
}
```

```shell
curl -X POST 'http://localhost:8080/api/posts' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer JWT_TOKEN_HERE' \
-d '{
	"url": "https://sivalabs.in",
	"title": "SivaLabs blog",
	"content":"My experiments with technology"
}'
```

#### 4. Get post by PostId
* **URL:** `GET /api/posts/:postId`
* `Authorization: Bearer JWT_TOKEN_HERE`

**Success Response:**

```json
{
  "id": 123,
  "title": "post tile",
  "url": "url",
  "content": "content",
  "createdBy": {
    "id": 123,
    "name": "siva"
  },
  "createdAt": "",
  "updatedAt": ""
}
```

```shell
curl -X GET 'http://localhost:8080/api/posts/1'
```

#### 5. Delete a post by PostId
* **URL:** DELETE /api/posts/:postId
* **Authentication Required:** true

```shell
curl -X DELETE 'http://localhost:8080/api/posts/1' \
-H 'Authorization: Bearer JWT_TOKEN_HERE'
```

##### 6. Get Posts
* **URL:** `GET /api/posts?page=1&query=keyword`

```shell
curl -X GET 'http://localhost:8080/api/posts?page=1' \
-H 'Content-Type: application/json'
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
            "content": "content",
            "createdBy": {
                "id": 123,
                "name": "siva"
            }       
          },
        {
          "id": 2,
          "title": "post tile",
          "url": "url",
          "content": "content",
          "createdBy": {
                "id": 456,
                "name": "prasad"
            } 
        }
    ]
}
```

#### 7. Get Current Logged-in User Info (Optional)
* **URL:** `GET /api/me`
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
curl -X GET 'http://localhost:8080/api/me' \
-H 'Authorization: Bearer JWT_TOKEN_HERE'
```

#### 8. Get User by id (Optional)
* **URL:** `GET /api/users/:id`

**Success Response:**
```json
{
    "id": 1,
    "name": "user",
    "email": "user@mail.com",
    "role": "ROLE_USER"
}
```

```shell
curl -X GET 'http://localhost:8080/api/users/1'
```

## Guidelines
* Implement unit and integration tests
* Decent code coverage is nice to have
* Implement continuous integration CI using GitHub Actions or CircleCI etc
