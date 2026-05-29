# DTU HPC Jupyter Mac quickstart for normal people

Are you: 
* late with your Python-based assignment and just want to get started as soon as possible? 
* **not** a system administrator who wants to waste days setting up a VPN with double proxy and mysterious enterprise nonsense? 
* feeling lost in the maze-like `hpc.dtu.dk/?page_id=4317` structure of the support guide?
* a Mac user

Then you're in the right place. This is a dumb-safe guide to getting into the Danmarks Tekniske Universitet's HPC with the fewest steps possible.

## The dumb-safe first setup

### 1. Make an SSH key

```bash
ssh-keygen -t ed25519 -C "sXXXXXX@student.dtu.dk"
ssh-add --apple-use-keychain ~/.ssh/<file_name>
```

### 2. Install VPN client

```bash
brew install openconnect
```

Otherwise there *seems* to be a [Cisco AnyConnect client](https://net.ait.dtu.dk/vpn/) that you can use instead.

### 3. Connect to DTU VPN

```bash
sudo openconnect --protocol=anyconnect --os=win --useragent=AnyConnect --user=sXXXXXX vpn.dtu.dk
```

I had to add the  `--os=win --useragent=AnyConnect` flags to avoid the "unsupported-client" *(undocumented)* error.

### 4. Allow your SSH key on DTU HPC

```bash
ssh-copy-id sXXXXXX@login.hpc.dtu.dk
```

This will avoid you having to connect to the VPN everytime.

### 5. Create a short SSH alias

Add this in the file `~/.ssh/config`:

```ssh
Host dtu-hpc
  HostName login.hpc.dtu.dk
  User sXXXXXX
```

This allows you to quickly connect with `ssh dtu-hpc`.

### 6. Connect from VS Code

Open VS Code and make sure you have the `Remote - SSH` extension installed. Then click the extension's icon in the left bar to open its panel. 

You should see your SSH alias `dtu-hpc` there. Click the arrow button to connect. If it prompts for trust, extensions, host key confirmation, or remote setup prompts, accept them and **follow the top alerts** (easy to miss) until the remote window opens.

> Upon connection, every file or terminal you see in VS Code is on the DTU HPC login node, not your local machine.

### 7. Move off the login node

Once the bottom terminal opens on DTU HPC login node, do:

```bash
linuxsh
```

for CPU work, otherwise if you need GPU do:

```bash
a100sh
```

> Never work on the login node. The *(only)* very clear thing is that login nodes are for access, while interactive nodes are for development and testing, and long workloads should be submitted as jobs.

### 8. Install Conda

They recommend to install Miniforge or Miniconda in your own directory and to ony use `conda-forge` as channel:

```bash
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
bash Miniforge3-Linux-x86_64.sh -b -p ~/miniforge3
source ~/miniforge3/bin/activate
conda init bash
conda config --add channels conda-forge
conda config --remove channels defaults
```

### 9. Create Conda environment

```bash
conda create -n <env_name> python=3.10
conda activate <env_name>
conda install jupyter ipykernel
```

That makes the environment appear in the VS Code notebook kernel picker as normally you would do on your pc.

### 10. Open your project folder and go nuts

In VS Code, open a folder under your DTU home directory, for example:

```text
/zhome/31/0/XXXXXX/myproject
```

Don't worry, your home directory on HPC is private and persistent.


## Credits 

This guide is based on the HPC maze-like support material and especially on:

* [DTU HPC, Simple Python Project Setup and Usage Guide](https://github.com/YoungChulDK/DTU-HPC-Setup) by YoungChulDK
* **Guide to High-performance Computing** by Nicklas Hansen, Aleksander Nagaj and Anna Schibelle


## Contribution

I hope to have saved a fellow soul from having some headache. If you have any suggestions to make it better (e.g. support Windows), please let me know or feel free to open a PR with it.
