# 🪐 ULTRA-HARDENED TRANSPORT CRYPTO V3

[![Stack: WebCrypto](https://img.shields.io/badge/Stack-WebCrypto-00ff9d?style=flat-square&logo=javascript)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)
[![Layer: Multi-Layer](https://img.shields.io/badge/Security-Multi--Layer_EtM-ff0055?style=flat-square)](https://en.wikipedia.org/wiki/Authenticated_encryption)

## 💀 PHILOSOPHY
This is not your average `crypto.js` wrapper. This is a **multi-layer transport shield** designed for end-to-end (E2EE) integrity. It treats data like a high-value asset moving through a hostile network. If one layer leaks, the next one bites.

## 🧬 ARCHITECTURAL LAYERS
The engine utilizes a cycling recursive encryption strategy based on the number of provided keys. Each key adds a new cryptographic "skin":

1.  **AES-256-GCM (AEAD):** Galois/Counter Mode for high-speed authenticated encryption with AD (Associated Data).
2.  **AES-CTR + HMAC-SHA-256 (EtM):** Counter mode with "Encrypt-then-Mac" for malleable-resistance.
3.  **AES-CBC + HMAC-SHA-256 (EtM):** Classic Cipher Block Chaining with PKCS#7 padding and strict integrity verification.

## 🛠 TECHNICAL SPECIFICATIONS
- **KDF:** PBKDF2-HMAC-SHA256 (310,000 iterations) -> HKDF-SHA256.
- **Entropy:** Hardware-level `crypto.getRandomValues()`.
- **Integrity:** Strict **EtM (Encrypt-then-MAC)** pattern across all layers.
- **Idempotency:** `decryptedDataClient` intelligently detects if the payload is encrypted or raw, preventing double-decryption crashes.

## 🚀 USAGE

### 1. Encryption (Packing the Payload)
```typescript
import { encryptedDataClient } from './client';
import { keys } from '@decrypted/decryptedKeys';
```bash

const data = { sensitive: "Top Secret Info" };
const cipherText = await encryptedDataClient(data, keys);
```
// Returns a Base64-encoded JSON Envelope v3
- 2. Decryption (Unpacking with Precision)
import { decryptedDataClient } from './client';

const secureData = await decryptedDataClient(cipherText, keys);
console.log(secureData.sensitive); // "Top Secret Info"

# 📊 ENVELOPE STRUCTURE (V3)
```bash
{
  "v": 3,
  "kdf": "PBKDF2-310000->HKDF",
  "s": "BASE64_SALT",
  "layers": [
    { "a": "GCM", "sl": "L_SALT", "iv": "IV", "tg": "TAG" },
    { "a": "CTR-HMAC", "sl": "L_SALT", "iv": "IV", "tg": "TAG" }
  ],
  "ct": "BASE64_FINAL_CIPHERTEXT"
}
```
# ⚠️ SECURITY DISCLOSURE
- Zero-Trust: Keys are never stored in the clear.

- Reverse-Order Decryption: The decryption process strictly mirrors the encryption stack in reverse (LIFO).

- Environment Agnostic: Works flawlessly in Node.js (LTS) and Modern Browsers.


## Developed with cold analytical mind. Built for the street.
**Stay encrypted.**