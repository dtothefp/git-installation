# Install and Setup Git

## Installation

### Windows
[Installing Git on Windows](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git#_installing_on_windows) can be accomplished through [downloading](https://git-scm.com/download/win) the `.exe` and running a GUI installer. This will install 3 programs:
- Git Bash - Linux style environment using the Bash shell (recommended for creating SSH keys)
- Git CMD - Command Line Prompt interpreter (preferably use Git Bash or PowerShell instead)
- Git GUI - Graphical interface for git commands and branch visualization

## Generate SSH Keys for GitHub Access

### Windows
SSH keys are used to authenticate your local machine with the GitHub server in order to read from and write to GitHub without having to send a password over the network. Generating SSH keys will result in a private key that is used to encrypt/decrypt data from your local machine and a public key which is stored on GitHub and used to decrypt/encrypt data.

#### Generate Keys Using the Git GUI
The process for generating a public/private SSH key pair with the Git GUI program is outlined [here](https://pagodabox.io/docs/setting_up_ssh-windows#using-the-git-gui). The overall steps are:
- Select "Help" > "Show SSH Key"
- Select "Generate Key"
- Enter a blank passphrase twice by selecting "OK" with a blank passphrase
- Your public key will then be displayed in the window for you to copy and then paste into the GitHub interface

In your `$HOME` directory this process results in creating a `.ssh` directory containing your public/private key files. If you already have a `.ssh` directory with keys you will be prompted to specify the name of the key files to generate. To view this directory's contents:
- Git Bash or PowerShell `ls ~/.ssh`
- Command Prompt `dir .ssh` from your HOME directory

#### Generate Keys Using Git Bash
The process for generating SSH keys can also be accomplished from the Git Bash interpreter by following the instructions [here](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/). There are a few subtle differences because this tutorial assumes a Linux and not Windows environment:
- When using the `ssh-keygen` command enter your work email (i.e. "hyperloop-one.com" not "hyperlooptech.com")
- Skip the passphrase by hitting enter twice with a blank passphrase
- On the last step using the `ssh-add` command make sure to pass a lowercase `-k` as a flag rather than an uppercase as demonstrated in the tutorial.

### Add Public Key to Your GitHub User
Use the following [link](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/) to copy and add your public SSH key to your GitHub account. If not using the GUI to access your public keys, skip running the `pbcopy` command and instead output the file contents to your terminal:
```sh
cat ~/.ssh/id_rsa.pub # and then highlight the text and use `ctrl+c` to copy the contents
```
Otherwise, you can use the Git GUI to copy the public key from it's graphical interface. This key can then be added to your GitHub account by:
- accessing your User dropdown in the upper right corner menu and selecting "Settings"
- select "SSH and GPG Keys" > "New SSH Key"
- name your SSH key something descriptive, i.e. "H1 Silver Laptop Windows 10"
- paste your public key into the interface and select "Add SSH Key"

Now that your SSH key is added you should see your SSH key represented in the UI but it's contents will be obscured and will not be able to be accessed for security reasons. The will also show a "Never Used -- Read/write" status which will change once you use your SSH key to interact with GitHub for the first time.

![SSH Url](/assets/ssh-key-unused.png)

## Configuring Git Globally

### Windows
There are many common commands and configuration utilized by git that can be pre-configured and aliased in a global config file. To see where this file exists you can run the following command:
```sh
git config --list --show-origin
```

which will output data from the `.gitconfig` file created for your user's global git config:

![SSH Url](/assets/git-config-global.png)

This tells you where you config file is , generally in `/ProgramData/Git/config` and it outputs the data that is in the config file. This data applies some global defaults and aliases for your git commands. There is a `.gitconfig` [file](/.gitconfig) in this repository that adds some common aliases that might be helpful for repetitive commands, and you can optionally add these to your global config on your local machine.

## Use a Global .gitignore to Ignore Common Files

### Windows
There are some common file that you may want to ignore globally, i.e. for Windows the `Thumbs.db` file. These files can be configured to be [ignored globally](https://gist.github.com/subfuzion/db7f57fff2fb6998a16c) in all repositories and there is an [example](/.gitignore) in this repository. To configure you can add a global `.gitignore` and configure it in your global git config file:

```sh
touch ~/.gitignore # add the ignore contents to this file
git config --global core.excludesfile %USERPROFILE%\.gitignore # this add the location of the .gitignore to your config file
```

Although, this will work for your machine this may create problems if someone you are collaborating with does not have the same files ignored. Therefore, it is recommended to create a `.gitignore` on a per project basis in the root of the project repository.
