 ### I havent yet work with multiple accounts because I didnt had AWS Organiztaion to manage all of them.

# AWS CLI: Managing Multiple AWS Accounts

## Introduction

**AWS CLI (Command Line Interface)** is a powerful tool for interacting with AWS services from the command line. One of the useful features of AWS CLI is the ability to manage multiple AWS accounts through named profiles. This is particularly important in environments where you need to access resources from different AWS accounts (such as development, staging, and production).

This guide explains how to configure, switch between, and manage multiple AWS accounts using AWS CLI.

---


## Setting Up AWS CLI for Multiple Accounts

### Step 1: Install AWS CLI

If you haven’t installed AWS CLI, follow the [official documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) for installation on your operating system.

### Step 2: Configure Multiple Profiles

To manage multiple AWS accounts, you will configure separate profiles for each account.

1. **Configure AWS CLI**:
   Run the following command to create or update your profile:

   ```bash
   aws configure --profile profile-name
   ```

   AWS CLI will prompt you to enter:
   - AWS Access Key ID
   - AWS Secret Access Key
   - Default Region
   - Default Output Format

   For example:

   ```bash
   aws configure --profile dev-account
   ```

   You can repeat this for each account you need to configure, replacing `dev-account` with the desired profile name (e.g., `prod-account`, `test-account`, etc.).

2. **AWS Config and Credentials Files**:
   The profiles are stored in two files:
   - `~/.aws/config`: Contains region and output format for each profile.
   - `~/.aws/credentials`: Contains the access and secret keys for each profile.

   Here’s an example:

   ```ini
   [profile dev-account]
   region = us-west-2
   output = json

   [profile prod-account]
   region = us-east-1
   output = json
   ```

### Step 3: Switching Between Accounts

You can switch between AWS accounts using the `--profile` option with any AWS CLI command.

```bash
aws s3 ls --profile dev-account
```

Alternatively, set the `AWS_PROFILE` environment variable to make a specific profile the default for the current session.

```bash
export AWS_PROFILE=prod-account
```

### Step 4: Setting a Default Profile

If you work with one account more frequently, you can set it as the default profile.

1. **Set in a session**:

   ```bash
   export AWS_DEFAULT_PROFILE=prod-account
   ```

2. **Permanently set in your environment** (add this to your `.bashrc` or `.zshrc`):

   ```bash
   export AWS_DEFAULT_PROFILE=prod-account
   ```

### Step 5: Viewing Configured Profiles

To list all profiles you've configured, run:

```bash
aws configure list-profiles
```

---

## Best Practices for Managing Multiple AWS Accounts

### 1. **Use Separate Profiles for Each Account**
   - Keep each account isolated with separate credentials and profiles.
   - Name profiles descriptively (e.g., `dev-account`, `prod-account`) to avoid confusion.

### 2. **Leverage AWS Organizations**
   - If managing multiple accounts, AWS Organizations can help you consolidate billing and apply policies to multiple accounts from a central place.

### 3. **Rotate Credentials Regularly**
   - Ensure that credentials stored in `~/.aws/credentials` are rotated regularly and never hardcoded in your applications.

### 4. **Use MFA (Multi-Factor Authentication)**
   - For increased security, enforce MFA for accessing the AWS accounts. AWS CLI can integrate with MFA tokens for secure access.

---

## Common Questions

### 1. **What does 'Account' refer to in AWS CLI?**

   In the context of AWS CLI, an **account** refers to a separate AWS account with its own resources and billing. IAM users (like Nabeeha or test-user) are entities within a single AWS account. You only need to set up multiple accounts if you are managing entirely separate AWS accounts, not just multiple IAM users within the same account.

### 2. **Can I have multiple accounts with different billing info?**

   Yes, each AWS account has its own billing setup. If you want to use multiple AWS accounts with distinct billing, you would create separate AWS accounts with different email addresses, payment methods, etc. You can then manage them using AWS CLI profiles.

### 3. **How to switch between different AWS accounts using AWS CLI?**

   You can switch accounts by specifying the profile for the account:

   ```bash
   aws s3 ls --profile dev-account
   ```

   Alternatively, set the profile as the default for your session:

   ```bash
   export AWS_PROFILE=prod-account
   ```

### 4. **Can I change the default profile?**

   Yes, you can change the default profile by setting the `AWS_DEFAULT_PROFILE` environment variable:

   ```bash
   export AWS_DEFAULT_PROFILE=prod-account
   ```

   This will set `prod-account` as the default profile for the current session.

---

## FAQs

### Q1: **What does "Account" refer to in AWS CLI?**
   - In AWS CLI, "account" refers to an individual AWS account. It doesn’t refer to IAM users but the entire AWS environment that includes billing, resources, and access.

### Q2: **Can I switch between IAM users in the same account using profiles?**
   - Yes, you can configure separate profiles for different IAM users within the same account by specifying different credentials for each user.

### Q3: **How can I view all the configured profiles?**
   - Run the command:

     ```bash
     aws configure list-profiles
     ```

### Q4: **How do I change the default AWS CLI account?**
   - You can either set the default profile for a single session using `export AWS_PROFILE=profile-name` or set it permanently in your `.bashrc` or `.zshrc` file.

### Q5: **Can I use CLI across multiple AWS accounts simultaneously?**
   - Yes, you can set up multiple profiles and switch between them using the `--profile` option for each CLI command.

---

## Summary

By leveraging AWS CLI's profiles, you can easily switch between multiple AWS accounts and manage different resources. It is especially useful for environments with distinct AWS accounts for development, testing, and production. The ability to handle multiple accounts efficiently saves time and minimizes the risk of managing resources in the wrong environment.
