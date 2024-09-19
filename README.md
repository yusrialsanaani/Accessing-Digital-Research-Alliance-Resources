# Accessing-Digital-Research-Alliance-Resources


### Full Step-by-Step Tutorial: **Accessing Digital Research Alliance Resources via SSH**

In this detailed guide, I will walk you through **setting up and using SSH access** to connect to the Digital Research Alliance of Canada’s compute resources. These steps cover everything from generating your SSH key to logging into the clusters. 

#### **Step 1: Generate an SSH Key**
SSH keys are a way to securely connect to remote systems without needing to repeatedly enter passwords.

1. **Open Terminal**:
   - On Ubuntu/Linux: Open the terminal directly.
   - On Windows: You can either use Git Bash or PowerShell (for native SSH support in Windows 10+).
   
2. **Generate an SSH Key Pair**:
   - Use the following command to create a new SSH key pair:
     ```bash
     ssh-keygen -t rsa -b 4096 -C "ysanaani@alliancecan.ca"
     ```
     Explanation:
     - `-t rsa`: Specifies the RSA algorithm (secure and commonly used).
     - `-b 4096`: Sets the key length to 4096 bits for stronger security.
     - `-C`: Adds a label to your key to make it identifiable.

3. **Save the Key**:
   - Press **Enter** to accept the default save location: `~/.ssh/id_rsa`.
   - Optionally, you can provide a custom save location if you prefer.

4. **Set a Passphrase**:
   - You will be prompted to enter a passphrase for the key. It is recommended for extra security, though optional. You can leave it empty if you don’t want a passphrase.

#### **Step 2: Add Your SSH Key to the SSH Agent**
After creating the SSH key, you need to add it to your local SSH agent to manage your keys.

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
   - To add your private SSH key to the agent, run:
     ```bash
     ssh-add ~/.ssh/id_rsa
     ```

#### **Step 3: Add the SSH Key to Your Digital Research Alliance Account**
This step ensures that the cluster will recognize your SSH key when you attempt to connect.

1. **Log in to Your Alliance Account**:
   - Go to the [CCDB login page](https://ccdb.computecanada.ca) and log in with your credentials.

2. **Access the SSH Key Management Section**:
   - Navigate to **My Account** or **SSH Keys**.
   - Look for an option to **add an SSH key**.

3. **Copy Your Public SSH Key**:
   - Run the following command to display your public key:
     ```bash
     cat ~/.ssh/id_rsa.pub
     ```
   - Copy the entire key, which should look like this:
     ```bash
     ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCt... ysanaani@alliancecan.ca
     ```

4. **Paste the SSH Key into Your Account**:
   - Paste the copied SSH public key into the provided form in your CCDB account.
   - Click **Save** to ensure it’s linked to your account.

#### **Step 4: Configure SSH Access via the Config File**
You can configure the SSH client to simplify connections to the Alliance clusters. This eliminates the need to type full hostnames and usernames every time you connect.

1. **Create/Edit the SSH Config File**:
   - Open your SSH config file with the following command:
     ```bash
     nano ~/.ssh/config
     ```
   - If the file doesn't exist, create it.

2. **Add Configuration for the Alliance Clusters**:
   - Add the following configuration for clusters like Béluga, Cedar, and Narval:
     ```
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
   Explanation:
   - **Host**: The nickname for the server (e.g., Béluga, Cedar).
   - **HostName**: The actual cluster hostname.
   - **User**: Your Alliance username (`ysanaani` in this case).
   - **IdentityFile**: The path to your SSH private key.

3. **Save and Exit**:
   - Press `Ctrl+O` to save the file, then `Ctrl+X` to exit.

#### **Step 5: Connect to the Alliance Clusters**
Now that everything is set up, you can connect to the clusters using the SSH command.

1. **Open a Terminal**:
   - Open your terminal (Linux/Ubuntu/WSL for Windows).

2. **Connect to a Cluster**:
   - Use the command to SSH into one of the clusters (for example, Béluga):
     ```bash
     ssh beluga
     ```
   - You’ll be logged into the cluster without needing to enter your password, as the SSH key is now being used.

3. **Check Available Resources**:
   - After connecting, you can check available resources or submit jobs:
     ```bash
     sinfo  # Lists available resources on the cluster
     ```
   - Example of submitting a job script:
     ```bash
     sbatch my_job_script.sh
     ```

#### **Step 6: Transfer Files Between Local and Remote**
Use **scp** to transfer files between your local system and the cluster.

1. **Transfer a File from Local to Remote**:
   - To copy a local file to the remote cluster:
     ```bash
     scp /path/to/local/file beluga:/path/to/remote/directory
     ```

2. **Transfer a File from Remote to Local**:
   - To download a file from the cluster to your local machine:
     ```bash
     scp beluga:/path/to/remote/file /path/to/local/directory
     ```

#### **Step 7: Disconnect from the Cluster**
When you’re finished with your session, type:
```bash
exit
```
This will safely log you out of the cluster.

---

This completes the **SSH method** for accessing the Digital Research Alliance of Canada's resources. Each step is essential for establishing a secure connection, and I’ve broken down every command and explanation for clarity.

Let me know if you’re ready to move on to the **Visual Studio Code method**, or if you need further clarification on any of the steps above.
