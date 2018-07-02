Title: aah Cryptography
Desc: aah provides ready to use and tested cryptography methods could save hours of efforts.
Keywords: crypto, cryptography, aah, AES, HMAC, HMAC SHA
---
# Cryptography

This document describes the cryptograhy methods provided by aah using Go standard libraries. Ready to use and tested, it could save hours efforts of research and implementation.

```go
import "aahframework.org/security.v0/acrypto"
```

## Encryption Methods

Method Name | Description
----------- | -----------
AESEncryptString | Encrypts text with AES. The key argument should be either 16, 24, or 32 bytes to select AES-128, AES-192, or AES-256
AESDecryptString | Decrypts text with AES. The key argument should be the one used during a encryption
AESEncrypt | Encrypts bytes with given `cipher.Block` in CTR mode
AESDecrypt | Decrypts bytes with given `cipher.Block` in CTR mode


## Sign Methods

Method Name | Description
----------- | -----------
SignString | Signs the given text using provided key with HMAC SHA. Supported SHA's are SHA-1, SHA-224, SHA-256, SHA-384, SHA-512
VerifyString | Verifies the signed text and text using provide key with HMAC SHA. Returns true if sign text is valid otherwise false
Sign | Signs a given bytes using HMAC and given SHA name. Supported SHA's are SHA-1, SHA-224, SHA-256, SHA-384, SHA-512
Verify | Verifies given key, value and mac is valid. If valid it returns true otherwise false.



