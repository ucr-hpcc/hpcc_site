---
type: docs
linkTitle: SSH Keys Apple macOS 
title: SSH Keys Apple macOS 
weight: 2
permalink: manuals_linux-cluster_sshkeys_macos.html
aliases:
    - /manuals_linux-cluster_sshkeys_macos.html
    - /manuals_linux-cluster_sshkeys_macos
---

## SSH Keys on macOS

### What are SSH Keys?

SSH (Secure Shell) keys are an access credential that is used in the SSH protocol.

The private key (ie. `id_rsa`) remains on the system being used to access the HPCC cluster and is used to decrypt information that is exchanged in the transfer between the HPCC cluster and your system.

A public key (ie. `id_rsa.pub`) is used to encrypt information, and is stored on the cluster.
The authorized keys file that is stored on the HPCC cluster (ie. `~/.ssh/authorized_keys`) contains one or more public keys (ie. `id_rsa.pub`).

### Why do you need SSH Keys?

HPCC supports two authentication methods; `Password+DUO` and `SSH Keys`.
The `Password+DUO` method requires a UCR NetID, if you do not have one then you will need to use `SSH keys` in order to access the HPCC cluster.

### Creating SSH Keys from the Command-line

By far the easiest way to create SSH keys on macOS systems is from the command-line following the instructions 
[here](https://hpcc.ucr.edu/manuals/login/#ssh-keys). Users who prefer to do this in a graphical user interface 
can follow the instructions below. 

### GUI-based SSH Key Creation

#### Filezilla

You will need to install `Filezilla` in order to transfer the public SSH key to the HPCC cluster.

1. Download the `Filezilla Client` for Mac OS X [here](https://filezilla-project.org).
	* Make sure your Mac OS X system is updated to the latest version.
2. Follow the install wizard to complete the install of `Filezilla`.

#### Sourcetree

You will need to install `Sourcetree` in order to generate your `SSH keys` (or use the command line method mentioned [here](/manuals/login/#ssh-keys).

1. Download `Sourcetree` from [here](https://www.sourcetreeapp.com)
2. Click on `Download for Mac OS X`
3. Install `Sourcetree`

### Create SSH Keys (`Sourcetree`)

1. Open the `Sourcetree` application and under the top `Sourcetree` menu click on the `Preferences...` sub-menu item.

   ![fig0](/img/sshkeys_macos/41.png)
   
2. Navigate to `Accounts` category and click on `Add...`.

   ![fig0](/img/sshkeys_macos/42.png)

3. Click on `Auth Type:` and change the drop down menu from `OAuth` to `Basic`. Make sure `Protocol:` is set to `SSH` in the drop down menu.

   ![fig0](/img/sshkeys_macos/43.png)

4. Enter `id_rsa` in the `Username` field.

   ![fig0](/img/sshkeys_macos/44.png)  
 
 
5. Click the `Generate Key` button.
   
   ![fig1](/img/sshkeys_macos/50.png)  
 
6. Press `Cancel` to exit out of the window.

### SSH Keys Location

By default, your key files are created in the path: `/Users/macOSUsername/.ssh/`.

To verify that the keys were created, do the following:

1. Open a new finder window. Click on your home directory on the left side pane.
   
   ![fig1](/img/sshkeys_macos/23.png)

2. Press the 3-button combo `Command`+`Shift`+`.` together (visualized below) to see hidden folders:
   
   ![fig1](/img/sshkeys_macos/47b.png)
 
3. You will now be able to see your `.ssh` folder, open it by double-clicking.
   
   ![fig1](/img/sshkeys_macos/48.png)

4. You should see your newly generated pair of `SSH key` files in the folder. 

   ![fig1](/img/sshkeys_macos/51.png)

5. Sourcetree adds the `-Bitbucket` to the end of the `SSH key` file names. Remove this by clicking on the file you want to rename and press the `Enter` key which allows us to rename the file before the extension.

   ![fig1](/img/sshkeys_macos/52.png)

6. After you have removed the `-Bitbucket` suffix from each of the `SSH key` file names, your new `SSH key` file names should be `id_rsa` and `id_rsa.pub`.

   ![fig1](/img/sshkeys_macos/53.png)

### Configure SSH Keys

#### Public SSH Key

Now that you have created your `SSH keys`, and renamed them, you will need to place your public key (`id_rsa.pub`) on the cluster.

If you do not have a UCR NetID, or prefer not to use `Password+DUO` authentication, then email your public key (`id_rsa.pub`) to [support](mailto:support@hpcc.ucr.edu) and skip to [Private SSH Key]('#private-ssh-key').
If you already have configured [Password+DUO](/manuals/login/#passwordduo) authentication, then proceed with the following:

1. Start the `Filezilla` application.

2. Open `Site Manager` window by clicking the upper left most button in the top bar of icons.
   
   ![fig60](/img/sshkeys_macos/60.png)

2. Click on `New Site`, which will unlock the fields to the right.

   ![fig54](/img/sshkeys_macos/54.png)

3. From the newly unlocked fields in the `General` tab, fill in the following:

   * `Protocol`: `SFTP - SSH File Transfer Protocol`
   * `Host`: `cluster.hpcc.ucr.edu`
   * `Logon Type`: `Interactive`
   * `User`: Your HPCC Username

   ![new_site_general](/img/sshkeys_macos/new_site_general.png)

4. When using `Password+DUO` authentication, you must also set the maximum number of connections.
   Navigate to the `Transfer Settings` tab and set the following:

   * `Limit Number of simultaneous connections`: checked
   * `Maximum number of connections`: 1

   Then click on `Connect`.
 
   ![new_site_transfer](/img/sshkeys_macos/new_site_transfer.png)


5. If a pop up prompts you to save your password, select the `Save passwords` option, then click the `OK` button.

6. Then enter in your password for the cluster, and click `OK`.

   ![fig](/img/sshkeys_macos/connect_password.png)

7. If the next pop up prompts you, then check the box that states `Always trust this host, add this key to the cache`, then click the `OK` button.

   ![fig8](/img/sshkeys_macos/6be.png)

8. You should now see the `DUO` authentication dialog, ensure your `User` is correct then enter the number for the preferred option from the list presented, then click `OK`.

   ![fig8](/img/sshkeys_macos/connect_duo.png)

9. Now that you are connected with Filezilla, transfer your public SSH key from your MacOS system by dragging the file `/Users/macOSUsername/.ssh/id_rsa.pub` and dropping it into the HPCC cluster direcotry `/rhome/username/.ssh/`.

   ![fig10](/img/sshkeys_macos/4e.png)

  If the `/rhome/username/.ssh/` directory does not exits, create it.

10. Once the `id_rsa.pub` file is transferred to the cluster, be sure to rename it to `authorized_keys`.

#### Private SSH Key

Once your public key is in place, now you can configure `Filezilla` to use your private `SSH key` and connect to the cluster through the `cluster.hpcc.ucr.edu` server.

1. Start the `Filezilla` application

2. Open `Site Manager` window by clicking the button in the top bar of icons.

   ![fig3](/img/sshkeys_macos/60.png)

2. Click on `New Site`, rename it (optional) and press enter.

   ![fig3](/img/sshkeys_macos/54.png)

3. Fill in the following fields from the `General` tab:

   * `Protocol`: `SFTP - SSH File Transfer Protocol`
   * `Host`: `cluster.hpcc.ucr.edu`
   * `Logon Type`: `Key file`
   * `User`: Your HPCC username
   * `Key file`: `/Users/macOSUsername/.ssh/id_rsa`

   Be sure to select the previously created private key (`/Users/macOSUsername/.ssh/id_rsa`) for the `Key file` field using the `Browse...` button.

   ![fig4](/img/sshkeys_macos/56.png)

4. Navigate to the folder you saved your key file in (default location is `/Users/macOSUsername/.ssh`) and open the private key file `id_rsa`.

   ![fig4](/img/sshkeys_macos/57.png)
   
5. You should see the added keyfile in the `Key file:` box, then click `Connect`.

   ![fig5](/img/sshkeys_macos/59.png)

   Subsequnt connections can be done from the `Quickconnect` history by clicking on the down arrow to the right side of the `Quickconnect` button.

   ![fig5](/img/sshkeys_macos/61.png)

9. Remember to select the `cluster.hpcc.ucr.edu` address.

   ![fig5](/img/sshkeys_macos/62.png)

10. Transfer files by double clicking or drag-n-drop. For more details regarding file transfers vist [Filezilla Usage](some_other_page).

