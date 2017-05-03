Title: Session Management
Desc: aah Session provides HTTP state management for web application and Stateless Session for API application. HMAC Signed session data, AES Encrypted session data, Extensible session.Storer interface.
Keywords: session, stateless session, stateful session, http state management, hmac signed, aes encrypted, external store
---
# Session Management

aah Session library provides HTTP state management for web application and Stateless Session for API application.

**Features:**

  * Extensible `session.Storer` interface
  * HMAC Signed session data
  * AES Encrypted session data

Out-of-the-box aah framework provides `Cookie` and `File` as a Session Store to persist encoded session data. Also it provides extensible `session.Storer` for adapting a other storages like Key-Value Database, NoSQL Database, and RDBMS. For implementation sample please refer `session.FileStore`; it's very easy.

Non-cookie store session data is maintained via store interface. Only `Session ID` is transmitted over the wire in the Cookie.

If you would like to store custom types in session then Register your custom types using `gob.Register(...)`.

Reference to [Session Configuration](security-config.html#section-session).

### Table of Contents

  * [`session.Storer` Interface](#session-storer-interface)
  * [Adding User-Defined Store into aah](#adding-user-defined-store-into-aah)
  * [Configuring User-Defined Store into aah](#configuring-user-defined-store-into-aah)

## `session.Storer` Interface

```go
// Storer is interface for implementing pluggable storage implementation.
Storer interface {
	Init(appCfg *config.Config) error
	Read(id string) string
	Save(id, value string) error
	Delete(id string) error
	IsExists(id string) bool
	Cleanup(m *Manager)
}
```

## Adding User-Defined Session Store into aah

Add the user-defined custom session store into aah framework.
```go
// Refer `session.FileStore` for implementation sample
func init() {
  aah.AddSessionStore("redis", &RedisSessionStore{})
}
```

## Configuring User-Defined Session Store into aah

Configuring user-defined custom store for session data storage in the `security.conf`.
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
