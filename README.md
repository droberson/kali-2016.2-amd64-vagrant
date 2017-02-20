# kali-2016.2-amd64-vagrant

Quickly deploy a Kali Linux 2016.2 Virtual Machine using Vagrant! This
was created mostly to save myself some time, because I typically have
Vagrant installed on my workstations.

This is a basic installation of Kali that has been apt update/apt
upgraded mid November 2016, along with the vagrant user and sudoers
access required for Vagrant to function properly. Puppet has been
installed in case you want to take advantage of the provisioning
section in the Vagrantfile. SSH is also enabled.

This image is insecure by default! The vagrant user should have its
SSH key replaced or completely removed if you wish to abandon Vagrant
and simply use an updated, pre-installed Kali Linux VM.

# Quickstart
- Install VirtualBox: https://www.virtualbox.org/wiki/Downloads

- Install vagrant: https://www.vagrantup.com/downloads.html

- $ git clone https://github.com/droberson/kali-2016.2-amd64-vagrant.git

- $ cd kali-2016.2-amd64-vagrant

- $ vagrant up

Username and password are root/toor

Adjust the number of cores and RAM in the Vagrantfile if
necessary. Defaults are 2 cores and 4GB of RAM.

# KNOWN ISSUES
- Only tested on Ubuntu 16.04 using VirtualBox 5.1 and Vagrant 1.8.3.
- Does not work out of the box on El Capitan.
## SSH Key problem on Vagrant 1.8.5

Something isn't working properly when Vagrant tries to replace the
insecure ssh key. For this reason, I've added config.ssh.insert_key =
false to the Vagrantfile.

UPDATE: I've figured out what the problem was and two solutions.

If using Vagrant from Ubuntu's packages, it is wildly out of date. For
best results, install the packages provided by Hashicorp.

If this isn't going to work for you, the actual problem is that
Vagrant 1.8.5 fails to set proper permissions on
~/.ssh/authorized_keys. Either set the permissions to 0600 through
VirtualBox console, continue to use the insecure key, or edit
vagrant-1.8.5/plugins/guests/linux/cap/public_key.rb, adding a chmod
command around line 56 like so:
```
            if test -f ~/.ssh/authorized_keys; then
              grep -v -x -f '#{remote_path}' ~/.ssh/authorized_keys > ~/.ssh/authorized_keys.tmp
              mv ~/.ssh/authorized_keys.tmp ~/.ssh/authorized_keys
              chmod 0600 ~/.ssh/authorized_keys
            fi
```
