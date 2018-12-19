# SSE

SSE-S3 requires that Amazon S3 manage the data and master encryption keys.
SSE-C requires that you manage the encryption key.
SSE-KMS requires that AWS manage the data key but you manage the master key in AWS KMS.

# Storage Gateway

File Gateway (NFS) - flat files stored in S3
Volume Gateway (iSCSI)
- Stored Volumes - dataset stored on site async backed up to S3.
- Cached Volumes - dataset stored in S3 and frequently accessed data stored on site.

# KMS

## Generate Data Key

We recommend that you use the following pattern to encrypt data locally in your application:

- Use the GenerateDataKey operation to get a data encryption key.
- Use the plaintext data encryption key (returned in the Plaintext field of the response) to encrypt data locally, then erase the plaintext data key from memory.
- Store the encrypted data key (returned in the CiphertextBlob field of the response) alongside the locally encrypted data.
