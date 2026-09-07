# AWS Day 41 – KMS Encryption and Decryption

## Objective

Create a symmetric AWS KMS key and use it to encrypt and decrypt a sensitive file.

## Resources Created

* KMS Key Alias: `alias/datacenter-KMS-Key`
* KMS Key ID: `4722f274-e85b-42e6-b1ef-e55817fc2024`
* Region: `us-east-1`
* Key Spec: `SYMMETRIC_DEFAULT`
* Key Usage: `ENCRYPT_DECRYPT`
* Key State: `Enabled`

## Files

* Original: `/root/SensitiveData.txt`
* Encrypted: `/root/EncryptedData.bin`
* Decrypted: `/root/DecryptedData.txt`

## Encryption

Encrypted `SensitiveData.txt` using the symmetric KMS key. The Base64-encoded ciphertext returned by AWS KMS was decoded and saved as `EncryptedData.bin`.

## Decryption

Successfully decrypted `EncryptedData.bin` using AWS KMS and saved the result as `DecryptedData.txt`.

## Verification

Used `cmp` to compare the original and decrypted files. No differences were reported, confirming that the decrypted data exactly matches the original file.

## Final Status

AWS Day 41 completed successfully.
