# Encryption at rest<a name="encryption-rest"></a>

Encryption of data at rest is automatically enabled when you create an Amazon FSx for OpenZFS file system through the AWS Management Console, the AWS CLI, or programmatically through the Amazon FSx API or one of the AWS SDKs\. Your organization might require the encryption of all data that meets a specific classification or is associated with a particular application, workload, or environment\. If you create a persistent file system, you can specify the KMS key to encrypt the data with\. If you create a scratch file system, the data is encrypted using keys managed by Amazon FSx\. For more information about creating a file system encrypted at rest using the console, see [Create Your Amazon FSx for OpenZFS File System](getting-started-step1.md)\.

**Note**  
The AWS key management infrastructure uses Federal Information Processing Standards \(FIPS\) 140\-2 approved cryptographic algorithms\. The infrastructure is consistent with National Institute of Standards and Technology \(NIST\) 800\-57 recommendations\.

For more information on how FSx for OpenZFS uses AWS KMS, see [How Amazon FSx for OpenZFS uses AWS KMS](#FSXKMS)\.

## How encryption at rest works<a name="howencrypt"></a>

In an encrypted file system, data and metadata are automatically encrypted before being written to the file system\. Similarly, as data and metadata are read, they are automatically decrypted before being presented to the application\. These processes are handled transparently by Amazon FSx for OpenZFS, so you don't have to modify your applications\.

Amazon FSx for OpenZFS uses industry\-standard AES\-256 encryption algorithm to encrypt file system data at rest\. For more information, see [Cryptography Basics](https://docs.aws.amazon.com/kms/latest/developerguide/crypto-intro.html) in the *AWS Key Management Service Developer Guide*\.

## How Amazon FSx for OpenZFS uses AWS KMS<a name="FSXKMS"></a>

Amazon FSx for OpenZFS integrates with AWS Key Management Service \(AWS KMS\) for key management for encrypting data at rest\. Amazon FSx uses AWS KMS keys to encrypt your file system in the following way:
+ **Encrypting data at rest** – Amazon FSx for OpenZFS uses a KMS key, either the AWS managed key for Amazon FSx or a custom KMS key, to encrypt and decrypt file system data\. All Amazon FSx for OpenZFS file systems are encrypted at rest with keys managed by the service\. Data is encrypted using an XTS\-AES\-256 block cipher\. Data is automatically encrypted before being written to the file system, and is automatically decrypted as it is read\. The keys used to encrypt scratch file systems at\-rest are unique per file system and destroyed after the file system is deleted\. For persistent file systems, you choose the KMS key used to encrypt and decrypt data, either the AWS managed key for Amazon FSx or a custom KMS key\. You specify which key to use when you create a persistent file system\. You can enable, disable, or revoke grants on this KMS key\. This KMS key can be one of the two following types:
  + **AWS managed key for Amazon FSx** – This is the default KMS key\. You're not charged to create and store a KMS key, but there are usage charges\. For more information, see [AWS Key Management Service pricing](https://aws.amazon.com/kms/pricing/)\.
  + **Customer managed key** – This is the most flexible KMS key to use, because you can configure its key policies and grants for multiple users or services\. For more information on creating customer managed keys, see [Creating keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the* AWS Key Management Service Developer Guide\.*

    If you use a customer managed key as your KMS key for file data encryption and decryption, you can enable key rotation\. When you enable key rotation, AWS KMS automatically rotates your key once per year\. Additionally, with a customer managed key, you can choose when to disable, re\-enable, delete, or revoke access to your customer managed key at any time\. For more information, see [Rotating AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html) and [ Enabling and disabling keys](https://docs.aws.amazon.com/kms/latest/developerguide/enabling-keys.html) in the *AWS Key Management Service Developer Guide*\.

**Important**  
Amazon FSx accepts only symmetric KMS keys\. You can't use asymmetric KMS keys with Amazon FSx\.

### Amazon FSx Key Policies for AWS KMS<a name="FSxKMSPolicy"></a>

Key policies are the primary way to control access to KMS keys\. For more information on key policies, see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide\. *The following list describes all the AWS KMS–related permissions supported by Amazon FSx for encrypted at rest file systems:
+ **kms:Encrypt** – \(Optional\) Encrypts plain\-text into cipher\-text\. This permission is included in the default key policy\.
+ **kms:Decrypt** – \(Required\) Decrypts ciphertext\. Ciphertext is plaintext that has been previously encrypted\. This permission is included in the default key policy\.
+ **kms:ReEncrypt** – \(Optional\) Encrypts data on the server side with a new KMS key, without exposing the plaintext of the data on the client side\. The data is first decrypted and then re\-encrypted\. This permission is included in the default key policy\.
+ **kms:GenerateDataKeyWithoutPlaintext** – \(Required\) Returns a data encryption key encrypted under a KMS key\. This permission is included in the default key policy under **kms:GenerateDataKey\***\.
+ **kms:CreateGrant** – \(Required\) Adds a grant to a key to specify who can use the key and under what conditions\. Grants are alternate permission mechanisms to key policies\. For more information on grants, see [Using grants](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) in the *AWS Key Management Service Developer Guide\.* This permission is included in the default key policy\.
+ **kms:DescribeKey** – \(Required\) Provides detailed information about the specified KMS key\. This permission is included in the default key policy\.
+ **kms:ListAliases** – \(Optional\) Lists all of the key aliases in the account\. When you use the console to create an encrypted file system, this permission populates the list to select the KMS key\. We recommend using this permission to provide the best user experience\. This permission is included in the default key policy\.