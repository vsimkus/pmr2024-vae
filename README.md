# PMR tutorial on VAEs 2021-2022

This is a repository for a computer tutorial of Probabilistic Modelling and Reasoning (2021/2022) - a University of Edinburgh master's course.

There are two notebooks provided:

* [Tutorial.ipynb](./Tutorial.ipynb) which contains some coding exercises (with the solutions provided in the text).
* [Tutorial-completed.ipynb](Tutorial-completed.ipynb) where the code has been filled-in for you so you can immediately run it on your machine.

To preview the Jupyter notebook we recommend using [nbviewer](https://nbviewer.org/github/vsimkus/pmr2022-vae/blob/main/Tutorial.ipynb) since GitHub does not properly render it (or to preview the version with the filled-in code see [this](https://nbviewer.org/github/vsimkus/pmr2022-vae/blob/main/Tutorial-completed.ipynb)).

## Installation

Before you run the experiments you will first need to setup the environment on your preferred machine. If you have previously created the `pmr` conda environment for the [HMM tutorials](https://github.com/vsimkus/pmr2022-hmm) you may now only need to update the environment (see below).

### Environments

We provide two environment files: [`environment.yml`](./environment.yml) and [`environment_cpuonly.yml`](./environment_cpuonly.yml). If you are *not* going to use CUDA (i.e. GPU) for running the experiments you may want to install/update from `environment_cpuonly.yml`, which will use significantly less storage (~2.5GB) than `environment.yml` (~6.5GB) which additionally installs the necessary dependencies for using the GPU.

You may also want to run `conda clean -t` after you've installed/updated your environment to remove the downloaded raw packages that are no longer necessary.

### Upgrading an existing environment

If you already have a `pmr` conda environment on your machine simply open the terminal and navigate to the project directory and type:

* `conda env update --file environment_cpuonly.yml` if you are not going to use the GPU
* or `conda env update --file environment.yml` if you plan to use the GPU.

### Installing on your machine

If you haven't already done so, you'll need to open terminal on your machine and then follow the below instructions

* Install git ([linux](https://git-scm.com/download/linux), [macOS](https://git-scm.com/download/mac), [windows](https://git-scm.com/download/win)) to access the repository if you don't have it already
* Clone the git repository on your machine or DICE by using `git clone` tool in the terminal (you can find a guide [here](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository))
* Once you've cloned the repository, step into the directory by entering `cd pmr2022-vae` into the terminal
* If you don’t already have it also install miniconda  ([linux](https://conda.io/projects/conda/en/latest/user-guide/install/linux.html), [macOS](https://conda.io/projects/conda/en/latest/user-guide/install/macos.html), [windows](https://conda.io/projects/conda/en/latest/user-guide/install/windows.html)), which will allow you to manage all python dependencies per project
* You can now create the `pmr` conda environment by typing `conda env create -f environment.yml` (or `conda env create -f environment_cpuonly.yml`). This step may take a while to complete and since it has to download large binaries you should better be connected to a good internet connection.

### Installing on DICE

Should you wish to use the DICE machines to run the notebook follow these alternative installation instructions

* Clone the git repository on your machine or DICE by using `git clone` tool in the terminal (you can find a guide [here](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)). Git should already be installed on your DICE machine.
* Once you've cloned the repository, step into the directory by entering `cd pmr2022-vae` into the terminal
* You may have installed miniconda for other courses (e.g. MLP), so you can use that too. If you have to install miniconda on your DICE follow the detailed explanation [here](https://github.com/VICO-UoE/mlpractical/blob/mlp2021-22/lab1/notes/environment-set-up.md#2-installing-miniconda)
* You can now create the `pmr` conda environment by typing `conda env create -f environment.yml` (or `conda env create -f environment_cpuonly.yml`). This step may take a while to complete and since it has to download large binaries you should better be connected to a good internet connection.

Make sure that you run all jobs on `student.compute`, i.e. when you start up a terminal type `ssh student.compute`.
Also note, that due to limited resources on the DICE server, at certain times it may be very slow. If you're finding yourself in this situation - come back later, or run your jobs overnight. Each of the main experiments in this tutorial should not take longer than half an hour to complete.

### Google Colab

You can access and run the notebook directly via this link <http://colab.research.google.com/github/vsimkus/pmr2022-vae>. More details can be found at <https://colab.research.google.com/github/googlecolab/colabtools/blob/master/notebooks/colab-github-demo.ipynb#scrollTo=WzIRIt9d2huC>.

Note that Colab is intended for interactive use, and hence it may time-out your notebook if you don't interact with it for a while (typically it prompts to confirm if you want to keep the notebook alive every couple of hours), so you will not be able to leave it unattended overnight. Also note that the Colab notebook already includes all the required dependencies, however, the versions may differ, hence the results may differ slightly but that should not be a problem for this tutorial.

## Starting the Jupyter server

Once you have the environment prepared you can start your jupyter notebook

* Activate the conda environment with `conda activate pmr`
* Now you will be able to start your jupyter server by typing `jupyter notebook`, which will start the server and open a browser to access the tutorial notebook. Click `Tutorial.ipynb` in the browser window. You can stop the server by pressing <kbd>Ctrl</kbd>+<kbd>c</kbd> (or <kbd>Cmd</kbd>+<kbd>c</kbd>) when you are done with it.
Note that if you're running on DICE you should be [nice](https://en.wikipedia.org/wiki/Nice_(Unix)) to each other and instead run `nice -n 19 jupyter notebook` to lower the priority of your job.

### Extra step for those using DICE

Before starting the jupyter server on a DICE server you must secure it with a passphrase, this is to prevent other people on the Informatics network from accessing your Jupyter server. Type the following in the terminal and follow the prompts (make sure you have activated your conda environment by typing `conda activate pmr` before running the following)

```bash
bash secure-notebook-server.sh
```

## Running the Jupyter notebook **remotely** on DICE

This section details how to run the tutorial on DICE remotely without physically sitting in front of an Informatics desktop. You don't need to read this if you are using one of the machines in Appleton tower.

The description in this sections is a shorter version of the detailed steps described in the [MLP course](https://github.com/VICO-UoE/mlpractical/blob/mlp2021-22/lab1/notes/remote-working-guide.md), so refer to the linked document if you need more details.

### (Prerequisite, if your are not on the University network) Connecting to the Informatics VPN

Majority of you will be connected to the University network, hence those can skip this.
If instead you want to run the experiments on the DICE machines remotely from *outside* the University network you can follow the below steps to setup.

According to the new Informatics [policy](https://blog.inf.ed.ac.uk/systems/2021/11/23/changes-to-the-ssh-service/) you will need to connect through the Informatics VPN to be able to remotely connect to the DICE machines. See <https://computing.help.inf.ed.ac.uk/openvpn> for VPN setup instructions on any OS. When the guide asks you to download ovpn configuration file, choose `Informatics-EdLAN-AT.ovpn`.

### SSH connection to student.compute

Once you are connected to Informatics network (or VPN), you can then in a different terminal window `ssh` to the DICE machines (replace `[your-student-id]` below with your student id, e.g. s1234567)

```bash
ssh -t [your-student-id]@student.ssh.inf.ed.ac.uk ssh student.compute
```

### Installation (remote DICE)

If you haven't already, follow the installation instructions for DICE above.

### Securing your notebook

Now before starting the jupyter server you must secure it with a passphrase, this is to prevent other people on the Informatics network from accessing your Jupyter server. Type the following in the terminal and follow the prompts (make sure you have activated your conda environment by typing `conda activate pmr` before running the following)

```bash
bash secure-notebook-server.sh
```

### Running the notebook

Having setup the passphrase we can now start up the server:

```bash
nice -n 19 jupyter notebook --no-browser
```

You can stop the server by pressing <kbd>Ctrl</kbd>+<kbd>c</kbd> (or <kbd>Cmd</kbd>+<kbd>c</kbd>), when you're done with it.

You should use `nice` to lower the priority of your jobs when using shared resources, such as the `student.compute` server.

Running the above command will give you an address with a port, e.g. `http://localhost:8795`, you will need to note the port address e.g. `8795` for the next step.

### Forwarding the notebook page to your machine

Now that you have a running jupyter server, the final step is to forward the service to your local machine. For a detailed guide see [this](https://github.com/VICO-UoE/mlpractical/blob/mlp2021-22/lab1/notes/remote-working-guide.md#forwarding-a-connection-to-the-notebook-server-over-ssh) which covers all operating systems. On Linux and MacOS you should just be able to run the below command in a new terminal window

```bash
ssh -N -o ProxyCommand="ssh -q [your-student-id]@student.ssh.inf.ed.ac.uk nc student.compute 22" -L [local-port]:localhost:[remote-port] [student-id]@student.compute
```

where `[local-port]` is a local port you want to assign it, e.g. `8889` and `[remote-port]` is the port you have noted before, i.e. `8795`.

You can now navigate to `http://localhost/8889` in a web browser on your local machine to access the remote Jupyter notebook. It will ask for the passphrase you have setup earlier.

Once you're done with the notebook, just press <kbd>Ctrl</kbd>+<kbd>c</kbd> (or <kbd>Cmd</kbd>+<kbd>c</kbd>) in all the terminal windows you have used until the processes are stopped and then close them.

## Attributions

The tutorial in this repository was authored by [Vaidotas Šimkus](https://github.com/vsimkus) and [Michael Gutmann](https://michaelgutmann.github.io/).
