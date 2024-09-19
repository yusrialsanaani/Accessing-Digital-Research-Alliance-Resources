# Accessing-Digital-Research-Alliance-Resources
### Introduction to Accessing Digital Research Alliance Resources

In this guide, we’ll walk through three different ways to connect to the **Digital Research Alliance of Canada’s clusters**. Depending on how you like to work—whether you're a fan of the command line, prefer a visual interface, or love using Jupyter notebooks—there's a method here that will fit right into your workflow.

We’ll cover three key methods:

1. **Access via Terminal (SSH)**: This is perfect if you’re comfortable with the command line. It gives you the most control over jobs, file management, and resource allocation. You'll be able to run scripts, manage batch jobs, and explore the cluster directly from your terminal.

2. **Access via Visual Studio Code (VS Code)**: If you like working in a graphical environment, VS Code is a great option. With the **Remote - SSH** extension, you can easily connect to the cluster, edit and run your code, and even debug programs all within the VS Code interface. It’s a full development environment that makes working on the cluster feel just like working on your local machine.

3. **Access via Jupyter Notebooks**: If your work is more interactive—like data analysis, machine learning experiments, or visualization—Jupyter notebooks are a fantastic way to run code on the cluster. You can either use **JupyterHub**, which manages the session for you, or set up a Jupyter notebook manually on a compute node for more control.

Each method has its strengths, and this guide will help you figure out which one works best for you. Whether you’re managing large-scale computations or just testing small pieces of code, we’ll help you get up and running on the Alliance’s powerful resources. Let's dive in!

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

Some clusters (such as **Béluga**, **Cedar**, **Narval**) provide access to **JupyterHub**, which allows you to run Jupyter notebooks through a web interface without manually setting up the notebook server.

1. **Access JupyterHub**:
   - Open your browser and navigate to the JupyterHub URL for the cluster:
     - For **Béluga**: `https://jupyterhub.beluga.alliancecan.ca`
     - For **Cedar**: `https://jupyterhub.cedar.computecanada.ca`
   - Log in with your Alliance credentials.

2. **Start a Notebook Session**:
   - After logging in, you can start a new notebook session by selecting the appropriate number of resources (CPUs, memory, etc.) for your task.
   - Once the notebook server is started, you’ll be provided with a web interface where you can create and work with notebooks.

---

### **Step 3: Connect VS Code to the Jupyter Notebook on the Cluster**

Once Jupyter is running on the cluster, you can connect to it from VS Code.

1. **Open Jupyter Server in VS Code**:
   - In VS Code, open the **Command Palette** by pressing `Ctrl+Shift+P`.
   - Search for **Jupyter: Specify Jupyter Server for Connections** and select it.
   
2. **Select Remote Jupyter Server**:
   - Choose the option **Existing: Specify the URI of an existing server**.
   - Enter the URL for the Jupyter server you started on the cluster (e.g., `http://localhost:8888/?token=<your_token>`).

3. **Open or Create a New Jupyter Notebook**:
   - Now, open an existing `.ipynb` file or create a new Jupyter notebook by selecting **Jupyter: Create New Jupyter Notebook** from the Command Palette.
   - You should now be connected to the remote Jupyter server running on the cluster, and you can execute code cells as usual.

---

### **Step 4: Running Notebooks and Selecting Kernels**

1. **Run Code Cells**:
   - Once the notebook is open, you can run code cells just like in a standard Jupyter notebook. VS Code provides the same interface with execution, variable exploration, and code output.

2. **Select the Right Kernel**:
   - In the top-right corner of the notebook, click the **kernel picker**.
   - You can select the appropriate Python environment running on the cluster (for example, the one in your virtual environment).

3. **Save and Load Notebooks**:
   - Changes to the notebook are automatically saved to the remote server. You can also manually save notebooks as usual by clicking the **Save** button.

---

### **Step 5: Using Jupyter Notebooks Efficiently on the Cluster**

#### **Running Short Interactive Jobs**:
- **Jupyter Notebooks** are great for running short and interactive tasks. However, if your task requires extensive computation, you should avoid running it on the login nodes or within the notebook. Instead, submit the task as a batch job.

#### **Submitting Long-Running Jobs**:
- For long-running jobs or jobs that require large amounts of resources, it’s better to submit them using a batch job script through **SLURM** (as shown in the previous section).

---

### **Summary**

Here’s what you need to do to work with Jupyter Notebooks on the Digital Research Alliance clusters via **Visual Studio Code**:
1. **Install the Jupyter Extension** in VS Code.
2. **Set up Jupyter** on the cluster, either manually or through **JupyterHub**.
3. **Forward ports** using SSH if running Jupyter manually on a compute node.
4. **Connect to the Jupyter Server** from VS Code using the **Jupyter: Specify Jupyter Server for Connections** command.
5. **Run notebooks and manage kernels** directly within VS Code for a smooth workflow.

You're right to ask about this, as it depends on how you use Jupyter on the cluster. Let's clarify:

### **Two Scenarios for Running Jupyter on the Cluster**

1. **Using JupyterHub on the Cluster**:
   - When you use **JupyterHub**, it already allocates resources (e.g., CPUs, memory, etc.) for you via the cluster's scheduler (SLURM) when you start a notebook session. 
   - **No job script is needed** because JupyterHub manages resource allocation for your session.
   - The session runs in an isolated environment, and you don’t need to manually submit jobs.

2. **Running Jupyter Notebook Manually on a Compute Node**:
   - If you're running **Jupyter Notebook** manually on a compute node (without JupyterHub), it's recommended to submit a job script to allocate resources for the notebook server.
   - In this case, **you should submit a job** to start the notebook on a compute node, especially for long-running or resource-intensive tasks.

---

### **Submitting a Job for Jupyter Notebook on a Compute Node**

If you are running Jupyter manually and want to submit it as a job, here’s how you can do it:

#### **1. Create a SLURM Job Script to Run Jupyter Notebook**

Create a job script to request resources for Jupyter. For example, save the following as `jupyter_job.sh`:

```bash
#!/bin/bash
#SBATCH --job-name=jupyter_notebook
#SBATCH --output=jupyter_output.txt
#SBATCH --error=jupyter_error.txt
#SBATCH --time=03:00:00           # Maximum runtime (3 hours)
#SBATCH --cpus-per-task=4         # Number of CPU cores
#SBATCH --mem=16G                 # Memory request (16 GB)
#SBATCH --gres=gpu:1              # If you need a GPU (optional)

module load python/3.8            # Load Python module
source ~/my_project/my_venv/bin/activate  # Activate your virtual environment

# Start Jupyter Notebook on a specific port (8888) without opening a browser
jupyter notebook --no-browser --port=8888 --ip=0.0.0.0
```

#### **2. Submit the Job to the Scheduler**

Submit the job script to run Jupyter on a compute node:

```bash
sbatch jupyter_job.sh
```

#### **3. Forward the Port to Your Local Machine**

Once the job starts, you’ll need to forward the port from the cluster to your local machine using SSH:

```bash
ssh -N -L 8888:localhost:8888 ysanaani@beluga.alliancecan.ca
```

#### **4. Connect to Jupyter**

Now you can open a browser on your local machine and navigate to:

```
http://localhost:8888
```

Enter the token provided when Jupyter Notebook starts (you’ll see it in the `jupyter_output.txt` or the terminal where Jupyter was started).

---

### **When Should You Submit a Job Script for Jupyter?**
- **Yes**: If you're running Jupyter **manually** on the cluster and expect your notebook to use significant resources (e.g., long-running tasks, large datasets, GPUs).
- **No**: If you're using **JupyterHub** provided by the cluster, **you don't need to submit a job script**, as JupyterHub already manages resources for your notebook sessions.


