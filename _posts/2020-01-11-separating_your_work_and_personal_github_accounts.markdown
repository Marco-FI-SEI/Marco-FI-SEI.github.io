---
layout: post
title:      "Separating your work and personal GitHub Accounts"
date:       2020-01-11 17:01:53 -0500
permalink:  separating_your_work_and_personal_github_accounts
---


When I began my journey at Flatiron School, I already had a GitHub account with some personal coding projects. Tasked with setting up our environment, I decided to create a new "Flatiron School" GitHub account as I liked the idea of keeping it as a standalone snapshot of my time there and also so that I could keep a more streamlined and concise personal GitHub, with perhaps a few cherry-picked projects from the separate account that would best showcase what I learnt. This "separation of concerns" really appealed to me.

As it turns out, achieving this wasn't as straightforward as I anticipated, so I thought it would be a worthwhile blog post. Perhaps in the future, you might like to seamlessly access more than one GitHub account (e.g. your personal and work account) from the same machine. Hopefully, this guide will help you achieve that!

To avoid having to type our credentials every time we interface with our GitHub, we can generate an SSH key on our computer (or use an existing one) which we then save in our GitHub account settings.

**Quick definition:**

*SSH stands for secure shell and it essentially exists to enable automation of online processes with an emphasis on security. It is essentially the automation of typing out credentials.*

For reference, the GitHub [documentation](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account) clearly shows how to create and add your SSH key to your account.


Now, whenever we try to push something up to our repository, for example,  the SSH key on our machine will be checked against the SSH key stored on our GitHub account. If these do not match, you’ll receive a validation error and will not be able to proceed.

**The problem:**

Once you've set up SSH authentication for one GitHub account on your machine, how do you configure your machine in a way that allows you to seamlessly interface with either GitHub account? What I mean by that is, how do you get authentication to work properly? Specifically, how can you get your machine to recognise the account you're interfacing with and serve up the correct SHA key accordingly?

When I first tackled this problem, I found that I was running into authentication errors because my machine would not serve the correct SSH key when I needed it to.

**The solution:**

Generate two SSH keys and name them something like:

1. `id_rsa_your_personal_github_username`
2. `id_rsa_your_work_github_username`

If you already had an SSH key for your personal account it may have looked something like "`~/.ssh/id_rsa`".

You can either:

a) Keep using this

b) Generate a new one with a naming convention similar to what I recommended above and replace your previous key in your GitHub (leaving the original SSH intact on your machine)

c) Delete it altogether if you don't use it for anything else. 
 
Now, you need to open up your SSH config file: `nano ~/.ssh/config` and copy this in, replacing the details as required:

```
# General
Host *
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_rsa #Override for your personal account
		
Host github.com-your_personal_github_user_name
    AddKeysToAgent yes
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_your_personal_github_username
    IdentitiesOnly yes #Override for your personal account
		
Host github.com-your_work_github_user_name
    AddKeysToAgent yes
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_your_work_github_username
    IdentitiesOnly yes
```

When you clone a repository from GitHub with SSH, you'll notice that the address looks like this:

`git@github.com:user_name/repo_name.git`

You’ll notice that this differs from the override address we create in the .ssh config file to differentiate our GitHub accounts (due to the -flags after github.com, the git@ is implicit):

`git@github.com:user_name/repo_name.git`

**VS**

`github.com-your_work_github_user_name`

`github.com-your_personal_github_user_name`

The next step is to configure git to correctly interpret these flags:

```
[url “-work_github_username:work_github_username/”]
    pushInsteadOf = :work_github_username[url “-personal_github_username:personal_github_username/”]
    pushInsteadOf = :personal_github_username
```

Before you can use your SSH keys, you need to start the ssh-agent and add your keys to it. It's important to note that this will not persist between your terminal sessions. The reason for this is it would not be secure to keep your decrypted keys saved to disk so they’re just held temporarily in memory. Keep this in mind if you run into authentication issues after a reboot or closing your terminal session etc.

To achieve all this quickly you can place the below scripts in your .zsh_rc or .bash_rc file and assign them to the appropriate alias:

`alias YOUR_ALIAS_HERE='eval ssh-agent -s && ssh-add ~/.ssh/id_rsa_your_personal_github_user_name'`

`alias YOUR_DIFFERENT_ALIAS_HERE='eval ssh-agent -s && ssh-add ~/.ssh/id_rsa_your_work_github_user_name'`

Now if I'm working with either account I just type the relevant alias in my shell and the ssh-agent will start and the required key loaded. If I'm working with both accounts in the same session, I can just run both aliases and the configuration in the SSH config file will keep things running smoothly.

Hope this helps!
