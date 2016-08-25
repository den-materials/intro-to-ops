# Introduction to Operations 

## Why is this important
Troubleshooting in production often requires you to view logs on a remote machine. You may also be asked to 
make small "hotfixes" to a running machine to fix a critical bug.
 
## Objectives
* __Create__ an SSH key
* __Log in__ to a remote server 


## Introduction
Servers are just computers. Webservers are often Linux based machines and once you have access you can run 
any of the commands you're used to on your local machine. Once an app gets into production, you often need
 to find out something about the running machine, either looking at local logs or even making small changes 
 and restarting the system.

## SSH 
The most common way to access a webserver is to use ``ssh`` or secure shell. ssh uses the same technology as 
SSL to enable you to access the server over an encrypted connection. Unlike SSL you need to need to do some 
of the heavy lifting around keys.

### ssh keys

Let's start by creating your local ssh keys.

```bash
cd ~/.ssh
# Check for id_rsa files
ssh-keygen -t rsa -C jbarela@generalassemb.ly
```

You can then distribute the ``id_rsa.pub`` file to other people like system administrators. The adminsitrators can then add your server access.


__ssh gotcha__
ssh logs you in as a specifc user. Sometimes there's a single user for server, sometimes you log in as 
yourself. This is often done for logging and permissions. In particular, you may only have read only 
permissions so you might get some new errors that say you can't run a command because you don't have permissions.

### Independent practice
Generate your private ssh key 

### ssh

Once your key has been transfered, you can start to log into the server the basic format is 

```bash
ssh <user-name>@<host>
```

For our AWS we'll use something slightly different. AWS adds its own private key when the server is used. 
This private key is what we need to log into the server. 

### Independent practice
Log into an instance find the ``carmen.sandiego`` file.

## SCP 
Since production servers are stripped down machines sometimes you need to transfer files. The easiest way to 
do this is to use ``scp``.

### Independent practice
Create a directory named by your github id inside of the class directory. Inside that directory put a copy of your headshot.

## Tail
When you're looking at production logs you can use the ``tail`` command.

### Indpendent Practice 
Use tail on ``app/logs/app.log`` to find a message for GA students.

<!-- ## Text Editors

## AWS -->



## Licensing
All content is licensed under a CC­BY­NC­SA 4.0 license.
All software code is licensed under GNU GPLv3. For commercial use or alternative licensing, please contact legal@ga.co.