# ACES interactive session
## BMIG 50303
## Horacio Gomez-Acevedo, PhD.


In this lecture, we will go through some of the details to create a conda environment in the High Performance Computing
resource for the class.

## What is conda?

It is a package manager that allows you to use different versions of python contained in a so-called *environment*.
There are different flavors for conda

![anaconda](../../Figures/fig_anaconda.png)

What is important to note is the size of the environment itself

![anaconda2](../../Figures/fig_anaconda2.png)

There is no need to downloaded locally since we will be using the available version in ACES.

## HPC cluster

In general, a High Performance Computing cluster has the following infrastructure

![hpc](../../Figures/HPCCluster.jpeg)

**Figure 1.** [Typical HPC cluster](https://www.hpc.iastate.edu/guides-old/introduction-to-hpc-clusters/what-is-an-hpc-cluster).

The **login node** is what allow us to have access to all the compute nodes, storage, etc. 
You may think of the *login node* as the orchestrator of any HPC cluster.

## ACES

Most HPC clusters have a *gentle* way to get into the *login node* and it is through a system referred to as **Open on Demand portal** . 

At this point we will need to use our [ACCESS](https://access-ci.org/) login
and go directly to the resources of ACES [portal](https://portal-aces.hprc.tamu.edu/)

![ood](../../Figures/fig_portalACES.png)

We will be using two sessions

+ One interactive session with a VCN desktop

+ A shell session to ACES login node

The shell session will help us create a conda environment that the interactive session will have access to.

### Why do we need to do that?

The interactive session (in our case the VNC desktop) is sitting inside somewhere on the compute nodes and it does not have access to the internet (for security and management reasons)
Yet, the interactive session can be totally functional provided that we supply it with the right software.


Let's create our VNC desktop session (with the default values)and make a request for such computing resources

![VNC launch](../../Figures/fig_vnc_desktop.png)

Once the resource has been allocated, we can actually access it

![VNC deploy](../../Figures/fig_vnc_desktop_launch.png)

Once you are inside your VNC you can open a terminal to get *inside* your compute resource.

We can check that there is no python isntalled when we type on the terminal 

```bash
python --version
```

![Nopython](../../Figures/fig_vnc_nopython.png)

Thus, we need to provide our interactive session with the right software. 

> Note that this is normally done before hand, this is just for demostrating purposes. 

### Creating a conda environment

TAMU knowledge base pages have a lot of useful information to setup [anaconda](https://hprc.tamu.edu/kb/Software/ANACONDA/)

First is to create a new conda environment. I will call this environment **hgenv**, but you can call it anything you want EXCEPT **base** (since there is a conflict with the default name)

The environment needs to be created on the login node. 

We move back to the portal and select 

![shell access](../../Figures/fig_access_shell.png)

> You don't need to cancel the VCN session, that is still running in the background

![shell login](../../Figures/fig_aces_login.png)

We will write some commands here with a short explanation

```bash
# we want the conda environment in the scratch directory since it has more space.
cd $SCRATCH 
# we will load the module that contains miniconda
module load Miniconda3 
# The following command is a technical one
conda config --set auto_activate_base False 
```

Now we are ready to create our environment

```bash
conda create --name hgenv

```

This environment is minimal and only contains python. We will add a couple of libraries that are useful, namely: *numpy* and *matplotlib* 

First, we need to activate our environment

```bash
source activate hgenv
```

You will notice that in your prompt at the left, it appears (hgenv). That means that our conda environment has been activated!

Now the installation of the libraries is particularly simple. The advantage is that it will install all the dependencies associated with the given library.

```bash
conda install numpy, matplotlib
```

If we are done with installing, then we can deactivate the environment. 

```bash
source deactivate
```
>This command does not destroy the conda environment, it is just deactivated and sitting in the scratch folder for future use

At this point, our job in the head node is done and we can close that tab or type *exit* on the terminal. 


### Using the newly create environment

We go back to our VNC desktop terminal and type

```bash
module load Miniconda3
source activate hgenv
```

![aces env](../../Figures/fig_aces_running_env.png)

From here we can run python and we notice that *numpy* is already available!

![aces numpy](../../Figures/fig_aces_numpy.png)

> Note that we don't need to create an environment every time, it will remain available for future use!
