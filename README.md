# SCALE-MAMBA

SCALE-MAMBA includes a custom compiler and MPC execution engine that implements a maliciously secure hybrid protocol. It supersedes the SPDZ framework. MAMBA is a Python-like language that compiles to a documented bytecode representation. SCALE implements a two-phase protocol, first offloading public-key operations to a offline, function-independent pre-processing stage, then executing an optimized hybrid protocol.

SCALE-MAMBA is developed by a team at [KU Leuven](https://homes.esat.kuleuven.be/~nsmart/SCALE/).

## Install Docker

First, Install Docker:
* Windows: *https://docs.docker.com/docker-for-windows/install/*
* MacOS: *https://docs.docker.com/docker-for-mac/install/*
* Ubuntu: *https://docs.docker.com/install/linux/docker-ce/ubuntu/*.
* Other Linux distro: check your distro's package manager.

Please make a post on Piazza if you're having trouble installing the container.

## Setup container

Clone this repo:
```
$ git clone https://github.com/Pratyush/spdz-tutorial.git && cd spdz-tutorial
```
Create a Docker image. This will take a few minutes. You only have to do this once.
```
$ docker build -t scalemamba .
```
To start the container, run:
```
$ docker run -it scalemamba 
```

Depending on your setup, you may have to prefix the above commands with `sudo`.

## Architecture
SCALE-MAMBA executes a two-phase hybrid protocol. The preprocessing and execution phases run back-to-back with a single executable. Users need to configure the parameters for the computation, compile the program with those parameters, and then run each party's role in the computation.

## Running Examples

The container setup script configures the system to use 2 players, and a 64-bit prime, so we can now write and execute programs that securely compute functions with inputs from 2 parties!

We provide a couple of simple examples: `millionaires`, and `millionaires-with-io`. The former runs on hard-coded inputs, while the latter receives player inputs during protocol execution.

Below we describe the steps to securely execute the `millionaires` program; the instructions for running `millionaires-with-io` are identical.

### compilation and execution
Compile the directory containing the program you wish to run. 
```
$ cd SCALE-MAMBA
$ ./compile.py Programs/millionaires
```
Then, open `tmux` and create a horizontal split via the shortcut `Ctrl-b "`.
This should give you two `tmux` panes, one for each party. You can switch between these panes with `Ctrl-b <up/down>`.

In the lower pane, start player 0's execution by running
```
$ ./Player.x 0 Programs/millionaires
```
Then, switch to the upper pane (via `Ctrl-b <up arrow>`), and start player 1's execution by running
```
$ ./Player.x 1 Programs/millionaires
```

## Modifying Examples
Secure code is stored in `Programs/<ex>/<ex>.mpc`. Add new programs by creating
a directory for them. Note that if you change the networking or sharing setup
(i.e. by adding another player or changing the modulus size), you'll need to
recompile the program.

Our examples are compatible with the repository as of March 2019, but we note that the project is under active development.


## Acknowledgement

This tutorial is a slight modification of the work done by [Hastings et al.](https://github.com/MPC-SoK/frameworks) for their excellent survey paper, [*"SoK: General-Purpose Compilers for Secure Multi-party Computation"*](https://marsella.github.io/static/mpcsok.pdf).
