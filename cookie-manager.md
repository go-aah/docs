Title: aah Cookie Manager
Desc: aah provides a secure cookie manager
Keywords: secure cookie manager, cookie manager, cookie
---
# Cookie Manager

aah provides a secure cookie manager to fulfill custom secure cookie purpose. It does AES encryption and HMAC SHA signing.

```go
import "aahframework.org/security.v0/cookie"
```

### Table of Contents

* [Create a secure Cookie Manager](#create-a-secure-cookie-manager)
* [Reply Cookie via Reply Builder](#reply-cookie-via-reply-builder)

## Create a secure Cookie Manager

A sample code snippet to create a secure cookie manager is below.  For non-secure cookie manager, simply supply `signKey`.

```go
// Create cookie options
opts := &cookie.Options{
    Name: "mycustomcookie",
    Domain; "aahframework.org",
    Path: "/",
    MaxAge: 2629746, // 1 month in seconds
    HTTPOnly: true,
    Secure: true, // use only SSL enabled website
}

// Get sign and encryption keys from configuration
signKey := aah.App().Config().StringDefault("myapp.mysecure.cookie.key.sign", ess.SecureRandomString(32))
encKey := aah.App().Config().StringDefault("myapp.mysecure.cookie.key.enc", ess.SecureRandomString(64))

// Creating a secure cookie manager
cookieMgr := cookie.NewManager(opts, signKey, encKey)
```

## Reply Cookie via Reply Builder

Using secure cookie manager, manipulating cookie value is a breeze.

#### Creating new cookie

```go
// create and reply
httpCookie := cookieMgr.New("This is my secure cookie value")
c.Reply().Cookie(httpCookie)
```

#### Decode Cookie value from Request

```go
func (c *ProductController) Show(id string) {

    cookieValue := c.Req.Cookie("cookiename")
    b, err := cookieMgr.Decode(cookieValue)
    if err != nil {
        // invalid cookie
        c.Session().SetFlash("cookie.error", "Invalid cookie")        
        c.Reply().BadRequest().HTMLf("/errors/cookie.html", nil)
        return
    }

    value := string(b)
    c.Log().Info("Cookie value: ", value)

}
```
