<!--Actually 9:50 -->

<!--9:40 5 minutes -->

<!--Hook: Think back to day 1.  Remember all that talk of the 7 layers of computer networking?  We talked briefly about the "Application" layer.  Long enough to mention that the "Application" layer indicates the type of data we're sending.  90% of the time as a web developer, you will be communicating with the HTTP or HTTPS type.  But we've already talked about sending Postgres traffic, Mongo traffic, and today we'll talk about a very special type of traffic--the type that lets you log in remotely to another machine...say, a web server.  -->

<!--Stress throughout this class to Read, Search, Ask...especially Read.  These directions are written approximately as clearly as new documentation.  Devs will need to put all the pieces of the puzzle together themselves without any external explanation. -->

# Introduction to Operations 

## Why is this important?

Troubleshooting in production often requires you to view logs on a remote machine. You may also be asked to 
make small "hotfixes" to a running machine to fix a critical bug.
 
## Objectives

*By the end of this lab, developers will be able to:*
* __Utilizee__ an SSH key
* __Log in__ to a remote server 
* __Copy files__ to a remote server
* __Review__ a file using tail

## Introduction

Servers are just computers. Webservers are often Linux-based machines and once you have access you can run 
any of the commands you're used to running on your local machine. 

Once an app gets into production, you often need to find out something from the machine where it's running,
either looking at local logs or even making small changes and restarting the system.

<!--9:53-->

<!--9:45 10 minutes -->

## SSH 

The most common way to access a webserver is to use `ssh` or secure shell. `ssh` uses the same technology as 
SSL/HTTPS to enable you to access the server over an encrypted connection. 

Let's start with a quick hello world.  Open up your terminal and connect to my extra laptop with:

`ssh wdi-devs@10.62.104.171`

This command will ask if you want to continue connecting.  Yup, we do.  It's hackin' time.

We will tell you the password out loud rather than put it in this repo.  (Security!  Keep that in mind for the future--never put passwords in a publicly accessible place.  Bad people (and robots) are everywhere.)

Type that into your terminal and...wait, did anything even change?  It sure did.  Notice that our Terminal is now logged into `Katherines-MacBook-Air`.  Poor Katherine, she's getting hacked.

Enter the `Documents` directory and leave her a friendly note.  Something like, 

`"Bwahahaha, I have access to your computer.  Consider yourself lucky I'm not Angelina Jolie from Hackers."`

Save this note into a file like `<your_name>.txt`, then make sure your file is there.  Watch as her folder blows up with all the h4x0rs in this class.  

Then `exit` to land safely back in your Terminal.

Want to set this up for your computer so you can log in from other machines on your network at work or home?  [Follow these directions](https://support.apple.com/kb/PH18726?locale=en_US).  Be very careful, though.  Whenever you open access to a machine, you should remember to follow these rules:

1. Have some sort of authentication.  Above, this is your username and password, but there are other methods (like the one below).
2. Be sure that all the people who could *potentially* reach your computer are trusted.  In the example above, we are trusting all the people on our WeWork network, but the more restricted you can be to specific addresses or networks, the better.

### ssh public keys

When possible, it is better to not share usernames or passwords at all.  One way to avoid that is to share public SSH keys that contain neither usernames or passwords in plain text.  Remember installFest?  [We totally did that](https://help.github.com/articles/generating-an-ssh-key/) during [installFest](https://github.com/den-materials/installFest/blob/master/mac-dev-tools.md).

<!--Raise your hand if you realized that's what you were doing.  -->

Once you have a public key, you can log into a machine with the same format as above: 

```bash
ssh <user-name>@<host>
```

<!--9:55 10 minutes -->

### ssh private keys

For AWS, we'll use something slightly different. AWS adds its own private key when a server is created. 
This private key is what we need to log into the server. To use the private key we'll need a slighlty different form:

```bash
ssh -i file.pem ec2-user@aws-host
```

The ``-i`` tells ssh to use the file listed after as the key.

<!--Actually 10:06 -->

For this lesson, we will be sharing a private key, for the sake of convenience.  Usually, you would get your own private key and **never share it**.  The instructor will share the key, called `WDI4_Ops_Zeb.pem`.  Put it in your `~/.ssh` folder.

One thing that you'll need to do is to make sure that the key file is read only.

How do you change the file so that only you can read the file MyCertificate.pem?
<details>
``chmod 400 MyCertificate.pem``
</details>

The default `ec2-user` is, well, `ec2-user`.

The `aws-host` is `ec2-35-163-111-237.us-west-2.compute.amazonaws.com`.

>**Note:** Like all files in your Terminal, you need to use either absolute path (`~/.ssh/WDI4_Ops_Zeb.pem`) or relative path (move to `~/.ssh` and use `WDI4_Ops_Zeb.pem`) in the command above.

__ssh gotcha__

ssh logs you in as a specifc user. Sometimes there's a single user for a server, sometimes you log in as 
yourself. This is often done for logging and permissions. In particular, you may only have read-only 
permissions, so you might get some new errors that say you can't run a command because you don't have permissions.

If you can't Google your way out of an issue like this, see if someone on your project can help you.

#### Independent practice

Log into the AWS instance above.  When prompted to connect (do you trust this AWS instance?), type `yes`.

Find the ``carmen.sandiego`` file.  Once you find it, raise your hand, and an instructor will check your answer.

<!--Actually 10:42, That ssh-i took FOREVER for devs to get -->

<!--10:05 10 minutes -->

## SCP 

Since production servers are stripped down machines, sometimes you need to transfer files to them.
The easiest way to do this is to use ``scp``.

Remember when we talked about FTP earlier when we were setting up our personal websites?  That is what `scp` uses.  We're *connecting* on so many *layers*.

The basic format is:
```bash
scp <local_file(s)> <user>@<host>:<remote_path>
```

For our AWS server we'll need:
```bash
scp -i <certificate> <local_file(s)> <user>@<host>:<remote_path>
```

You can also reverse the command to transfer files from the remote machine to your local machine:
```bash
scp -i <certificate> <user>@<host>:<remote_path> <local_path> 
```

#### Independent practice

Create a directory with a name of your GitHub username. Inside that directory put a picture of yourself. Copy this directory onto the AWS instance inside the `class` directory.  Remember how to copy a whole directory and all the items inside it?  You need the same flag to `scp` a whole directory.

Once you have uploaded the directory *to AWS*, download one of your fellow developer's directories *from AWS*.

<!--Actually 11:10 -->

<!--10:15 5 minutes -->

## Tail

OK, one more exercise.  When you're looking at production logs, sometimes it's hard to tell where the problem is.  An easy way to narrow your search is to use the ``tail`` command.  `tail` allows you to see only the most recent lines of a log file, which will usually contain the most recent error.

#### Indpendent Practice 

Use tail on the log file in the `app` folder to find a message for GA students.

<!--Actually 11:15 -->

## Licensing

All content is licensed under a CC­BY­NC­SA 4.0 license.
All software code is licensed under GNU GPLv3. For commercial use or alternative licensing, please contact legal@ga.co.
