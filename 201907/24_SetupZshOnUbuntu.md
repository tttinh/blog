# How to Setup ZSH and Oh-my-zsh on Ubuntu 18.04 LTS

[Home](../README.md)

Oh-my-zsh is an open source framework for managing ZSH, the Z shell. It is a community-based framework with many functions. It comes with a customizable design and has an extensive catalog of plugins aimed at system administrators and developers.

## Update and Upgrade

First update and upgrade your system.

```bash
sudo apt update
sudo apt -y upgrade
```

Confirm installation if prompted to do so.

## Install and Configure ZSH

To install `zsh` from the repository, use the following commands.

```bash
$ sudo apt install zsh
```

After the installation is complete, change the default shell of the root user to `zsh` with the `chsh` command below.

```bash
$ chsh -s $(which zsh)
```

Now logout and log in again, `zsh` installer will run and ask you for install option. Type 2 and you will get the `zsh` as default shell. Check the current shell used with the command below.

```bash
$ echo $SHELL
```

The output should be `/usr/bin/zsh`.

## Install and Configure Oh-my-zsh Framework

Oh-my-zsh provides an installer script for installing the framework, let's download the installer script and execute it.

```bash
sh -c "$(wget -O- https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Oh-my-zsh is now installed on the system, and the Z shell has been configured for using the oh-my-zsh framework with default configuration.

### Change default themes

The default `.zshrc` configuration that's provided by oh-my-zsh is using 'robbyrusell' theme. In this step, we will edit the configuration and change the default theme.

The Oh-my-zsh framework provides many themes for your zsh shell, head to the link below to take a look at the available options.

https://github.com/robbyrussell/oh-my-zsh/wiki/Themes

Alternatively, you can go to the `themes` directory and see the list of available themes.

```bash
cd ~/.oh-my-zsh/themes/
ls -a
```

In order to change the default theme, we need to edit the `.zshrc` configuration file. Edit the configuration with the vim editor.

```bash
vim ~/.zshrc
```

Pick one zsh theme - let's say 'risto' theme. Then change the 'ZSH_THEME' line 10 with 'risto' theme as below.

```
...
ZSH_THEME='risto'
...
```

Save and exit.

Now, reload the configuration `.zshrc` and you will see that 'risto' theme is currently used as your shell theme.

```bash
source ~/.zshrc
```

### Enable Oh-my-zsh Plugins

Oh-my-zsh offers awesome plugins. There are a lot of plugins for our environment, aimed at developers, system admins, and everyone else.

Default plugins are in the 'plugins' directory.

```bash
cd ~/.oh-my-zsh/plugins/
ls -a
```

In this step, we will tweak zsh using the 'oh-my-zsh' framework by enabling some plugins. In order to enable the plugins, we need to edit the `.zshrc` configuration file. Edit .zshrc configuration file.

```bash
vim ~/.zshrc
```

Go to the 'plugins' line 54 and add some plugins that you want to enable inside the bracket (). For example, here's the change I made in my case:

```
plugins=(git docker docker-compose kubectl helm)
```

Now, the Z shell, as well as the oh-my-zsh framework, have been installed. In addition, oh-my-zsh default theme has been changed with some plugins enabled.

### Some More Useful Plugins

These plugins aren’t installed with Oh-My-Zsh. So that we need to install it to use.

**zsh-syntax-highlighting**

Navigate to `~/.oh-my-zsh/custom/plugins` and clone the code from Github into this folder:

```bash
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting
```

You will see a folder named `zsh-syntax-highlighting`. This is the name that needs to be added to the plugins list.

**zsh-autosuggestions**

You can also use zsh-autosuggestions for command completion. It suggests commands based on your command history. Very useful! To select the proposed command, press the right arrow key.

Installation is the same as with _zsh-syntax-highlighting_:

```bash
$ git clone https://github.com/zsh-users/zsh-autosuggestions
```

And add `zsh-autosuggestions` to the plugins list.

### Change Color of OTHER_WRITABLE

After install zsh, all directories that are other-writable (o+w) and not sticky will have blue foreground with green background when you use `ls` command to list out. That is very hard to read. To change that we do the following steps:

Save the defaults color (it contains some explanation text) to file by runing the following command:

```bash
dircolors -p >> ~/.dircolors
```

Then change the 'OTHER_WRITABLE' value from **34;42**(blue foreground with green background) to **35;40**(purple foreground with black background) in the `.dircolors` file.

Add the following command at the last line in your `.zshrc` file.

```
eval `dircolors some_path/dir_colors`
```

Reload the zsh config, and we are done.

```bash
$ source ~/.zshrc
```
