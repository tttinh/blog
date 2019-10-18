# Setup Docker With WSL

## Configure Docker for Windows (Docker Desktop)

When install Docker Desktop please choose `Expose daemon on tcp://localhost:2375 without TLS`. This is going to allow your local WSL instance to connect locally to the Docker daemon running within Docker for Windows.

## Install Docker and Docker Compose within WSL

We will install Docker and Docker Compose inside of WSL because it’ll give us access to both CLI apps. We just won’t bother starting the Docker daemon. The following instructions are for Ubuntu 18.04:

### Install Docker

You can copy / paste all of the commands below into your WSL terminal.

```bash
# Update the apt package list.
sudo apt-get update -y

# Install Docker's package dependencies.
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

# Download and add Docker's official public PGP key.
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify the fingerprint.
sudo apt-key fingerprint 0EBFCD88

# Add the `stable` channel's Docker upstream repository.
#
# If you want to live on the edge, you can change "stable" below to "test" or
# "nightly". I highly recommend sticking with stable!
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# Update the apt package list (for the new apt repo).
sudo apt-get update -y

# Install the latest version of Docker CE.
sudo apt-get install -y docker-ce

# Allow your user to access the Docker CLI without needing root access.
sudo usermod -aG docker $USER
```

Close your terminal and open a new one so that you can run Docker without sudo.

### Install Docker Compose

We’re going to install Docker Compose using PIP instead of the pre-compiled binary on GitHub because it runs a little bit faster (both are still Python apps).

```bash
# Install Python and PIP.
sudo apt-get install -y python python-pip

# Install Docker Compose into your user's home directory.
pip install --user docker-compose
```

**The next step is to make sure $HOME/.local/bin is set on your WSL $PATH.** If it’s not there, you’ll want to add it to your $PATH. You can do that by opening up your profile file with nano ~/.profile. Then anywhere in the file, on a new line, add export PATH="$PATH:$HOME/.local/bin" and save the file.

Finally, run source ~/.profile to active your new $PATH and confirm it works by running echo $PATH. You should see it there now. Done!

I am using `zsh` so that I will update $PATH in my .zshrc file instead of .profile.

## Configure WSL to Connect to Docker for Windows

**Connect to a remote Docker daemon with this 1 liner:**

```bash
echo "export DOCKER_HOST=tcp://localhost:2375" >> ~/.bashrc && source ~/.bashrc
# or
echo "export DOCKER_HOST=tcp://localhost:2375" >> ~/.zshrc && source ~/.zshrc
```

That just adds the export line to your .bashrc file so it’s available every time you open your terminal. The source commands reloads your bash configuration so you don’t have to open a new terminal right now for it to take effect.

**Verify Everything Works**

```bash
# You should get a bunch of output about your Docker daemon.
# If you get a permission denied error, close + open your terminal and try again.
docker info

# You should get back your Docker Compose version.
docker-compose --version
```

## Ensure Volume Mounts Work

The last thing we need to do is set things up so that volume mounts work. When using WSL, Docker for Windows expects you to supply your volume paths in a format that matches this: /c/Users/nick/dev/myapp. But, WSL doesn’t work like that. Instead, it uses the /mnt/c/Users/nick/dev/myapp format.

Create and modify the new WSL configuration file:

```bash
sudo nano /etc/wsl.conf
```

Now make it look like this and save the file when you're done:

```text
[automount]
root = /
options = "metadata"
```

We need to set root = / because this will make your drives mounted at /c or /e instead of /mnt/c or /mnt/e.

The options = "metadata" line is not necessary but it will fix folder and file permissions on WSL mounts so everything isn’t 777 all the time within the WSL mounts. I highly recommend you do this!

Once you make those changes, sign out and sign back in to Windows to ensure the changes take effect. Win + L isn’t enough. You’ll need to do a full blown sign out / sign in.

Once that’s done, you’re all set. You’ll be able to access your mounts and they will work perfectly with Docker and Docker Compose without any additional adjustments. For example you’ll be able to use .:/myapp in a docker-compose.yml file, etc..

## Referrences

<https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly>
