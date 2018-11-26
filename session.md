Title: Session Management
Desc: aah session provides HTTP state management for web applications and stateless session for API applications. aah session is HMAC signed and AES encrypted. aah customizes session storage via interface `session.Storer`.
Keywords: session, stateless session, stateful session, http state management, hmac signed, aes encrypted, external storage
---
# Session Management

aah session library provides HTTP state management for web applications and stateless session for API applications.

**Features:**

  * HMAC Signed session data
  * AES Encrypted session data
  * Extensible `session.Storer` interface

aah provides ready-to-use `Cookie` and `File` session store to persist signed and encrypted session data. For custom session store (Key-Value Database, NoSQL Database, RDBMS, etc.), implement interface `session.Storer` and register in file `<app-base-dir>/app/init.go` (refer `session.FileStore` implementation; it is very easy to follow).

<div class="alert alert-info-green">
<p><strong>Note:</strong> In non-cookie session store, only <code>Session ID</code> is transmitted over the wire via Cookie.</p>
</div>

To add values of custom data types in the session, register them using `gob.Register(...)`.

### Table of Contents

  * [How to access current Session?](#how-to-access-current-session)
  * [Adding User-Defined Store into aah](#adding-user-defined-session-store-into-aah)
  * [Session Configuration](security-config.html#section-session)

## How to access current session?

Current session can be accessed via `ctx.Session()`.

## Adding user-defined session store into aah

Steps to add user-defined session store into aah:

  1. Implement interface `session.Storer` (Refer `session.FileStore`).
  2. Register it in aah at `<app-base-dir>/app/init.go` file.
  3. Configure it in app session config.

### Step 1: Implement interface `session.Storer`

```go
//Implement interface `session.Storer` for custom session storage
type Storer interface {
	Init(appCfg *config.Config) error
	Read(id string) string
	Save(id, value string) error
	Delete(id string) error
	IsExists(id string) bool
	Cleanup(m *Manager)
}
```


### Step 2: Add the newly created custom session store into aah

```go
// Refer `session.FileStore` for implementation
func init() {
  aah.App().AddSessionStore("redis", &RedisSessionStore{})
}
```

### Step 3: Configure the added custom session store in the config file `security.conf`

```bash
security {
  session {
    # ....

    store {
      type = "redis"
    }

    # ....
  }
}
```

Read more about [authentication](authentication.html) and [authorization](authorization.html). 
