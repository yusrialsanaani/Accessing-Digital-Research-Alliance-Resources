# Accessing-Digital-Research-Alliance-Resources


### **1- Accessing Digital Research Alliance Resources via SSH**

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

### 2- Accessing Digital Research Alliance Resources via **Visual Studio Code**, **Globus**, and the Cluster Resources

In this combined tutorial, I will guide you through connecting to the Digital Research Alliance clusters via **Visual Studio Code**, transferring files using **Globus**, setting up your environment, submitting jobs, and more. By the end of this guide, you will have everything you need to work efficiently on the cluster.

---

### **Step 1: Install Visual Studio Code**

1. **Download and Install VS Code**:
   - Visit the official [Visual Studio Code website](https://code.visualstudio.com) and download the latest version for your platform (Windows, macOS, or Linux).
   - Follow the installation instructions for your operating system.

2. **Install Extensions**:
   - Open Visual Studio Code after installation.
   - In the left sidebar, click the **Extensions** icon (or press `Ctrl+Shift+X`).
   - Search for the following extensions and install them:
     - **Remote - SSH** (by Microsoft): This will allow you to connect to remote machines over SSH.
     - **Python** (by Microsoft): If you will be working with Python code on the cluster.

---

### **Step 2: Uploading Files to the Cluster Using Globus**

Before you start coding, you will likely need to transfer files from your local machine to the cluster. Here's how you can upload files using **Globus**.

1. **Create a Globus Account**:
   - Go to [Globus](https://www.globus.org/) and create a free account if you don’t have one.
   
2. **Install Globus Connect Personal**:
   - Download and install the **Globus Connect Personal** client for your operating system (available for Windows, macOS, and Linux).
   - Follow the instructions to set up a **Personal Endpoint** on your local machine. This allows your machine to serve as a transfer point.

3. **Log in to Globus**:
   - Once logged in, search for the endpoint associated with the Digital Research Alliance cluster (e.g., Béluga).
     - **Example Endpoint Name**: `beluga#compute`

4. **Initiate a File Transfer**:
   - On the left-hand side, select your **local endpoint** (your computer).
   - On the right-hand side, select the **cluster endpoint** (e.g., Béluga).
   - Navigate to the directory on the cluster where you want to transfer the files (e.g., `/home/ysanaani/my_project/`).
   - Select the files from your local machine and click **Start Transfer**.
   - The transfer will proceed in the background, and you will receive an email once it completes.

5. **Verify File Transfer**:
   - Once the transfer is complete, SSH into the cluster via VS Code (as explained below) or the terminal.
   - Navigate to the directory to verify that the files are there:
     ```bash
     cd ~/my_project
     ls
     ```

---

### **Step 3: Configure SSH in VS Code**

Now that you’ve uploaded your files, you need to configure **SSH access** in Visual Studio Code to work on the cluster.

1. **Set Up Your SSH Key**:
   - If you haven't already generated and added your SSH key (as described in the SSH tutorial), you need to do that first. Make sure your SSH key is registered with your Digital Research Alliance account.

2. **Add SSH Configuration to VS Code**:
   - Open the **Command Palette** in VS Code (press `Ctrl+Shift+P` or `F1`).
   - Type and select **Remote-SSH: Open SSH Configuration File**.
   - Choose the configuration file location: `~/.ssh/config`.
   - Add the following SSH configuration for the clusters you plan to access:
     ```
     Host beluga
       HostName beluga.alliancecan.ca
       User ysanaani
       IdentityFile ~/.ssh/id_rsa

     Host cedar
       HostName cedar.alliancecan.ca
       User ysanaani
       IdentityFile ~/.ssh/id_rsa

     Host narval
       HostName narval.alliancecan.ca
       User ysanaani
       IdentityFile ~/.ssh/id_rsa
     ```
   - Save the file (`Ctrl+S`).

---

### **Step 4: Connect to the Remote Cluster**

1. **Open the Command Palette** (`Ctrl+Shift+P` or `F1`).
2. **Search for and select**: **Remote-SSH: Connect to Host**.
3. **Select the Cluster**: You should see the cluster names you added in the SSH config file (e.g., `beluga`, `cedar`, `narval`).
   - Click on the cluster you want to connect to (e.g., `beluga`).
4. **Authenticate**: If this is the first time you're connecting, VS Code may ask for confirmation regarding the host's authenticity. Type `yes` to continue.

   - If you're prompted for a passphrase (if you set one when creating your SSH key), enter it.
   
5. **Remote Connection Established**: You should now be connected to the remote cluster. You can confirm the connection by looking at the bottom-left corner of VS Code, where it will display the name of the host (e.g., `SSH: beluga`).

---

### **Step 5: Navigating and Organizing Files on the Cluster**

Once connected, you can navigate and manage your files:

1. **Using the VS Code Explorer**:
   - In the **Explorer** tab of VS Code, you can browse and open your files on the cluster.
   - You can also create new files or folders by right-clicking within the **Explorer** panel and selecting **New File** or **New Folder**.

2. **Using the Terminal**:
   - You can navigate through directories, create files, and manage your environment via the terminal:
     - **To create a directory**:
       ```bash
       mkdir new_directory
       ```
     - **To create a file**:
       ```bash
       touch myfile.py
       ```
     - **To list files**:
       ```bash
       ls
       ```

---

### **Step 6: Setting Up a Python Virtual Environment**

A **virtual environment** allows you to manage project-specific dependencies without affecting the system-wide Python installation.

1. **Load the Python Module**:
   - Clusters often have multiple Python versions installed. First, load the Python module:
     ```bash
     module load python/3.8
     ```

2. **Create a Virtual Environment**:
   - Navigate to your project directory and create a virtual environment:
     ```bash
     cd ~/my_project
     python -m venv my_venv
     ```
     - `my_venv` is the name of the virtual environment directory.

3. **Activate the Virtual Environment**:
   - Activate the virtual environment before installing dependencies:
     ```bash
     source my_venv/bin/activate
     ```

4. **Install Python Packages**:
   - You can now install any Python packages your project needs:
     ```bash
     pip install numpy pandas
     ```

5. **Deactivate the Environment**:
   - When you're done working in the virtual environment, deactivate it:
     ```bash
     deactivate
     ```

---

### **Step 7: Checking Cluster Resources**

Before submitting jobs, it’s useful to check the available resources such as CPUs, GPUs, and memory.

1. **Check the Available Resources**:
   - Use the `sinfo` command to view the available resources and status of the cluster:
     ```bash
     sinfo
     ```
   - This will display a list of nodes, their availability, and whether they are free or in use.

2. **Check Your Resource Quota**:
   - You can also check your usage and quota with:
     ```bash
     sacctmgr show user ysanaani
     ```

---

### **Step 8: Submitting Jobs on the Cluster**

Now that you’ve uploaded your files, set up your environment, and checked resources, you can submit a job to the cluster.

1. **Create a Job Script**:
   - A typical job script defines how many CPUs, GPUs, memory, and time your task requires. Create a file called `my_job_script.sh`:
     ```bash
     nano my_job_script.sh
     ```
   - Add the following lines to the script:
     ```bash
     #!/bin/bash
     #SBATCH --job-name=my_job
     #SBATCH --output=output.txt
     #SBATCH --error=error.txt
     #SBATCH --time=01:00:00          # Max runtime of 1 hour
     #SBATCH --cpus-per-task=4        # Request 4 CPU cores
     #SBATCH --mem=16G                # Request 16GB of memory
     
     module load python/3.8
     source ~/my_project/my_venv/bin/activate
     
     python ~/my_project/my_script.py  # Replace with your Python script
     ```

2. **Submit the Job**:
   - Submit the job using the `sbatch` command:
     ```bash
     sbatch my_job_script.sh
     ```
   - You will receive a job ID after submitting.

3. **Check the Job Status**:
   - To check the status of your job, use the `squeue` command:
     ```bash
     squeue -u ysanaani
     ```

4. **View Job Output**:
   - Once the job completes, you can check the output and error files (`output.txt` and `error.txt`).
   - View the

 contents with:
     ```bash
     cat output.txt
     ```

---

### **Step 9: Creating and Running a Simple Example Job**

Let’s go through a quick example of running a simple Python script on the cluster:

1. **Create a Simple Python Script**:
   - In your project directory, create a new Python script, `hello_world.py`:
     ```bash
     nano hello_world.py
     ```
   - Add the following code:
     ```python
     print("Hello, Digital Research Alliance!")
     ```

2. **Create a Job Script**:
   - Now, create a job script to run this Python script:
     ```bash
     nano hello_world_job.sh
     ```
   - Add the following lines:
     ```bash
     #!/bin/bash
     #SBATCH --job-name=hello_world
     #SBATCH --output=hello_output.txt
     #SBATCH --time=00:10:00

     module load python/3.8
     python ~/my_project/hello_world.py
     ```

3. **Submit the Job**:
   - Submit this job to the cluster:
     ```bash
     sbatch hello_world_job.sh
     ```

4. **Check Output**:
   - After the job finishes, check the `hello_output.txt` file for the output:
     ```bash
     cat hello_output.txt
     ```
   - You should see the output:
     ```bash
     Hello, Digital Research Alliance!
     ```

---

This complete guide integrates file uploads with **Globus**, cluster access using **Visual Studio Code**, and submitting jobs on the Digital Research Alliance clusters. With these steps, you can now transfer files, set up environments, submit jobs, and monitor your tasks with ease.

Let me know if you'd like to explore any specific section further!
