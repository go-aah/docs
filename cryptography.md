Title: aah Cryptography
Desc: aah provides ready-to-use and well-tested cryptography methods that save hours of efforts.
Keywords: crypto, cryptography, aah, AES, HMAC, HMAC SHA
---
# Cryptography

This document describes the cryptograhy methods provided by aah. All of aah cryptography methods are developed using Go standard libraries. aah cryptography methods are ready to use and completely tested. It could save hours of efforts that require in research and implementation.

```go
import "aahframe.work/security/acrypto"
```

## Encryption Methods

Method Name | Description
----------- | -----------
AESEncryptString | Encrypts text with AES. The key argument should be 16 or 24 or 32 bytes to select AES-128 or AES-192 or AES-256 correspondingly
AESDecryptString | Decrypts text with AES. The key argument should be the one that is used during encryption
AESEncrypt | Encrypts bytes with given `cipher.Block` in CTR mode
AESDecrypt | Decrypts bytes with given `cipher.Block` in CTR mode


## Sign Methods

Method Name | Description
----------- | -----------
SignString | Signs the given text using the key provided with HMAC SHA. Supported SHA's are SHA-1, SHA-224, SHA-256, SHA-384 and SHA-512
VerifyString | Verifies the signed text and text using the key provided with HMAC SHA. Returns true if sign text is valid; returns false otherwise
Sign | Signs the given bytes using HMAC and given SHA name. Supported SHA's are SHA-1, SHA-224, SHA-256, SHA-384 and SHA-512
Verify | Verifies whether the given key, value and mac are valid. If they are valid, it returns true; otherwise, returns false
