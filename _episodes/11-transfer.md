---
title: "Transfer files in and out the cluster"
teaching: 20
exercises: 10
questions:
- "How to send files in and out the cluster"
objectives:
- "Transfer files using rsync and Globus Online"
keypoints:
- "Transfer files using rsync and Globus Online"
---

## Overview
Now that you have access to the HPC system, you need to be able to transfers files/data to and from the system.  Each HPC system you encounter will have various ways to transfer files but we will focus on a few of the core methods

## Globus Online

Globus Online is the preferred method for transferring files to, from, and between Research Computing Resources. WVU is also a Globus Subscriber which provides additional services over the basic/free subscription. Globus Online offers the following advantages over traditional transfer methods (i.e. scp, sftp, rsync):

- Auto performance tuning to ensure the data is transferred as quickly as possible. One can expect a speedup of at least 2x over traditional transfer methods.
- Safe transfers by ensuring data integrity using checksum methods.
- Transfers are automatically restarted after a failed or stopped connection.
- Ability to only transfer files that have yet to be transferred (similar to rsync).
- Transfers are done in the background so users do not need to remain logged into the system.

WVU’s Globus Subscription adds the following features:
- Ability to archive/transfer data with unlimited storage to user’s Google Drive MIX Account.
- Sharing of data with others inside and outside the university.
- Sharing of data from personal workstations via Globus Connect Personal.

### Globus Online Demo

## Transferring files interactively with sftp

SFTP (Secure File Transfer Protocol) utilizes SSH to transfer files over a ftp style interface.  SFTP is an interactive way of downloading and uploading files. Let's connect to a cluster, using `sftp`- you'll notice it works the same way as SSH: 

```
sftp yourUsername@remote.computer.address
```
{: .bash}

This will start what appears to be a bash shell (though our prompt says `sftp>`). However we only
have access to a limited number of commands. We can see which commands are available with `help`:

```
sftp> help
```
{: .bash}
```
Available commands:
bye                                Quit sftp
cd path                            Change remote directory to 'path'
chgrp grp path                     Change group of file 'path' to 'grp'
chmod mode path                    Change permissions of file 'path' to 'mode'
chown own path                     Change owner of file 'path' to 'own'
df [-hi] [path]                    Display statistics for current directory or
                                   filesystem containing 'path'
exit                               Quit sftp
get [-afPpRr] remote [local]       Download file
reget [-fPpRr] remote [local]      Resume download file
reput [-fPpRr] [local] remote      Resume upload file
help                               Display this help text
lcd path                           Change local directory to 'path'
lls [ls-options [path]]            Display local directory listing
lmkdir path                        Create local directory
ln [-s] oldpath newpath            Link remote file (-s for symlink)
lpwd                               Print local working directory
ls [-1afhlnrSt] [path]             Display remote directory listing

# omitted further output for clarity
```
{: .output}

Notice the presence of multiple commands that make mention of local and remote. We are actually
connected to two computers at once (with two working directories!).

To show our remote working directory:
```
sftp> pwd
```
{: .bash}
```
Remote working directory: /global/home/yourUsername
```
{: .output}

To show our local working directory, we add an `l` in front of the command:

```
sftp> lpwd
```
{: .bash}
```
Local working directory: /home/jeff/Documents/teaching/hpc-intro
```
{: .output}

The same pattern follows for all other commands:

* `ls` shows the contents of our remote directory, while `lls` shows our local directory contents.
* `cd` changes the remote directory, `lcd` changes the local one.

To upload a file, we type `put some-file.txt` (tab-completion works here).

```
sftp> put config.toml
```
{: .bash}
```
Uploading config.toml to /global/home/yourUsername/config.toml
config.toml                                   100%  713     2.4KB/s   00:00 
```
{: .output}

To download a file we type `get some-file.txt`:

```
sftp> get config.toml
```
{: .bash}
```
Fetching /global/home/yourUsername/config.toml to config.toml
/global/home/yourUsername/config.toml                               100%  713     9.3KB/s   00:00
```
{: .output}

And we can recursively put/get files by just adding `-r`. Note that the directory needs to be
present beforehand.

```
sftp> mkdir content
sftp> put -r content/
```
{: .bash}
```
Uploading content/ to /global/home/yourUsername/content
Entering content/
content/scheduler.md              100%   11KB  21.4KB/s   00:00
content/index.md                  100% 1051     7.2KB/s   00:00
content/transferring-files.md     100% 6117    36.6KB/s   00:00
content/.transferring-files.md.sw 100%   24KB  28.4KB/s   00:00
content/cluster.md                100% 5542    35.0KB/s   00:00
content/modules.md                100%   17KB 158.0KB/s   00:00
content/resources.md              100% 1115    29.9KB/s   00:00
```
{: .output}

To quit, we type `exit` or `bye`.

### Exercise 1

Using Spruce Knob as your client, retrieve the download.txt file from 149.165.169.156 and display the contents of the file to screen.  

**Note:** Login information will be provided during class.

## Secure Copy (scp)

To copy a single file to or from the cluster, we can use `scp`. The syntax can be a little complex
for new users, but we'll break it down here:

To transfer *to* another computer:
```
[local]$ scp /path/to/local/file.txt yourUsername@remote.computer.address:/path/on/remote/computer
```
{: .bash}

To download *from* another computer:
```
[local]$ scp yourUsername@remote.computer.address:/path/on/remote/computer/file.txt /path/to/local/
```
{: .bash}

Note that we can simplify doing this by shortening our paths. On the remote computer, everything
after the `:` is relative to our home directory. We can simply just add a `:` and leave it at that
if we don't care where the file goes.

```
[local]$ scp local-file.txt yourUsername@remote.computer.address:
```
{: .bash}

To recursively copy a directory, we just add the `-r` (recursive) flag:

```
[local]$ scp -r some-local-folder/ yourUsername@remote.computer.address:target-directory/
```
{: .bash}

### Exercise 2

Copy (i.e. download) the directory `class` from 149.165.169.156 and display the contents of the directory to screen.

**Note:** Login information will be provided during class.

## Rsync

## Google Drive


{% include links.md %}
