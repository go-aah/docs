# aah Security Configuration

aah Security configuration to configure Session, CORS **`(upcoming)`**, CSRF **`(upcoming)`**, Security Headers **`(upcoming)`**, etc. The configuration syntax is used by aah framework is similar to HOCON syntax, not 100%. To learn more about **[configuration syntax](configuration.html)**.

Reference to [App Config](app-config.html), [Routes Config](routes-config.html), [Logging Config](logging-config.html).

### Table of Contents

* [security { ... }](#section-security)
  - [session { ... }](#section-session)

## Section: security { ... }
To configure application security related configuration in the section.

### Section: session { ... }
HTTP state management across multiple requests.

### mode
Session mode is to choose whether HTTP session should be persisted or destroyed at the end of each request. Supported values are -

* `stateless` - Session data is destroyed at end of each request
* `stateful` - Session data is persisted based on store type config

Default value is `stateless`
```bash
mode = "stateful"
```

### Section: store { ... }
Session store is to choose where session values should be persisted. Currently aah framework supports `cookie` and `file` as a store type. Also framework provide extensible `session.Storer` interface to add custom session store.

### type
Currently aah framework supports `cookie` and `file` as a store type.

Default store type value is `cookie`
```bash
type = "cookie"
```

### filepath
Filepath is used for file store to store session data in the file system. This is only applicable for `type="file"`, make sure application has Read/Write access to the directory. Provide absolute path.

No default value, it is mandatory for file system type store.
```bash
filepath = "/path/to/store/session/files"
```

### id_length
Session Identifier length. Identifier(ID) is generated using random bytes from `crypto/rand` and `HEX` encoding.

Default value is `32`
```bash
id_length = 32
```

### ttl
Time-to-live value for session data expiry. Valid time units are `m -> minutes`, `h -> hours` and `0`.

Default value is `0`, cookie is deleted when the browser is closed.
```bash
ttl = "0"
```

### prefix
HTTP session cookie name prefix value.

Default value is `aah` For e.g.: `aah_session`.
```bash
prefix = "aah"
```

### domain
HTTP session cookie domain value.

Default value is `empty` string.
```bash
domain = ""
```

### path
HTTP session cookie path value.

Default value is `/`.
```bash
path = "/"
```

### http_only
HTTP session cookie HTTPOnly value. This option prevents XSS (Cross Site Scripting) attacks, basically it disallows access of cookie to scripts like JavaScript.

Default value is `true`.
```bash
http_only = true
```

### secure
HTTP session cookie secure value. However, if aah server is not configured with SSL then aah framework sets this value as `false`.

Default value is `true`.
```bash
secure = true
```

### sign_key
HTTP session cookie value signing using `HMAC`. For server farm this value should be same in all instance. For HMAC sign & verify it is recommend to use key size is `32` or `64` bytes.

Default value is `64` bytes (generated when application gets created using `aah new` command).
```bash
sign_key = "generated-value"
```

### enc_key
HTTP session cookie value encryption and decryption using `AES`. For server farm this value should be same in all instance. AES algorithm is used, valid lengths are `16`, `24`, or `32` bytes to select `AES-128`, `AES-192`, or `AES-256`.

Default value is `32` bytes (generated when application gets created using `aah new` command).
```bash
enc_key = "generated-value"
```

### cleanup_interval
Cleanup Interval is used to clean the expired session objects from store. This is only applicable for non-cookie store type. Cleanup performed in dedicated goroutine. Valid time units are `m -> minutes`, `h -> hours`.

Default value is `30m`.
```bash
cleanup_interval = "30m"
```
