# set-up-ssh-guide

This guide provides step-by-step instructions for generating, configuring, and managing SSH keys for multiple accounts.

---

## Steps to Generate and Configure SSH Keys

### STEP 1: Create a Folder for SSH Key Generation
Create a folder to store your SSH keys.

---

### STEP 2: Generate a Second SSH Key
If you don't already have an SSH key for the second account:
- **Command**: `ssh-keygen -t ed25519/rsa -C "email_id"`
  - `ed25519/rsa` are the algorithms you can choose from.
- **After Command**:
  - Provide a name for the SSH private/public key file (e.g., `ssh_file_name`).
  - Add a passphrase for additional security (e.g., `12345`).

---

### STEP 3: Add the New Key to the SSH Agent
Add the private key to the SSH agent:
- **Command**: `ssh-add ~/.ssh/ssh_file_name`
  - This stores the private key in memory for the helper agent.
- **Verify**:
  - **Command**: `ssh-add -l`
  - This lists all added SSH identities. Verify by checking the email ID.

---

### STEP 4: Check Existing SSH Keys
List all existing SSH keys:
- **Command**: `ls -l ~/.ssh/`
- If you see files like `id_ed25519` and `id_ed25519.pub`, you can use them.
- **To Copy the Public Key**:
  - **Command**: `cat id_ed25519.pub`
  - Paste the output into GitHub.

---

### STEP 5: Add the Key to GitHub
1. Open GitHub in your browser.
2. Navigate to **Settings → SSH and GPG keys → New SSH Key**.
3. Paste the public key.

---

### STEP 6: Configure SSH for Multiple Accounts
If you encounter a "directory not found" error:
1. **Create the Config File**:
   - ** `drwxr`: This represents the permissions of a directory in a Unix-like file system.
    - `d`: Indicates that this is a directory.
    - `rwx`: The owner of the directory has read (`r`), write (`w`), and execute (`x`) permissions.
    - `r-x`: Members of the group have read (`r`) and execute (`x`) permissions, but no write (`-`) permission.
    - `r-x`: Others (everyone else) also have read (`r`) and execute (`x`) permissions, but no write (`-`) permission.


    755 // This number represents the permissions of a directory in a Unix-like file system. It means:
        // - The owner has full permissions: read (4), write (2), and execute (1), totaling 7.
        // - The group has read (4) and execute (1) permissions, totaling 5.
        // - Others also have read (4) and execute (1) permissions, totaling 5.```

   - **Command**:
     ```
     touch ~/.ssh/config
     chmod 600 ~/.ssh/config
     ```

2. **Modify the Config File**:
   - **Command**: `nano ~/.ssh/config`
   - Add the following configuration:
     ```
     Host github.com-second
         HostName github.com
         User git
         IdentityFile ~/.ssh/id_ed25519
     ```
   - Save and exit (`CTRL + X`, `Y`, `ENTER`).

**Details**:
- `Host github.com-second`: Creates an alias for GitHub for the second account.
- `IdentityFile ~/.ssh/id_ed25519`: Specifies the SSH key to use.

**Important**:
- Use `github.com` for repositories requiring private libraries (e.g., `@ git+ssh://git@github.com/kafka@version.1`).
- This prevents "connection refused" errors.

---

### STEP 7: Test the SSH Configuration
- **Command**: `ssh -T git@github.com`
- **Expected Message**: 
  ```
  Hi YourSecondUsername! You've successfully authenticated, but GitHub does not provide shell access.
  ```

---

### STEP 8: Clone a Repository with the New Identity
1. Copy the SSH URL from your GitHub repository.
2. Replace `git@github.com` with your new identity (`git@github.com-second`).
   - **Command**: `git clone git@github.com-second:YourSecondAccount/YourRepo.git`

**Default**: `git@github.com`  
**Modified**: `git@github.com-second`

---

### STEP 9: Verify the Remote URL
- **Command**: `git remote -v`
- **Expected Output**:
  ```
  origin  git@github.com-second:YourSecondAccount/YourRepo.git (fetch)
  origin  git@github.com-second:YourSecondAccount/YourRepo.git (push)
  ```
- If it shows `github.com` instead of `github.com-second`, update it:
  - **Command**:
    ```
    git remote set-url origin git@github.com-second:a-m-i-t-chaurasiya/docker.git
    ```

---

## Removing All SSH Keys

### STEP 1: List All Existing SSH Keys
- **Command**: `ls -al ~/.ssh`
- This displays all files in the `.ssh` directory (e.g., `id_rsa`, `id_ecdsa`, `id_ed25519`).

--- 

### STEP 2: Remove the SSH Keys
- **Command**:
  ```
  rm -f ~/.ssh/id_rsa ~/.ssh/id_rsa.pub
  rm -f ~/.ssh/id_ecdsa ~/.ssh/id_ecdsa.pub
  rm -f ~/.ssh/id_ed25519 ~/.ssh/id_ed25519.pub
  ```
- This deletes the private and public key pairs.

---

### STEP 3: Verify the Removal
- **Command**: `ls -al ~/.ssh`
- The directory should no longer contain any SSH key files.

**Note**: Be cautious when removing SSH keys, as this action is irreversible. Ensure you have backups if needed.

---
