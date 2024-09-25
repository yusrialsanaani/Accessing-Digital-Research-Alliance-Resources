# Accessing-Digital-Research-Alliance-Resources
### Introduction to Accessing Digital Research Alliance Resources

In this guide, we will walk through three different ways to connect to the **Digital Research Alliance of Canada’s clusters**.

We will cover three key methods:

1. **Access via Terminal (SSH)**: It gives the most control over jobs, file management, and resource allocation, allowing to run scripts, manage batch jobs, and explore the cluster directly from the terminal.

2. **Access via Visual Studio Code (VS Code)**: VS Code is a great option for a graphical environment. With the **Remote - SSH** extension, we can easily connect to the cluster, edit and run code, and even debug programs all within the VS Code interface. It is a full development environment that makes working on the cluster feel just like working on the local machine.

3. **Access via Jupyter Notebooks**: If the work is more interactive—like data analysis, machine learning experiments, or visualization—Jupyter notebooks are a fantastic way to run code on the cluster. You can either use **JupyterHub**, which manages the session for you, or set up a Jupyter notebook manually on a compute node for more control.

Each method has its strengths, and this guide will help you figure out which one works best for you. Whether you are managing large-scale computations or just testing small pieces of code, this tutorial well help you get up and running on the Alliance’s powerful resources. Let's dive in!

### **1- Accessing Digital Research Alliance Resources via SSH**

In this detailed guide, I will walk you through **setting up and using SSH access** to connect to the Digital Research Alliance of Canada’s compute resources. These steps cover everything from generating your SSH key to logging into the clusters. 

#### **Step 1: Generate an SSH Key**
SSH keys are a way to securely connect to remote systems without needing to repeatedly enter passwords.

1. **Open Terminal**:
   - On Ubuntu/Linux: Open the terminal directly.
   - On Windows: You can either use Git Bash or PowerShell (for native SSH support in Windows 10+).
   
2. **Generate an SSH Key Pair**: The recommended key type is ed25519 for better security and efficiency.
   - Use the following command to create a new SSH key pair:
     ```bash
     ssh-keygen -t ed25519 -C "your_email@example.com"
     ```
     Explanation:
     - `-t ed25519`: Specifies the RSA algorithm (secure and commonly used).
     - `-C`: Adds a label to your key to make it identifiable. This can be your email or a custom label

4. **Save the Key**:
   - Press **Enter** to accept the default save location: `~/.ssh/id_ed25519`.
   - Optionally, you can provide a custom save location if you prefer.

5. **Set a Passphrase**:
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
     ssh-add ~/.ssh/id_ed25519
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
     cat ~/.ssh/id_ed25519.pub
     ```
   - Copy the entire key, which should look like this:
     ```bash
     ssh-ed25519 AAAAC3Nza... your_email@example.com
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
     or
     ```bash
     notepad $HOME\.ssh\config
     ```
2. **Add Configuration for the Alliance Clusters**:
   - Add the following configuration for clusters like Béluga, Cedar, and Narval:
```
Host beluga
  HostName beluga.computecanada.ca
  User your_compute_canada_username    # Replace with your Compute Canada username
  IdentityFile C:/Users/your_local_username/.ssh/id_ed25519  # Path to your local private SSH key
  ForwardX11 yes

Host cedar
  HostName cedar.computecanada.ca
  User your_compute_canada_username    # Replace with your Compute Canada username
  IdentityFile C:/Users/your_local_username/.ssh/id_ed25519  # Path to your local private SSH key
  ForwardX11 yes

Host graham
  HostName graham.computecanada.ca
  User your_compute_canada_username    # Replace with your Compute Canada username
  IdentityFile C:/Users/your_local_username/.ssh/id_ed25519  # Path to your local private SSH key
  ForwardX11 yes

Host narval
  HostName narval.computecanada.ca
  User your_compute_canada_username    # Replace with your Compute Canada username
  IdentityFile C:/Users/your_local_username/.ssh/id_ed25519  # Path to your local private SSH key
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
   - You’ll be logged into the cluster.

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

### Using Jupyter Notebooks on the Cluster via Visual Studio Code

In this section, we’ll focus on how to **set up Jupyter Notebooks on the cluster and access them via Visual Studio Code (VS Code)**. This allows you to combine the power of Jupyter notebooks with the remote computing resources of the cluster, all while using the familiar interface of VS Code.

---

### **Step 1: Install the Jupyter Extension in VS Code**

To work with Jupyter notebooks inside VS Code, you need to install the **Jupyter extension**:

1. **Install the Jupyter Extension**:
   - Open **Visual Studio Code**.
   - Go to the **Extensions** view by clicking on the **Extensions** icon in the sidebar (or press `Ctrl+Shift+X`).
   - Search for the **Jupyter** extension (by Microsoft) and click **Install**.

This extension provides rich support for running Jupyter notebooks directly within VS Code.

---

### **Step 2: Set Up Jupyter on the Cluster**

Before using Jupyter Notebooks on the cluster via VS Code, you need to set up Jupyter on the cluster itself. This can be done either by running Jupyter Notebooks in a compute node or using **JupyterHub** if available on the cluster.

#### **Option 1: Running Jupyter Notebook on a Compute Node**

1. **SSH into the Cluster**:
   - Connect to the cluster using **SSH** (as explained in previous sections).

2. **Load the Python Module**:
   - Load the Python environment on the cluster:
     ```bash
     module load python/3.8
     ```

3. **Create a Virtual Environment (Optional but recommended)**:
   - Create and activate a virtual environment:
     ```bash
     python -m venv jupyter_venv
     source jupyter_venv/bin/activate
     ```

4. **Install Jupyter**:
   - If Jupyter is not already installed on the cluster, install it in your virtual environment:
     ```bash
     pip install jupyter
     ```

5. **Start Jupyter Notebook**:
   - Run Jupyter Notebook on the cluster but without opening a browser:
     ```bash
     jupyter notebook --no-browser --port=8888
     ```
   - The output will show a URL containing a token, similar to:
     ```bash
     http://localhost:8888/?token=<your_token>
     ```

6. **Forward the Port to Your Local Machine**:
   - To access the notebook running on the cluster from your local machine, you need to set up SSH port forwarding.
   - Open a new terminal on your local machine and run the following command to forward port 8888:
     ```bash
     ssh -N -L 8888:localhost:8888 ysanaani@beluga.alliancecan.ca
     ```
   - Now, the notebook running on the remote cluster is accessible locally at `http://localhost:8888`.

#### **Option 2: Using JupyterHub on the Cluster**

Here’s the updated, flexible tutorial using **Beluga** as the example cluster. It includes more detailed instructions for navigating directories, modifying files in the terminal, and creating SLURM job scripts. Additionally, I have ensured to avoid repeating certain steps like loading the environment and Python version within the job script.

---

## **Using Jupyter Notebooks on Compute Canada Clusters: Beluga Example**

This guide explains how to set up and run Jupyter Notebooks on **Compute Canada clusters**, focusing on Beluga as an example. It covers both how to handle resource-intensive tasks with SLURM and how to modify job scripts through terminal commands.

---

### **Step 1: SSH into the Beluga Cluster**

Start by connecting to the Beluga cluster:

```bash
ssh your_username@beluga.alliancecan.ca
```

For example, if your username is `ysanaani`, you would run:
```bash
ssh ysanaani@beluga.alliancecan.ca
```

This command will initiate an SSH connection to the Beluga cluster.

---

### **Step 2: Navigate to Your Project Directory**

After logging in, navigate to your home directory or any subdirectory where your project files or notebooks are stored.

For example, to navigate to your own home directory (adjust based on your actual directory structure):
```bash
cd /home/ysanaani
```

Once you're in your home directory, you can navigate further to the folder where your project is stored:
```bash
cd /home/ysanaani/my_project_directory
```

*Note:* Replace `ysanaani` with your actual username and `my_project_directory` with the correct directory name for your project. If you are unsure about the directory structure, use the command `ls` to list the contents of your current directory.

---

### **Step 3: Load the Python Module and Set Up Your Virtual Environment**

Ensure that you have the required environment and Python version loaded:

```bash
module load StdEnv/2020
module load python/3.8.10
```

If needed, activate the virtual environment:
```bash
source ~/my_project_directory/jupyter_venv/bin/activate
```

---

### **Step 4: Create a SLURM Job Script for Jupyter Notebook**

If you want to run Jupyter on a compute node (for resource-intensive tasks), you need to create and submit a SLURM job. Here’s how to create and edit your job script.

1. **Create the SLURM Job Script**:
   - Use the `nano` editor to create a job script file. Run:
     ```bash
     nano jupyter_job.sh
     ```

2. **Write the Job Script**:
   - In the nano editor, type the following script to request resources and start Jupyter Notebook:

     ```bash
     #!/bin/bash
     #SBATCH --job-name=jupyter_notebook     # Job name
     #SBATCH --output=jupyter_output.txt     # Standard output log
     #SBATCH --error=jupyter_error.txt       # Standard error log
     #SBATCH --time=02:00:00                 # Set maximum runtime (adjust as needed)
     #SBATCH --cpus-per-task=4               # Number of CPU cores
     #SBATCH --mem=16G                       # Memory allocation
     #SBATCH --gres=gpu:1                    # Optional, request GPU if needed

     # Start Jupyter Notebook without opening a browser
     jupyter notebook --no-browser --port=8888 --ip=0.0.0.0
     ```

   - **Save and Exit**:
     - Press `Ctrl + O` to save the file.
     - Press `Enter` to confirm the file name.
     - Press `Ctrl + X` to exit the editor.

3. **Reopen and Edit the Job Script**:
   - If you need to make changes to the job script later, you can reopen it with:
     ```bash
     nano jupyter_job.sh
     ```
   - After making the necessary edits, save and exit as described earlier.

4. **Make the Script Executable** (Optional):
   - If you want to make the script executable, use the following command:
     ```bash
     chmod +x jupyter_job.sh
     ```

---

### **Step 5: Submit the SLURM Job**

Now that the job script is created, submit it to the SLURM scheduler:

```bash
sbatch jupyter_job.sh
```

This command will submit the job, and once the job starts, the output (including the URL and token for Jupyter) will be saved in `jupyter_output.txt`.

---

### **Step 6: Set Up SSH Port Forwarding**

While your Jupyter Notebook is running on the cluster, set up SSH port forwarding so that you can access it from your local machine:

```bash
ssh -N -L 8888:localhost:8888 your_username@beluga.alliancecan.ca
```

Leave this terminal open while Jupyter is running. This forwards port `8888` on the remote cluster to port `8888` on your local machine.

---

### **Step 7: Access Jupyter Notebook on Your Local Machine**

After the job starts and SSH port forwarding is set up, open your browser and go to:

```
http://localhost:8888
```

Enter the token that was displayed when you started Jupyter Notebook (this will be in the `jupyter_output.txt` file or in the terminal output).

---

### **Step 8: Modify or Troubleshoot the Job Script (if needed)**

If you need to modify the job script:
1. Use `nano` or another text editor to reopen it:
   ```bash
   nano jupyter_job.sh
   ```
2. After making the necessary changes, save the file as described earlier and resubmit the job using `sbatch`.

If the job fails, check the error output in the `jupyter_error.txt` file to debug any issues.

---

Here is the updated section on **Option 2: Using JupyterHub on the Cluster**, along with the correct URLs for Cedar and Narval:

---

### **Option 2: Using JupyterHub on the Cluster**

If your cluster supports **JupyterHub**, you can run Jupyter Notebooks through a web interface, which simplifies the process. This approach automatically allocates resources for your notebook without the need to manually submit SLURM jobs.

#### **Supported Clusters for JupyterHub:**
- **[JupyterHub on Beluga](https://jupyterhub.beluga.alliancecan.ca)**
- **[JupyterHub on Cedar](https://jupyterhub.cedar.computecanada.ca)**
- **[JupyterHub on Narval](https://jupyterhub.narval.alliancecan.ca)**

You can find general information about JupyterHub on clusters via the [Alliance Documentation](https://docs.alliancecan.ca/wiki/JupyterHub#JupyterHub_on_clusters).

#### **Steps to Use JupyterHub**:

1. **Access JupyterHub**:
   - Open the JupyterHub URL for your cluster (see the links above).

2. **Log in with Your Alliance Credentials**:
   - Use your Digital Research Alliance of Canada credentials to log in.

3. **Start a Notebook Session**:
   - Once logged in, you'll be prompted to select resources (e.g., number of CPUs, memory, etc.). 
   - After choosing the desired resources, start the notebook session.

4. **Run Notebooks**:
   - Jupyter notebooks can be run directly within the browser. If you need more resources (such as for machine learning tasks), you can allocate them in the session options.

---

This method is ideal for quick, interactive tasks. However, for more resource-intensive computations or longer-running tasks, refer to **Option 1** and submit your Jupyter Notebook as a SLURM job. 




