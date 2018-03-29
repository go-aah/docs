Title: Tutorial of Multipart File Upload
Desc: How to do Multipart File Upload using aah framework
Keywords: file upload, multipart file upload, aah framework
---
# Tutorial of Multipart File Upload

Goal of this tutorial is to demonstrate Multipart File Upload using aah framework.

It's easy to handle file, on server side you just call method `Req.SaveFile` with form field name and destination path. Method returns file size on successful save. That's it!

### How to get the aah tutorials source code?

```bash
go get -u -d github.com/go-aah/tutorials
```

<br>
Focus on following files/directory:
```cfg
  form-fileupload/app/controllers/app.go
  form-fileupload/views/pages/app/index.html
  form-fileupload/views/pages/app/fileupload.html
  form-fileupload/config/routes.conf
```

### Explanation

  * `controllers` package
      - `AppController` has implementation of `FileUpload` action, which saves uploaded file into file system.
  * `views` directory
      - Implements pages for choosing file and upload then displays the uploaded file details (such as request file name, saved file name, file size in bytes).
  * `routes.conf` has route defined for index page and file upload.

### Let's see it in the action

```cfg
aah run -i github.com/go-aah/tutorials/form-fileupload
```

#### Now visit this URL in your browser :)

  * http://localhost:8080

Page includes details about the implemented functionality and button to choose file and upload. It is self explanatory.

<br><br>
