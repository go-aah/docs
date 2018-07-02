Title: aah Cookie Manager
Desc: aah provides secure Cookie manager 
Keywords: secure cookie manager, cookie manager, cookie
---
# Cookie Manager

aah provides secure Cookie Manager for custom secure cookie purpose. It does AES encryption and HMAC SHA signing.

```go
import "aahframework.org/security.v0/cookie"
```

### Table of Contents

* [Create secure Cookie Manager](#create-secure-cookie-manager)
* [Reply Cookie via Reply Builder](#reply-cookie-via-reply-builder)

## Create secure Cookie Manager

Sample code snippet to create secure cookie manager.  For non-secure cookie manager supply only `signKey`.

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
signKey := aah.AppConfig().StringDefault("myapp.mysecure.cookie.key.sign", ess.SecureRandomString(32))
encKey := aah.AppConfig().StringDefault("myapp.mysecure.cookie.key.enc", ess.SecureRandomString(64))

// Creating secure cookie manager
cookieMgr := cookie.NewManager(opts, signKey, encKey)
```

## Reply Cookie via Reply Builder

Using secure cookie manager, manipulating cookie value is breeze.

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
