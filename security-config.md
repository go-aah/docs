Title: aah Security Configuration
Desc: aah Security configuration to configure Session Management, Authentication (upcoming),  CORS (upcoming), CSRF (upcoming), Security Headers (upcoming), etc.
Keywords: security config, security configuration, session config, auth config, cors, csrf, HOCON
---
# aah Security Configuration

aah Security configuration is to configure Authentication, Authorization, Session Management, CORS **`(upcoming)`**, CSRF **`(upcoming)`**, Security Headers **`(upcoming)`**, etc. The configuration syntax is used by aah framework is very similar to HOCON syntax. To learn more about **[configuration syntax](configuration.html)**.

Reference to [App Config](app-config.html), [Routes Config](routes-config.html), [Log Config](log-config.html).

### Table of Contents

* [security { ... }](#section-security)
  - [auth_schemes { ... }](authentication.html#auth-schemes)
      - [form auth](authentication.html#form-based-auth)
      - [basic auth](authentication.html#basic-auth)
      - [generic auth](authentication.html#generic-auth)
  - [session { ... }](#section-session)

## Section: security { ... }
To configure application security related configuration in the section.

### Section: auth_schemes { ... }
`auth_schemes` section is used to configure application authentication and authorization. Out-of-the-box aah framework supports following schemes-

  * [FormAuth](authentication.html#form-based-auth) - Register your own dynamic integration of Authentication and Authorization.
  * [BasicAuth](authentication.html#basic-auth) - File realm or implement your own dynamic integration of Authentication and Authorization.
  * [GenericAuth](authentication.html#generic-auth) - brings more possibilities.

### Section: session { ... }
HTTP state management across HTTP requests.

### mode
Session mode is to choose whether HTTP session should be persisted or destroyed at the end of each request. Supported values are -

* `stateless` - Session data is destroyed at end of each request
* `stateful` - Session data is persisted based on store type config

Default value is `stateless` for API and `stateful` for Web application.
```cfg
mode = "stateful"
```

### Section: store { ... }
Session store is to configure where session values should be persisted. Currently aah framework supports `cookie` and `file` as a store type. Also framework provides extensible `session.Storer` interface to add your custom session store.

### type
Currently aah framework supports `cookie` and `file` as a store type.

_**Note:** If you're using cookie session store, you have to be mindful about cookie size limit `4kb`._

Default store type value is `cookie`.
```cfg
type = "cookie"
```

### filepath
Filepath is used for file store to store session data in the file system. This is only applicable for `store.type = "file"`, make sure application has Read/Write access to the directory. Provide absolute path.

Default value is `<app-base-dir>/sessions`.
```cfg
filepath = "/path/to/store/session/files"
```

### id_length
Session Identifier length. Identifier(ID) is generated using random bytes from `crypto/rand` and `HEX` encoding.

Default value is `32`
```cfg
id_length = 32
```

### ttl
Time-to-live value for session data expiry. Valid time units are `m -> minutes`, `h -> hours` and `0`.

Default value is `0`, cookie is deleted when the browser is closed.
```cfg
ttl = "0"
```

### prefix
HTTP session cookie name prefix value.

Default value is `aah` For e.g.: `aah_session`.
```cfg
prefix = "aah"
```

### domain
HTTP session cookie domain value.

Default value is `empty` string.
```cfg
domain = ""
```

### path
HTTP session cookie path value.

Default value is `/`.
```cfg
path = "/"
```

### http_only
HTTP session cookie HTTPOnly value. This option is to prevents XSS (Cross Site Scripting) attacks, basically it disallows access of cookie to scripts like JavaScript.

Default value is `true`.
```cfg
http_only = true
```

### secure
HTTP session cookie secure value. However, if aah server is not configured with SSL then aah framework sets this value as `false`.

Default value is `true`.
```cfg
secure = true
```

### sign_key
HTTP session cookie value signing using `HMAC`. For server farm this value should be same in all instance. For HMAC sign & verify it is recommend to use key size is `32` or `64` bytes.

Default value is `64` bytes (generated when application gets created using `aah new` command).
```cfg
sign_key = "generated-value"
```

### enc_key
HTTP session cookie value encryption and decryption using `AES`. For server farm this value should be same in all the instances. AES algorithm is used, valid lengths are `16`, `24`, or `32` bytes to select `AES-128`, `AES-192`, or `AES-256`.

Default value is `32` bytes (generated when application gets created using `aah new` command).
```cfg
enc_key = "generated-value"
```

### cleanup_interval
Cleanup Interval is used to clean the expired session data from the store. This is only applicable for non-cookie store type. Cleanup performed in dedicated goroutine. Valid time units are `m -> minutes`, `h -> hours`.

Default value is `30m`.
```cfg
cleanup_interval = "30m"
```
