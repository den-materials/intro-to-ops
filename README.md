# Introduction to Operations 

## Why is this important

Troubleshooting in production often requires you to view logs on a remote machine. You may also be asked to 
make small "hotfixes" to a running machine to fix a critical bug.
 
## Objectives

*By the end of this lab, developers will be able to:*
* __Create__ an SSH key
* __Log in__ to a remote server 
* __Copy files__ to a remote server
* __Review__ a file using tail

## Introduction

Servers are just computers. Webservers are often Linux-based machines and once you have access you can run 
any of the commands you're used to running on your local machine. 

Once an app gets into production, you often need to find out something from the machine where it's running,
either looking at local logs or even making small changes and restarting the system.

## SSH 

The most common way to access a webserver is to use `ssh` or secure shell. `ssh` uses the same technology as 
SSL/HTTPS to enable you to access the server over an encrypted connection. Unlike SSL, that does a lot of work
for you, you need to do some of the heavy lifting around keys with `ssh`.

### ssh keys

Let's start by creating your local ssh keys.

```bash
cd ~/.ssh
# Check for id_rsa files
ls
# Create a file if it does not exist
ssh-keygen -t rsa -C zgirouard@generalassemb.ly
```

You can then distribute the ``id_rsa.pub`` file to other people like system administrators. The 
administrators can then give your server access. Never transfer the ``id_rsa`` to someone as this is the same 
as handing someone a sticky note with your id and password.

#### Independent practice

Generate your private ssh key, with your email.  When prompted to add a passphrase, you can leave it blank.  If you want to be super-secure, and add a passphrase, just make sure you remember it!

### ssh

Once your key has been transfered, you can log into the server. The basic format for this is: 

```bash
ssh <user-name>@<host>
```

For our AWS we'll use something slightly different. AWS adds its own private key when the server is used. 
This private key is what we need to log into the server. To use the private key we'll need a slighlty different form:

```bash
ssh -i file.pem ec2-user@aws-host
```

The ``-i`` tells ssh to use the file listed after as the key. One thing that you'll need to do is to make
sure that the file is read only.

How do you change the file so that only you read the file MyCertificate.pem?
<details>
``chmod 400 MyCertificate.pem``
</details>

__ssh gotcha__

ssh logs you in as a specifc user. Sometimes there's a single user for server, sometimes you log in as 
yourself. This is often done for logging and permissions. In particular, you may only have read only 
permissions so you might get some new errors that say you can't run a command because you don't have permissions.

#### Independent practice
Log into an instance find the ``carmen.sandiego`` file.

## SCP 
Since production servers are stripped down machines sometimes you need to transfer files. The easiest way to 
do this is to use ``scp``.

The basic format is:
```bash
scp <local_file(s)> <user>@<host>:<remote_local>
```

For our AWS server we'll need 
```bash
scp -i <certificate> <local_file(s)> <user>@<host>:<remote_local>
```

You can also reverse the system
```bash
scp -i <certificate> <user>@<host>:<remote_local> <local_file(s)> 
```

#### Independent practice
Create a directory named by your github id inside of the class directory. Inside that directory put a copy of your headshot.

## Tail
When you're looking at production logs you can use the ``tail`` command.

#### Indpendent Practice 
Use tail on the log file in the app folder to find a message for GA students.

<!-- ## Text Editors

## AWS -->



## Licensing
All content is licensed under a CC­BY­NC­SA 4.0 license.
All software code is licensed under GNU GPLv3. For commercial use or alternative licensing, please contact legal@ga.co.
