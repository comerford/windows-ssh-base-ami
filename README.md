# windows-ssh-base-ami
WinRM has been historically flakey and prone to connection issues when used as a connection method. It also requires a new security group pattern to open WinRM ports rather than SSH as seen with Linux. SSH compatibility for Windows is mentioned in the [Ansible docs](https://docs.ansible.com/ansible/2.5/user_guide/windows_setup.html), but it is up to you to figure out how to get it working. The idea with this repository is to use packer and ansible to create a custom AMI based on the latest official Windows Server 2019 AMI that installs and enables SSH on Windows so that it can be used in general but also as a connection method in future Ansible runs.

Of course, since SSH is not yet available on this initial run we will have to use WinRM to bootstrap the host and install/configure SSH first.

## WinRM Configuration

The WinRM configuration scripts and the [ConfigureRemotingForAnsible.ps1](https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1) script used all come from the Ansible docs for using WinRM with Ansible.

## Usage Instructions

There are pre-requisites, of course. You must have a working version of Ansible, and a working version of Packer. My own usage of this has all been via WSL2 on Windows using Ubuntu 20.04 (Packer 1.6.0 and Ansible 2.9.6 for reference) - using anything else your mileage may vary.

FULL COMMANDS TBC

## SSH configuration

The main reason for creating this configuration and switching over to SSH was how flakey WinRM was for extended builds (the Unreal Engine 4 source build for example), but SSH does not solve everything magically. I still have the occasional issue, but they are far less frequent than they were with WinRM. If your connection is unreliable, then the best thing to do is set up a host in EC2 to run these commands from and use screen, tmux or similar to take your connection out of the equation.

