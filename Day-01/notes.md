# Day 1 - AWS EC2 Key Pair Creation

## Task

Create an EC2 key pair named `devops-kp` using the RSA algorithm in the `us-east-1` region.

## Steps Performed

1. Logged into the AWS Management Console.
2. Switched to the `us-east-1` region.
3. Opened the EC2 dashboard.
4. Navigated to **Network & Security → Key Pairs**.
5. Created a new key pair:

   * Name: `devops-kp`
   * Type: `RSA`
6. Downloaded the `.pem` file.
7. Verified that the key pair was created successfully.

## What I Learned

* The purpose of EC2 key pairs.
* The difference between RSA and ED25519 keys.
* The importance of AWS regions.
* Best practices for handling private key files.

## Important Note

Never upload `.pem` files, passwords, or temporary lab credentials to GitHub.

## Status

✅ Day 1 Completed
