# windows-ssh-base-ami
WinRM has been historically flakey and prone to connection issues when used as a connection method. It also requires a new security group pattern to open WinRM ports rather than SSH as seen with Linux. The idea with this repository is to use packer and ansible to create a custom AMI bases on the official Windows Server 2019 AMI that installs and enables SSH on Windows so that it can be used in general but also as a connection method in future Ansible runs.

## Usage Instructions

TBD
