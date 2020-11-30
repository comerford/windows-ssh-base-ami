# A Windows AMI for AWS EC2 with OpenSSH installed and configured using Packer/Ansible

## WARNING - WINDOWS INSTANCES ARE NOT FREE TO RUN, THIS COSTS MONEY

This should be well known to most, but just in case someone is very new to AWS, if you run these scripts you will be charged. Windows hosts, particularly large ones can run up costs quickly. You do this at your own risk (as mentioned in the MIT license this software is provided as-is with no warranty etc.)

With that out of the way....

# Why use SSH at all, what about WinRM

In my experience WinRM has been flakey and prone to error when used as a connection method for automating windows builds. It also requires a new security group pattern to open WinRM ports rather than SSH as seen with Linux. SSH compatibility for Windows is mentioned in the [Ansible docs](https://docs.ansible.com/ansible/2.5/user_guide/windows_setup.html), but it is up to you to figure out how to get it working. The idea with this repository is to use packer and ansible to create a custom AMI based on the latest official Windows Server 2019 AMI that installs and enables SSH on Windows so that it can be used in general but also as a connection method in future Ansible runs.

Of course, since SSH is not yet available on this initial run we will have to use WinRM to bootstrap the host and install/configure SSH first.

## WinRM Configuration

The WinRM configuration scripts and the [ConfigureRemotingForAnsible.ps1](https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1) script used all come from the Ansible docs for using WinRM with Ansible.

## Usage Instructions

There are pre-requisites, of course. You must have an AWS account, AWS credentials configured, Ansible and Packer installed, sufficient permissions on the account to create images etc. My own usage of this has all been via WSL2 on Windows using Ubuntu 20.04 (Packer 1.6.0 and Ansible 2.9.6 for reference) - using anything else your mileage may vary.

Once you have all the pre-requisites, and you confirm that you are running this in the region you desire (the example defaults to eu-west-1) then the usage is very straightforward. You clone the repo, then from the root of the repo run:

```
cd packer
packer build 0.openssh-base.windows.packer.json 
```

That's it - once complete you will have a windows-base-openssh-timestamp AMI available in your AWS account to use as a base for subsequent builds.

To help you get started there is an example of a packer file using it to create an [image with Visual Studio 2019 installed](https://github.com/comerford/windows-ssh-base-ami/blob/main/packer/1.vs2019.windows.packer.json) for reference. Please note that this will not run as-is, the ansible playbooks are not included as they are beyond the scope of this repo. It also expects an `SSH_KEY_FILE_PATH` variable to be set, the account information has been replaced with meaningless digits etc. It still serves as a good starting/reference point.

## SSH configuration may not fix everything

The main reason for creating this configuration and switching over to SSH was how flakey WinRM was for extended builds (the Unreal Engine 4 source build for example), but SSH does not solve everything magically. I still have the occasional issue, but they are far less frequent than they were with WinRM. If your connection is unreliable, then the best thing to do is set up a host in EC2 to run these commands from and use screen, tmux or similar to take your connection out of the equation.

