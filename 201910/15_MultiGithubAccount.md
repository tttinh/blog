# Multiple GitHub Accounts On A Single Machine With SSH Keys

## Generating the SSH keys

Before generating an SSH key, we can check to see if we have any existing SSH keys: `ls -al ~/.ssh`. This will list out all existing public and private key pairs, if any.

If **~/.ssh/id_rsa** is available, we can reuse it, or else we can first generate a key to the default **~/.ssh/id_rsa** by running:

```bash
ssh-keygen -t rsa -C "<your first email>"
```

Let’s use this default key pair for our first account.

For the other accounts, we will create different SSH keys. The below code will generate the SSH keys, and saves the public key with the tag "other@other.com" to **~/.ssh/id_rsa_other.pub**

```bash
ssh-keygen -t rsa -C "other@other.com" -f "id_rsa_other"
```

We have two different keys created:

```text
~/.ssh/id_rsa
~/.ssh/id_rsa_other
```

## Adding the new SSH key to the corresponding GitHub account

We already have the SSH public keys ready, and we will ask our GitHub accounts to trust the keys we have created. This is to get rid of the need for typing in the username and password every time you make a Git push.

Copy the public key **~/.ssh/id_rsa.pub** and then log in to your personal GitHub account:

- Go to `Settings`
- Select `SSH and GPG keys` from the menu to the left.
- Click on `New SSH key`, provide a suitable title, and paste the key in the box below
- Click `Add key` — and you’re done!

For the other accounts, use the corresponding public keys (**~/.ssh/id_rsa_other.pub**) and repeat the above steps.

## Registering the new SSH Keys with the ssh-agent

To use the keys, we have to register them with the `ssh-agent` on our machine. Ensure `ssh-agent` is running using the command `eval "$(ssh-agent -s)"`.Add the keys to the `ssh-agent` like so:

```bash
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_other
```

## Creating the SSH config file

Here we are actually adding the SSH configuration rules for different hosts, stating which identity file to use for which domain.

The SSH config file will be available at `~/.ssh/config`. Edit it if it exists, or else we can just create it.

```bash
cd ~/.ssh/
touch config           // Creates the file if not exists
code config            // Opens the file in VS code, use any editor
```

Make configuration entries for the relevant GitHub accounts similar to the one below in your `~/.ssh/config` file:

```text
# First account - the default config
Host github.com
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa

# Second account
Host github.com-other
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_other
```

"other" is the GitHub user id for the work account.
"github.com-other" is a notation used to differentiate the multiple Git accounts. You can also use "other.github.com" notation as well. Make sure you’re consistent with what hostname notation you use. This is relevant when you clone a repository or when you set the remote origin for a local repository

The above configuration asks `ssh-agent` to:

- Use **id_rsa** as the key for any Git URL that uses `@github.com`
- Use the **id_rsa_other** key for any Git URL that uses `@github.com-other`

## Cloning repositories

Repositories can be cloned using the clone command Git provides:

```bash
git clone git@github.com:<github account>/<repo name>.git
```

The repository of other Github account will require a change to be made with this command:

```bash
git clone git@github.com-<github account>:<github account>/<repo name>.git
```

This change is made depending on the host name defined in the SSH config. The string between **@** and **:** should match what we have given in the SSH config file.

## Setting the git remote Url for the local repositories

Once we have local Git repositories cloned/created, ensure the Git config user name and email is exactly what you want. GitHub identifies the author of any commit from the email id attached with the commit description.

To list the config name and email in the local Git directory, do `git config user.name` and `git config user.email`. If it’s not found, update accordingly.

```bash
git config user.name "example"   // Updates git config user name
git config user.email "example@example.com"
```

## Note

If you get the error: `Bad owner or permissions on ~/.ssh/config` when using Git commands, that means you need to set proper permissions for your SSH config file. These commands should fix the permission problem:

```bash
chown $USER ~/.ssh/config
chmod 644 ~/.ssh/config
```

Prefix with `sudo` if the files are owned by different user (or you don't have access to them).

In `man ssh` we can read:

> Because of the potential for abuse, this file must have strict permissions: read/write for the user, and not writable by others. It may be group-writable provided that the group in question contains only the user.

## Referrences

<https://www.freecodecamp.org/news/manage-multiple-github-accounts-the-ssh-way-2dadc30ccaca/>

<https://serverfault.com/questions/253313/ssh-returns-bad-owner-or-permissions-on-ssh-config>
