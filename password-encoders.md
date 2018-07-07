Title: Password Encoders
Desc: Password Encoders (hashing algorithm) supported by aah.
Keywords: password, password encoder, authentication
---
# aah Password Encoders

Out-of-the-box aah supports three password encoders for authenticating users in your application. A good  read about [password hashing security](https://crackstation.net/hashing-security.htm).

  * [bcrypt Algorithm](#bcrypt-algorithm)
  * [scrypt Algorithm](#scrypt-algorithm)
  * [pbkdf2 Algorithm](#pbkdf2-algorithm)
  * [Adding additional password encoder into aah](#adding-additional-password-encoder-into-aah)

Password encoders implements the interface `PasswordEncoder`.

```go
// PasswordEncoder interface is used to implement generate password hash and compare given hash & password
// based chosen hashing type. Such as `bcrypt`, `scrypt` and `pbkdf2`.
//
// Good read about hashing security https://crackstation.net/hashing-security.htm
type PasswordEncoder interface {
	Generate(password []byte) ([]byte, error)
	Compare(hash, password []byte) bool
}
```

## bcrypt Algorithm

`bcrypt` password hashing algorithm, good read [here](https://security.stackexchange.com/a/6415), [here](https://en.wikipedia.org/wiki/Bcrypt). Configure bcrypt encoder in `security.conf` at section `password_encoder { ... }`.

#### To hash your password

```go
import "aahframework.org/security.v0"

// To hash your password
hashedPassword, err := security.Bcrypt.Generate([]byte(passwordString))
```

#### Configuration

```bash
bcrypt {
  # Default value is `true`
  enable = true

  # https://godoc.org/golang.org/x/crypto/bcrypt#pkg-constants
  # Default value is `12`.
  cost = 12
}
```

## scrypt Algorithm

`scrypt` password hashing algorithm, good read [here](https://pthree.org/2016/06/28/lets-talk-password-hashing/), [here](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet). Configure scrypt encoder in `security.conf` at section `password_encoder { ... }`.

#### To hash your password

```go
import "aahframework.org/security.v0"

// To hash your password
hashedPassword, err := security.Scrypt.Generate([]byte(passwordString))
```

#### Configuration

```bash
scrypt {
  # Default value is `false`
  enable = true

  # CPU/Memory Cost
  # Default value is `2^15`
  cpu_memory_cost = 32768

  # Default value is `8`
  block_size = 8

  # Default value is `1`
  parallelization = 1

  # Default value is `32`
  derived_key_length = 32

  # Default value is `24`
  salt_length = 24
}
```

## pbkdf2 Algorithm

`pbkdf2` password hashing algorithm, good read [here](https://cryptosense.com/parameter-choice-for-pbkdf2/), [here](https://pthree.org/2016/06/28/lets-talk-password-hashing/), [here](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet). Configure pbkdf2 encoder in `security.conf` at section `password_encoder { ... }`.

<div class="alert alert-info-blue">
<p><strong>Note:</strong></p>
<p>It's commonly recommended to use `bcrypt` password hashing algorithm. However real world usage different per application. If you're using `pbkdf2` hashing algorithm, it's highly advised to use pbkdf2 with SHA-512 or SHA-256. Good read <a href="https://security.stackexchange.com/questions/4781/do-any-security-experts-recommend-bcrypt-for-password-storage/">here</a>, <a href="https://crypto.stackexchange.com/questions/15218/is-pbkdf2-hmac-sha1-really-broken">here</a>.</p>
</div>

#### To hash your password

```go
import "aahframework.org/security.v0"

// To hash your password
hashedPassword, err := security.Pbkdf2.Generate([]byte(passwordString))
```

#### Configuration

```bash
pbkdf2 {
  # Default value is `false`
  enable = true

  # Default value is `10000`
  iteration = 10000

  # Default value is `32`
  derived_key_length = 32

  # Default value is `24`
  salt_length = 24

  # Supported SHA's are `sha-1`, `sha-224`, `sha-256`, `sha-384`, `sha-512`.
  # Default value is `sha-512`
  hash_algorithm = "sha-512"
}
```

## Adding additional password encoder into aah

aah provides extensibility to add additional password encoder into aah easily. Implement the interface `acrypto.PasswordEncoder` then add it to `aah`.

#### Registering password encoder

```go
// Choose whichever the argon2 library and implement interface `acrypto.PasswordEncoder`
// then register it here.
func init()  {
  aah.AddPasswordAlgorithm("argon2", &Argon2Encoder{})
}
```

#### Using registered encoder in auth schemes

```bash
# In your auth scheme, simply mention the name you have used for the registering. That's it very easy!
form_auth {
  #...
  password_encoder = "argon2"
  #...
}
```
