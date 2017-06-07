Title: Hello, World! aah API application
Desc: Learning simple Hello World aah API application
Keywords: tutorial, hello world, api application
---
# Hello, World! aah API application

This page takes you with few steps to learn and creating a Hello World aah API application. If you have not yet installed aah framework, refer [Getting started with aah](/getting-started.html).

### Step 1

Create API application using `aah new` command.

```
jeeva@mb-pro:~$ aah new
–––––––––––––––––––––––––––––––––––––––––––––––––––––
   aah framework v0.6 -  https://aahframework.org
–––––––––––––––––––––––––––––––––––––––––––––––––––––


Welcome to interactive way to create your aah application, press ^C to exit :)

Based on your inputs, aah CLI tool generates the aah application structure for you.

Enter your application import path: api-hello-world

Choose your application type (web or api), default is 'web': api

Your aah api application was created successfully at '/Users/jeeva/go-home/src/api-hello-world'
You shall run your application via the command: 'aah run -importPath=api-hello-world'


Go to https://docs.aahframework.org to learn more and customize your aah application.

jeeva@mb-pro:~$
```

### Step 2

Open the file `$GOPATH/src/api-hello-world/app/controller/app.go` in your favorite editor and modify the greet message to `Hello World!`.

After modification greet message will look like -
```go
"Greet": models.Greet{
	Message: "Hello, World!",
},
```

### Step 3

Let's run your hello world API application.

```
aah run -ip=api-hello-world
```

Goto this URL http://localhost:8080 in your browser.

JSON response:
```javascript
{
    "message": "Hello, World!"
}
```

### Step 4

Congratulations, you have successfully created and executed the hello world API aah application.
