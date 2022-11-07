# SSH Connection #
The most traditional method of connecting to Unity is using an SSH connection. A **shell** is what you type commands into. The most common shell in linux is `bash`, which is what you will likely be using on Unity. SSH stands for "secure shell".

## Connection Details ##

**Hostname/Address:** `unity.rc.umass.edu`

**Username:** `<USERID>_<ORGANIZATION>_edu`

* View your Unity username [here](https://unity.rc.umass.edu/panel/account.php)

## Configure SSH Keys ##
We use public/private RSA keys for authentication. You can read more about the public/private key exchange [here](https://ssd.eff.org/en/module/deep-dive-end-end-encryption-how-do-public-key-encryption-systems-work).

For the purposes of this guide, you should know that there is a public key which is stored on the server, and a private key, which you keep on your local computer. Think of them like your name and your social security number, respectively. In very basic terms, you authenticate the public key with your private key and that allows you to login to Unity.

You must save your public key on Unity by adding it in your [account settings](https://unity.rc.umass.edu/panel/account.php). If you don't already have an RSA key that you want to use, generate a new pair.

###  Generating a new Key Pair ###
If your home directory's `.ssh` folder doesn't exist, [create it](https://support.microsoft.com/en-us/office/create-a-new-folder-cbbfb6f5-59dd-4e5d-95f6-a12577952e17).

* Windows: `C:/Users/<USERNAME>/.ssh/`
* [Mac](https://www.cnet.com/tech/computing/how-to-find-your-macs-home-folder-and-add-it-to-finder/): `/Users/<USERNAME>/.ssh/`
* Linux: `/home/<USERNAME>/.ssh/`
!!! note
    You might also want to [pin the folder to quick access](https://support.microsoft.com/en-us/windows/pin-remove-and-customize-in-quick-access-7344ff13-bdf4-9f40-7f76-0b1092d2495b). We will be using this folder again for OpenSSH setup.

Click the '<red>**+**</red>' and then 'Generate Key'. The public key will be added to our database, and the private key will be downloaded to your computer.

Place this downloaded private key in your `.ssh` folder and rename it to `unity-privkey.key`.


On Linux/Mac, you will need to change the permissions on the key file due to its importance to security.
```
chmod 600 ~/.ssh/unity-privkey.key
```

It's recommended that you also add a password to the key using the following command:
```
ssh-keygen -p -f ~/.ssh/unity-privkey.key
```

## How to Connect ##
You can connect using OpenSSH (CLI) or PuTTY (Windows GUI).

### OpenSSH ###
Most modern operating systems come with the OpenSSH client, which you can use to connect to Unity in your terminal. If your Windows computer doesn't have the `ssh` command, you can [enable it](https://www.howtogeek.com/336775/how-to-enable-and-use-windows-10s-built-in-ssh-commands/).

!!! note
    In Windows, it's highly recommended that you use the [Terminal App](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701) rather than the Command Prompt `cmd`.

You can log in with:
```
ssh <UNITY_USERNAME>@unity.rc.umass.edu -i <PATH_TO_PRIVATE_KEY>
```
Or you can set up the OpenSSH config file, which will allow you to log in with:
```
ssh unity
```

### Setup OpenSSH Config File ###
#### Open a text editor (Notepad or TextEdit), and copy the following contents: ####
```
Host unity
     HostName unity.rc.umass.edu
     User <UNITY_USERNAME>
     IdentityFile <PATH_TO_PRIVATE_KEY>
```
#### Replace `<UNITY_USERNAME>` and `<PATH_TO_PRIVATE_KEY>` to your specifications. ####

* View your Unity username [here](https://unity.rc.umass.edu/panel/account.php).
* A Key downloaded during [Generating a new Key Pair](#generating-a-new-key-pair) should be located at `~/.ssh/unity-privkey.key`.

#### Save the config file to your `.ssh` folder with the name `config.txt`. ####

* This is the same folder described in [Generating a new Key Pair](#generating-a-new-key-pair).
* It's important that the file is saved in plain-text (`.txt`) format.
    * In MacOS TextEdit, you can make your current file plain-text formatted using ⌘-⇧-T, and you can also [add plain-text as a 'Save as' option](https://macreports.com/how-to-create-a-text-txt-file-on-a-mac/).

#### Remove the `.txt` file extension. ####

* In Windows, if you have file extensions hidden, [un-hide them](https://www.howtogeek.com/205086/beginner-how-to-make-windows-show-file-extensions/).
* Rename the file and remove the extension.

#### Login to Unity with the `ssh unity` command. ####

![Unity SSH](res/unity-ssh.png)

You may be prompted to confirm the ssh fingerprint.

![OpenSSH Fingerprint Prompt](res/ssh-fingerprint-prompt.png)

Unity's ECDSA fingerprint is `1mVG9GxLxyV2QVl5fR16rqy7glnKazjozSdwD9I9Hn4`.

If this matches your prompt, type `yes`.

## PuTTY ##
Windows users can use [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) to connect to Unity. Download and install PuTTY by following the link above. Be sure to select the 64 bit / 32 bit download depending on your system. Most are 64 bit, but if you are unsure 32 bit will always work.

Open PuTTY and enter hostname `unity.rc.umass.edu` on the main page

![PuTTY Host](res/putty-host.png)

On the left sidebar, navigate to connection -> data, and enter your username.

![PuTTY Username](res/putty-username.png)

On the left sidebar, navigate to connction -> ssh -> auth, and browse to your private key location.

![PuTTY SSH Key](res/putty-ssh-key.png)

Finally, in the main screen again, save the profile you created so you don't have to enter this information every time. Enter `unity` as the profile name and click 'Save'.

![PuTTY Save](res/putty-save.png)

**Log in to Unity using the 'Open' button:**

![PuTTY Open](res/putty-open.png)

You may be prompted to confirm the ssh fingerprint.

![PuTTY Fingerprint](res/putty_fingerprint.PNG)

Unity's ED25519 fingerprint is `jC7BF7h5/RJo5Svx1v+lufdf+I/ogu5dQV2sUe+y8ek`.

If this matches your prompt, click 'Accept'.
