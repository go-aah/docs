Title: Hello, World! aah Web application
Desc: Learning simple Hello World aah web application
Keywords: tutorial, hello world, web application
---
# Hello, World! aah Web application

This page takes you with few steps to learn and creating a Hello World aah web application. If you have not yet installed aah framework, refer [Getting started with aah](/getting-started.html).

### Step 1

Create web application using `aah new` command.

```
jeeva@mb-pro:~$ aah new
–––––––––––––––––––––––––––––––––––––––––––––––––––––
   aah framework v0.7 -  https://aahframework.org
–––––––––––––––––––––––––––––––––––––––––––––––––––––
# Report improvements/bugs at https://github.com/go-aah/aah/issues


Welcome to interactive way to create your aah application, press ^C to exit :)

Based on your inputs, aah CLI tool generates the aah application structure for you.

Enter your application import path: web-hello-world

Choose your application type (web or api), default is 'web': web

Choose your application Auth Scheme (form, basic), default is 'none': none

Choose your session store (cookie or file), default is 'cookie': cookie

Your aah web application was created successfully at '/Users/jeeva/go-home/src/web-hello-world'
You shall run your application via the command: 'aah run --importpath web-hello-world'


Go to https://docs.aahframework.org to learn more and customize your aah application.

jeeva@mb-pro:~$
```

### Step 2

Open the file `$GOPATH/src/web-hello-world/app/controller/app.go` in your favorite editor and modify the greet message to `Hello World!`.

After modification greet message will look like -
```go
"Greet": models.Greet{
	Message: "Hello, World!",
},
```

### Step 3

Let's run your hello world Web application.

```
aah run -i web-hello-world
```

Goto this URL http://localhost:8080 in your browser.

### Step 4

Congratulations, you have successfully created and executed the hello world Web aah application.

Spread the word - aah web framework for Go.
