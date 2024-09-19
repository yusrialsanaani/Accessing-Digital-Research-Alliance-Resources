
# Full Step-by-Step Tutorial: Accessing Digital Research Alliance Resources via SSH

This document provides a detailed guide for accessing the Digital Research Alliance of Canada's resources via SSH. 
Follow the instructions step by step to securely connect to the compute clusters.

## Step 1: Generate an SSH Key
SSH keys are a way to securely connect to remote systems without needing to repeatedly enter passwords.

1. **Open Terminal**:
   - On Ubuntu/Linux: Open the terminal directly.
   - On Windows: You can either use Git Bash or PowerShell (for native SSH support in Windows 10+).
   
2. **Generate an SSH Key Pair**:
   Use the following command to create a new SSH key pair:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "ysanaani@alliancecan.ca"
   ```
   Explanation:
   - `-t rsa`: Specifies the RSA algorithm (secure and commonly used).
   - `-b 4096`: Sets the key length to 4096 bits for stronger security.
   - `-C`: Adds a label to your key to make it identifiable.

3. **Save the Key**:
   Press **Enter** to accept the default save location: `~/.ssh/id_rsa`.

4. **Set a Passphrase** (Optional):
   You will be prompted to enter a passphrase for the key.

## Step 2: Add Your SSH Key to the SSH Agent

1. **Start the SSH Agent**:
   - On Linux/Ubuntu:
     ```bash
     eval "$(ssh-agent -s)"
     ```
   - On Windows (in Git Bash):
     ```bash
     eval $(ssh-agent -s)
     ```

2. **Add the SSH Key**:
   Run:
   ```bash
   ssh-add ~/.ssh/id_rsa
   ```

## Step 3: Add the SSH Key to Your Digital Research Alliance Account

1. **Log in to Your Alliance Account**:
   Go to the [CCDB login page](https://ccdb.computecanada.ca) and log in with your credentials.

2. **Access the SSH Key Management Section**:
   Navigate to **My Account** or **SSH Keys**.

3. **Copy Your Public SSH Key**:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

4. **Paste the SSH Key into Your Account**.

## Step 4: Configure SSH Access via the Config File

1. **Create/Edit the SSH Config File**:
   ```bash
   nano ~/.ssh/config
   ```

2. **Add Configuration for the Alliance Clusters**:
   ```bash
   Host beluga
       HostName beluga.alliancecan.ca
       User ysanaani
       IdentityFile ~/.ssh/id_rsa
       ForwardX11 yes

   Host cedar
       HostName cedar.alliancecan.ca
       User ysanaani
       IdentityFile ~/.ssh/id_rsa
       ForwardX11 yes

   Host narval
       HostName narval.alliancecan.ca
       User ysanaani
       IdentityFile ~/.ssh/id_rsa
       ForwardX11 yes
   ```

3. **Save and Exit**.

## Step 5: Connect to the Alliance Clusters

1. **Open a Terminal**.

2. **Connect to a Cluster**:
   ```bash
   ssh beluga
   ```

3. **Check Available Resources**:
   ```bash
   sinfo
   ```

## Step 6: Transfer Files Between Local and Remote

1. **Transfer a File from Local to Remote**:
   ```bash
   scp /path/to/local/file beluga:/path/to/remote/directory
   ```

2. **Transfer a File from Remote to Local**:
   ```bash
   scp beluga:/path/to/remote/file /path/to/local/directory
   ```

## Step 7: Disconnect from the Cluster
To safely disconnect:
```bash
exit
```

---
This completes the SSH method for accessing the Digital Research Alliance of Canada's resources.
