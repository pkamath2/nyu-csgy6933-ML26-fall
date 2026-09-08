# HPC Tips
For your final projects, you will most likely need to work outside of Jupyter/Python notebooks, and may need GPUs to assist in model training. For this, you can use NYU HPC, no called **Torch** (which funnily enough has nothing to do with PyTorch 🤡)! The HPC team's [documentation](https://services.rt.nyu.edu/docs/hpc/getting_started/intro/) is substantial, but here I'll provide some tips on my workflow specifically. To start, you'll need to request access to an HPC account if you don't have that already. You can follow the steps [here](https://www.nyu.edu/life/information-technology/research-computing-services/high-performance-computing/high-performance-computing-nyu-it/hpc-accounts-and-eligibility.html). If you CS advisor is not listed in the sponsor list, you can use Prof. Bello (jb2843). Once this approved, you'll be able to `ssh` into the cluster. Here are some general recommendations/tips based on what has worked for me in the past:

## Basic tips / getting started
- You will need to first configure your local SSH (configuration file in local `~/.ssh/config` on MacOS/Linux) to be able to SSH into the cluster, you can follow the tips [here](https://services.rt.nyu.edu/docs/hpc/connecting_to_hpc/connecting_to_hpc/#configuring-your-ssh-client) to set this up.
- Once you've set that up, to `ssh` into the cluster: 
`ssh <NetID>@login.torch.hpc.nyu.edu`. You will need to do dual authentication here before gaining access. Note you will need to be on the NYU VPN or main NYU network to connect. 
- Check out [this guide](https://services.rt.nyu.edu/docs/hpc/storage/intro_and_data_management/#hpc-storage-comparison-table) for storage guidelines on HPC Torch. `/scratch/<NetID>` is usually your best bet for storage here in terms of disk space and number of files allocated.
- You can move files between local and HPC using `scp`, example [here](https://services.rt.nyu.edu/docs/hpc/tutorial_intro_hpc/transferring_files_remote/#transferring-single-files-and-folders-with-scp).
- Use the `tmux` command to persist interactive shell windows on Torch (i.e., run jobs while laptop is closed)
- You can also work with remote Github repositories on remote HPC. I recommend setting up an SSH key on Github, and then you can use your normal Git workflow on HPC after doing the initial authentication.

## Submitting CPU/GPU jobs
You typically want to avoid doing anything on the "login" nodes of HPC (it is very slow and low-memory). Instead, you will need to submit `sbatch` jobs that request CPU or GPU nodes with appropriate resources, and then ssh into those nodes. An example of requesting a CPU-only node is: `sbatch --cpus-per-task=4 --mem=12GB --time=4:00:00 --account=<YOURPROJECTID>`, and for GPU `sbatch --cpus-per-task=12 --gres=gpu --mem=32GB --time=6:00:00 --account=<YOURPROJECTID>`. Note you will need to get a [project ID](https://services.rt.nyu.edu/docs/hpc/getting_started/Slurm_Accounts/creating_hpc_projects/) to request these jobs. 

Once you submit the sbatch jobs above, you can check the status of them with `squeue -u <YOURNETID>`. You will see the job status and nodes given once allocated. To run an interactive session (e.g., run a Python script on these nodes), from within the HPC session, ssh again, onto that node, simply: `ssh <NODE>`, example, `ssh cs123`. 

Use `nvidia-smi` to check your GPU utilization! ⚠️ A common issue is GPU under-utilization. You will get many emails from HPC about this, and they can kill jobs if your average 2 hour utilization is under some threshold :')


## Connecting HPC to an IDE
You can connect an IDE such as VSCode or Cursor to HPC remote access as well - I use the Remote - SSH and Remote Explorer extensions. You can ssh in through the IDE and then access your code and actively update it on the server there. The setup may be a little tricky at first, here is an example configuration you can add to your local ssh config file that your IDE can then read from:

```
Host torch-cursor
HostName cs676 <--- !! This needs to be an active compute node, have to request compute job like the CPU example above, then change this each time
ProxyJump <NETID>6@login.torch.hpc.nyu.edu
User jw3596
ServerAliveInterval 60
StrictHostKeyChecking no
UserKnownHostsFile /dev/null
```

## Getting started with a Python environment on HPC
There are many ways to do this, and the main HPC docs provide a different route. But here's my flow I've found quite easy - a nice way to manage Conda environments with singularity.

Use **singuconda**! Follow the installation instructions [here](https://github.com/beasteers/singuconda?tab=readme-ov-file#Install) which are very clear for installing singuconda.

Once you have followed the steps below, inside your working directory on HPC, you'll have `sing` and `singrw`. For installing python packages into this conda environment/using pip, use `singrw`. To activate this environment, run `./singrw`, then go ahead and install there. If you are not installing packages (e.g. most of the time, when you're running interactive jobs), use `./sing`. 

In summary, here's a simple workflow I use (once I've set up everything above) for running Python scripts/training models: 
1. `ssh` to Torch
2. Right away, `tmux` to persist your terminal window (e.g. if you close your laptop, your interactive job will keep running!)
3. Request GPU or CPU jobs with sbatch, get the active node and ssh onto that compute node. 
4. Navigate to working directory where I've set up `singuconda`.
5. Run `./sing` to start the interactive session
6. Run python code directly here, e.g. `python myscript.py`. 
7. 🪄 That's it! 🪄


Good luck!
