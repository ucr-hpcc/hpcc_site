---
type: docs
linkTitle:  SSH Keys Microsoft Windows
title: SSH Keys Microsoft Windows
weight: 3
permalink: manuals_linux-cluster_sshkeys_winos.html
aliases:
    - /manuals_linux-cluster_sshkeys_winos.html
    - /manuals_linux-cluster_sshkeys_winos
---

## SSH Keys on MS Windows

### What are SSH keys?

SSH (Secure Shell) keys are an access credential that is used in the SSH protocol.

The private key remains on the system being used to access the HPCC cluster and
is used to decrypt information that is exchanged in the transfer between the
HPCC cluster and your system.

A public key file is used to encrypt information, and is stored on your own system.
The public key file is stored on the HPCC cluster and contains a list of authorized public keys.

### Why do you need SSH keys?

HPCC supports two authentication methods; `Password+DUO` and `SSH Keys`.
The `Password+DUO` method requires a UCR NetID, if you do not have this then you will need to use `SSH keys` in order to access the HPCC cluster.

### What you need

#### MobaXterm

You will need to install `MobaXterm` in order to generate your `SSH keys` and also to transfer the keys to the cluster.

1. Download `MobaXterm` from [`here`](https://mobaxterm.mobatek.net/download-home-edition.html).
2. Unzip
3. Double click portable version of exe and run the `MobaXterm` application.

##### Persistent Home Directory

By default, MobaXterm will not have a presistent home directory, meaning any files saved there will be lost the next time you start it. To use a persistent home directory, go to "Setting > General > Persistent Home Directory". If this shows "< Temp Directory >", then change it to where you would like your home directory to be stored on your computer.

##### Finding Files on Windows

To find where a file is located on windows from within a MobaXterm terminal, you can use the `open` command. For example `open .` to open the directory you are currently in, or `open ~` to open your home directory.

#### FileZilla

If you choose to upload you SSH key to the HPCC cluster with a [GUI app](https://hpcc.ucr.edu/manuals/hpc_cluster/sshkeys/sshkeys_winos/#b-gui-based-ssh-key-creation), you will need to install `FileZilla` or a similar sFTP/SCP client.
Note, FileZilla is not required if you use the [command-line approach](https://hpcc.ucr.edu/manuals/hpc_cluster/sshkeys/sshkeys_winos/#a-command-line-based-ssh-key-creation) below.

1. Download the `FileZilla Client` for Windows [here](https://filezilla-project.org/download.php?show_all=1).
        * Make sure your Windows system is updated to the latest version.
2. Follow the install wizard to complete the install of `Filezilla`.

### Create SSH Keys (`MobaXterm`)

The following provides instructions for both [(A)
command-line-based](https://hpcc.ucr.edu/manuals/hpc_cluster/sshkeys/sshkeys_winos/#a-command-line-based-ssh-key-creation)
and [(B)
GUI-based](https://hpcc.ucr.edu/manuals/hpc_cluster/sshkeys/sshkeys_winos/#b-gui-based-ssh-key-creation)
SSH key creation. Users need to choose which option is more suitable for them.
Usually, the command-line based approach is much quicker even for users without
command-line experience since it only requires to copy and paste a few lines 
of code.


#### (A) Command-line-based SSH key creation

Creating SSH keys in MobaXterm from the command-line is straightforward and
almost identical to creating SSH keys under macOS and Linux (see
[here](https://hpcc.ucr.edu/manuals/login/#ssh-keys)). To create the SSH key
pair from the command-line, open the MobaXterm 
[terminal](https://mobaxterm.mobatek.net/demo.html) and then execute the
following commands. This can be done by a simple copy and paste rather than typing, 
and then pressing the enter key. Users who wish to use WinSCP instead of FileZilla
as sFTP client need to follow the key generation instructions of this software as 
outlined [here](https://www.youtube.com/watch?v=EBrtRBI0-k0).

```sh
mkdir -p ~/.ssh # creates SSH directory
ssh-keygen -t rsa -f ~/.ssh/id_rsa # creates key pair (private and public)
```  

Next, check the content of the newly created `.ssh` directory with `ls -al
.ssh/`. It should contain files for the private and public keys that are named
`id_rsa` and `id_rsa.pub`, respectively. Importantly, this private key file should not be shared. 

Note, when using PuTTY (and WinSCP) instead of MobaXterm for generating SSH
keys, then the private key is stored in PuTTY's proprietary key format, which is
indicated by a `.ppk` file extension. A key of this format is required when using
PuTTY as SSH client, and it cannot be used with other SSH client tools. 

The public key is the one that needs to be uploaded to the remote system one
wishes to connect to. On the HPCC cluster it needs to be saved in a file
located under this location of your home directory: `~/.ssh/authorized_keys`.
The upload can be performed with an sFTP/SCP GUI app like the one built into
MobaXterm or FileZilla (see GUI section below). Copying the key from MobaXterm
into the clipboard (_e.g._ in `less`) and then pasting it into the
corresponding file opened on the remote system with a code editor like `vim` is
another but more advanced option. The following shows how to upload the private
SSH key from the command-line in MobaXterm to the HPCC cluster using the `scp`
command, where it is important that users replace `<username>` with their own
username on the HPCC cluster. Importantly, only one of the following two
commands should be used. The first one should be used if an `authorized_keys`
file does not exist yet, _e.g._ when a user configures SSH key accees on the
HPCC system for the first time. The second one should be used to append a new
public SSH key to an already existing `authorized_keys` file.

1. Create new `authorized_keys` file
    
    ```sh
    scp .ssh/id_rsa.pub <username>@cluster.hpcc.ucr.edu:.ssh/authorized_keys
    ```

2. Append SSH key to already existing `authorized_keys` file

    ```sh
    scp .ssh/id_rsa.pub <username>@cluster.hpcc.ucr.edu:tmpkey && ssh username@cluster.hpcc.ucr.edu "cat tmpkey >> ~/.ssh/authorized_keys && rm tmpkey"
    ```
Note, prior to setting up SSH key access both of the above scp commands
require functional password/DUO credentials. Users who do not have password/DUO
access (_e.g._ non-UCR users) will need to email their public SSH key to support@hpcc.ucr.edu so that the
systems admin can add their public SSH key to `~/.ssh/authorized_keys` of the corresponding
user account. 


#### (B) GUI-based SSH key creation

<font color="red"> Please use the following GUI-based instructions for generating SSH keys at your own risk. The above command-line approach is preferred since it is 
much easier and reliable.</font>

1. Begin by clicking on the tools drop down on the upper menu bar
   
   ![mobasshkey1](/img/ssh1moba.png)

2. Find and click on the MobaKeyGen (SSH key generator) option
   
   ![mobasshkey2](/img/ssh2moba.png)

3. A window should appear to create a new SSH key. Click on generate to create a new SSH key pair. Follow the on menu instructions.
   
   ![revisedkeygen](/img/revisedkeygen.png)

4. Once your key has been created, enter a password in the key passphrase field to password protect your key. Click on `conversions` in the tool bar and click on `Export OpenSSH Key`. Save this key as `id_rsa` and put the file in an easy to access location. 
Click on `Save private key` to save the private key with an extension of `.ppk` to use with MobaXterm or FileZilla. Save the key as `mobaxterm_privkey` and put the file in an easy to access location.
   
   ![revisedkeygen2](/img/revisedkeygen2.png)

5. Highlight EVERYTHING in the box labeled "Public key for pasting into OpenSSH authorized_keys file" then right-click on it and choose Copy. Open `Notepad` and paste the copied text. Save the file as `id_rsa.pub` and put the file in an easy to access location.
   
   ![revisedkeygen3](/img/revisedkeygen3.png)


### Keys Location

SSH keys should be saved under the location `C:\Users\username\.ssh`. 

   ![sshkeyloc](/img/sshkeyloc.png)



### Configure SSH Keys

#### Public SSH Key

Now that you have created your `SSH keys`, and renamed them, you will need to placed the public key (`id_rsa.pub`) on the cluster using the `cluster.hpcc.ucr.edu`

1. Start the `Filezilla` application.

2. Fill in the `Quickconnect` fields at the top of the application window:

   * Enter your HPCC username in the `Username` field.
   * Enter the HPCC servername `cluster.hpcc.ucr.edu` for the `Host` field.
   * Enter your password in the `Password` field.
   * Enter `22` in the `Port` field.

   ![filezilla1](/img/filezilla1.png)

3. Click on `Quickconnect`

   ![filezilla2](/img/filezilla2.png)

4. If the next pop up prompts you, then check the box that states `Always trust this host, add this key to the cache`, then click the `OK` button.

   ![filezilla3](/img/filezilla3.png)

5. You will need to create a `.ssh` directory to hold your SSH keys. On the right hand side, right click and click on the `Create directory option` under your home folder location.
   ![createsshdir](/img/createsshdir.png)

6. A window will appear to name the new directory. Name should be the following format: `/rhome/username/.ssh`. After naming the new directory click on `OK`.
   ![createsshdir](/img/createsshdir2.png)

7. Right click on the new `.ssh` directory that has been created. Find and click on `File permissions`.
   ![createsshdir](/img/createsshdir3.png)

8. A window with the directory permissions will appear. The `.ssh` directory needs exact permissions in order for it to function properly. Follow the image below to apply the permissions.
   ![createsshdir](/img/createsshdir4.png)

9. Now that you are connected to Filezilla transfer your public SSH key from your system by dragging the file `id_rsa.pub` and dropping it into the HPCC cluster direcotry `/rhome/username/.ssh/`.

   ![filezilla4](/img/filezilla4.png)

10. Once the file is transferred to the cluster, be sure to rename `id_rsa.pub` to `authorized_keys`.


#### Private SSH Key

Once your public key is in place, now you can configure `Filezilla` to use your private `SSH key` and connect to the cluster through the `cluster.hpcc.ucr.edu` server.

1. Open Filezilla `Site Manager` button in the top bar of icons.

   ![filezilla5](/img/filezilla5.png)

2. Click on `New Site`, rename it (optional) and press enter.

   ![filezilla6](/img/filezilla6.png)

3. Make sure the following fields are correctly filled before adding your `SSH key` file:

   * `Protocol`: should be set to `SFTP - SSH File Transfer Protocol`
   * `Host`: type in `cluster.hpcc.ucr.edu`
   * `Port`: type `22`
   * `Logon Type`: set to `Key file`
   * `User`: type in your HPCC username

   After these fields are finalized, click the `Browse..` button.

   ![filezilla7](/img/filezilla7.png)

4. Navigate to the folder you saved your private key file in and open the private key file `mobaxterm_privkey.ppk`. You should see the added keyfile in the `Key file:` box, then click `Connect`.

   ![filezilla9](/img/filezilla9.png)

5. Subsequnt connections can be done from the `Quickconnect` history by clicking on the down arrow to the right side of the `Quickconnect` button. Remember to select the `cluster.hpcc.ucr.edu` address.

   ![filezilla11](/img/filezilla11.png)

6. Transfer files by double clicking or drag-n-drop. For more details regarding file transfers vist [Filezilla Usage](some_other_page).
