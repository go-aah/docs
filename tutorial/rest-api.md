Title: Tutorial of REST API Services
Desc: How to do REST API services using aah framework
Keywords: file upload, multipart file upload, aah framework
---
# Tutorial of REST API Services

Goal of this tutorial is to demonstrate REST API services using aah framework. It gives you an idea about flexibility and capability out-of-the-box features.

Take a moment to learn about clean, robust implementation of [auto parse and bind](/request-parameters-auto-bind.html) capabilities. Which transforms the request body of JSON and XML content into `struct` along with request parameters.

### How to get the aah tutorials source code?

```bash
go get -u -d github.com/go-aah/tutorials
```

<br>
Focus on following files/directory:
```cfg
  rest-api/app/controllers/app.go
  rest-api/app/controllers/post.go
  rest-api/config/routes.conf
```

### Explanation

  * `controllers` package
      - Implements welcome message endpoint and blog post endpoints.
  * `routes.conf` has routes definition for `/`, `/v1/posts`, `/v1/posts/*`.

### Let's see it in the action

```cfg
aah run -i github.com/go-aah/tutorials/rest-api
```

### Use your favorite REST client to make a request

Tutorial application has following endpoints with in-memory blog post APIs implementation. Take a look at application logs too.

  * GET&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8080/ - Welcome message.
  * POST&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8080/v1/posts - Creates a simple blog post with `Title` and `Body`.
  * GET&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8080/v1/posts/:id - Get blog post content by ID.
  * PUT&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8080/v1/posts/:id - Update blog post content by ID with `Title` and `Body`.
  * DELETE http://localhost:8080/v1/posts/:id - Delete blog post by ID.
  * GET&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;http://localhost:8080/v1/posts - Get all the blog posts.

##### Welcome Message

Send GET request to http://localhost:8080/

Response:
```json
{
  "message": "Welcome to aah framework - REST API Services tutorial"
}
```

##### Create a Blog Post

Send POST request to http://localhost:8080/v1/posts

Header: Content-Type -> application/json or text/json

Request Body:
```json
{
    "title": "Hey, my first post",
    "body": "<p>This is my first blog post</p>",
}
```

##### Get Blog Post by ID

Send GET request to http://localhost:8080/v1/posts/1

Response:
```json
{
    "id": 1,
    "title": "Hey, my first post",
    "body": "<p>This is my first blog post</p>",
    "created_at": "2017-08-31T13:10:50-07:00",
    "updated_at": "2017-08-31T13:10:50-07:00"
}
```

#### Get All Blog Posts

Send GET request to http://localhost:8080/v1/posts

Response:
```json
{
    "posts": [
        {
            "id": 1,
            "title": "Hey, my first post",
            "body": "<p>This is my first blog post</p>",
            "created_at": "2017-08-31T13:10:50-07:00",
            "updated_at": "2017-08-31T13:14:15-07:00"
        }
    ]
}
```

#### Update Blog Post by ID

Send PUT request to http://localhost:8080/v1/posts/1

Header: Content-Type -> application/json or text/json

Request Body:
```json
{
    "title": "Hey, my first post - update",
    "body": "<p>This is my first blog post - update</p>"
}
```
Response: 204 No Content

##### Delete Blog Post by ID
Send PUT request to http://localhost:8080/v1/posts/1

Response: 204 No Content

<br><br>
