Title: Password Encoders
Desc: Password Encoders (hashing algorithm) supported by aah.
Keywords: password, password encoder, authentication
---
# aah Password Encoders

Out-of-the-box aah supports three password encoders for authenticating users in your application. A good  read about [password hashing security](https://crackstation.net/hashing-security.htm).

  * [bcrypt Algorithm](#bcrypt-algorithm)
  * [scrypt Algorithm](#scrypt-algorithm)&nbsp;&nbsp;<span class="badge lb-xs">Since v0.9</span>
  * [pbkdf2 Algorithm](#pbkdf2-algorithm)&nbsp;&nbsp;<span class="badge lb-xs">Since v0.9</span>

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

<br>
**To hash your password**
```go
import "aahframework.org/security.v0"

// To hash your password
hashedPassword, err := security.Bcrypt.Generate([]byte(passwordString))
```

<br>
**Configuration**
```cfg
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

<br>
**To hash your password**
```go
import "aahframework.org/security.v0"

// To hash your password
hashedPassword, err := security.Scrypt.Generate([]byte(passwordString))
```

<br>
**Configuration**
```cfg
scrypt {
  # Default value is `false`
  enable = true

  # CPU/Memory Cost
  # Default value is `2^15`
  #cpu_memory_cost = 32768

  # Default value is `8`
  #block_size = 8

  # Default value is `1`
  #parallelization = 1

  # Default value is `32`
  #derived_key_length = 32

  # Default value is `24`
  #salt_length = 24
}
```

## pbkdf2 Algorithm

`pbkdf2` password hashing algorithm, good read [here](https://cryptosense.com/parameter-choice-for-pbkdf2/), [here](https://pthree.org/2016/06/28/lets-talk-password-hashing/), [here](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet). Configure pbkdf2 encoder in `security.conf` at section `password_encoder { ... }`.

<div class="alert alert-info-blue">
<p><strong>Note:</strong></p>
<p>Please have a look [here](https://security.stackexchange.com/questions/4781/do-any-security-experts-recommend-bcrypt-for-password-storage/6415#6415), it seems that scrypt (and bcrypt) are advised over pbkdf2 in most cases. If you use pbkdf2, using it with SHA-512 or SHA-256 is a good practice.</p>
</div>

<br>
**To hash your password**
```go
import "aahframework.org/security.v0"

// To hash your password
hashedPassword, err := security.Pbkdf2.Generate([]byte(passwordString))
```

<br>
**Configuration**
```cfg
pbkdf2 {
  # Default value is `false`
  enable = true

  # Default value is `10000`
  #iteration = 10000

  # Default value is `32`
  #derived_key_length = 32

  # Default value is `24`
  #salt_length = 24

  # Supported SHA's are `sha-1`, `sha-224`, `sha-256`, `sha-384`, `sha-512`.
  # Default value is `sha-512`
  #hash_algorithm = "sha-512"
}
```
