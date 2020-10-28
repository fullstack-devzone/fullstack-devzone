# Fullstack DevZone

FullstackDevZone is an approach to learn various programming languages and frameworks by building a sample application.
This idea is heavily inspired by [RealWorld](https://github.com/gothinkster/realworld).

## DevZone application

DevZone is a web application where developers can register and post their favourite article/video links.
To enable the application to be build using different programming languages and frameworks, 
the Backend and Frontend will be developed as separate applications in separate repositories. 

### Features
* Authentication
    * Users can register, login, change password and reset password
* User Profile
    * Provide name, email, bio, photo, tech skills, social media accounts etc
    * Change profile pic
    * Subscribe to certain categories to receive daily or weekly Notification Emails 
      which contain all new links in their following categories 
* Any User Actions
    * Any user(including guest users) can view links of a category with pagination, sort by posted date/upvotes
* Authenticated User Actions
    * Submit a new article/video link with a category and set of tags associated to it
    * Upvote a link and remove vote
* Admin User Actions
    * Mark a link as Spam Or Invalid
    * Delete a link

### Backend API Specs

#### 1. Login
**URL:** POST /api/auth/login

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
  "token": "jwt_token",
  "expires_at": "expiry_timestamp",
  "user":  {
      "name": "user",
      "username": "user@mail.com",
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

**Request Payload:** 
```json
{
  "name": "name",
  "username": "user@mail.com",
  "password": "password"
}
```

#### 3. Activate User Registration by Email
**URL:** GET /api/user/activate?email=user@mail.com&code=unique_code

#### 4. Change Password
**URL:** POST /api/user/change-password

**Request Payload:** 

```json
{
  "username": "user@mail.com",
  "old_password": "old_password",
  "new_password": "new_password"
}
```

#### 5. Forgot Password
**URL:** POST /api/user/forgot-password

**Request Payload:** 
```json
{
  "username": "user@mail.com"
}
```

#### 6. Reset Password
**URL:** POST /api/user/reset-password

**Request Payload:** 
```json
{
  "username": "user@mail.com"
}
```

#### 7. Get Login User Info
**URL:** GET /api/user

**Success Response:** 
```json
{
    "name": "user",
    "username": "user@mail.com",
    "roles": ["ROLE_ADMIN", "ROLE_USER"]
}
```

#### 8. Update User Profile
**URL:** PUT /api/user

**Request Payload:** 
```json
{
    "name": "user",
    "bio": "",
    "location": "Hyderabad, India",
    "skills": ["Java", "Kotlin", "SpringBoot", "Docker"],
    "github": "https://github.com/sivaprasadreddy",
    "twitter": "https://twitter.com/sivalabs",
    "email_notifications": "none|daily|weekly",
    "favourite_categories": ["Java", "ReactJS"]
}
```

#### 9. Upload User photo
**URL:** POST /api/user/photo

#### 10. Get User Profiles
**URL:** GET /api/users

**Success Response:** 
```json
[
    {
      "name": "user",
      "email": "user@mail.com",
      "bio": "",
      "location": "Hyderabad, India",
      "skills": ["Java", "Kotlin", "SpringBoot", "Docker"],
      "github": "https://github.com/sivaprasadreddy",
      "twitter": "https://twitter.com/sivalabs",
      "email_notifications": "none|daily|weekly",
      "favourite_categories": ["Java", "ReactJS"]
    }
]
```

#### 11. Get User Profile by Email
**URL:** GET /api/users/user@gmail.com

**Success Response:** 
```json
{
  "name": "user",
  "email": "user@mail.com",
  "bio": "",
  "location": "Hyderabad, India",
  "skills": ["Java", "Kotlin", "SpringBoot", "Docker"],
  "github": "https://github.com/sivaprasadreddy",
  "twitter": "https://twitter.com/sivalabs",
  "email_notifications": "none|daily|weekly",
  "favourite_categories": ["Java", "ReactJS"]
}
```

#### 12. Add new link
**URL:** POST /api/:category/links

**Request Payload:** 
```json
{
  "title": "post tile",
  "url": "url",
  "type" : "article|video",
  "tags": ["tag1", "tag2"]
}
```

#### 13. Get link
**URL:** GET /api/links/:link_id

**Success Response:**

```json
{
  "title": "post tile",
  "url": "url",
  "type" : "article|video",
  "category": "category_name",
  "tags": ["tag1", "tag2"]
}
```

#### 14. Update link
**URL:** PUT /api/links/:link_id

**Request Payload:** 
```json
{
  "title": "post tile",
  "url": "url",
  "type" : "article|video",
  "category": "category_name",
  "tags": ["tag1", "tag2"]
}
```

#### 15. Delete link
**URL:** DELETE /api/links/:link_id

#### 16. Get links of a category for a given page number and sort by posted_date or votes

**URL:** GET /api/:category/links?page=1&sort_by=posted_date&sort_order=desc

**Success Response:** 
```json
[
      {
        "title": "post tile",
        "url": "url",
        "type" : "article|video",
        "category": "category_name",
        "tags": ["tag1", "tag2"]
      },
    {
      "title": "post tile",
      "url": "url",
      "type" : "article|video",
      "category": "category_name",
      "tags": ["tag1", "tag2"]
    }
]
```

#### 17. User upvote/remove vote for a link

**URL:** POST /api/links/:link_id/votes

**Request Payload:** 
```json
{
   "vote": "true|false"
}
```

#### 18. Admin marks a link as spam or invalid

**URL:** PUT /api/links/:link_id

**Request Payload:** 
```json
{
  "status": "invalid | spam"
}
```

## Guidelines
* Implement unit and integration tests
* Decent code coverage is nice to have
* Implement continuous integration CI using GitHub Actions or Travis
* Postman collection for API is nice to have
