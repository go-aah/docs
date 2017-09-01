Title: Tutorial of Form Submission
Desc: How to do Form Submission using aah framework
Keywords: form submission, form tutorial, aah framework
---
# Tutorial of Form Submission

Goal of this tutorial is to demonstrate Form Submission using aah framework. That's includes Auto parse and bind (nested struct bind and input sanitization to prevent XSS attacks).

Take a moment to learn about clean, robust implementation of [auto parse and bind](/request-parameters-auto-bind.html) capabilities.

### How to get the aah tutorials source code?

```cfg
go get -u -d github.com/go-aah/tutorials
```

<br>
Focus on following files/directory:
```cfg
  form/app/controllers/app.go
  form/views/pages/app/index.html
  form/views/pages/app/userprofile.html
  form/config/routes.conf
```

### Explanation

  * `controllers` package
      - `AppController` has implementation of `UserProfileSubmit`, which receives the submitted form values.
  * `views` directory
      - Implements pages for collecting form-data and displays the submitted form-data in `userprofile.html`.
  * `routes.conf` has routes defined for index page, display user profile form page and to receive form data submission on server side.

### Let's see it in the action

```cfg
aah run -i github.com/go-aah/tutorials/form
```

#### Now visit this URL in your browser :)

  * http://localhost:8080

Page includes details about the implemented functionality and button to go to User Profile - Form Submission. It is self explanatory.

Fill out the form fields and submit. As a response, submitted field values displayed on the page.

<br><br>
<center>**Spread the word of `aah`, the web framework for Go. Thank you!**</center>
