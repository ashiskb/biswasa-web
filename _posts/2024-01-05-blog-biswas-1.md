---
title: 'Github command line with new authentication'
date: 2024-01-05
toc: true
permalink: /posts/2024/01/biswas/blog-github-command-line-with-new-authentication/
tags:
  - linux
  - ubuntu
  - ubutun22.04
  - github
  - gh
  - github-auth
  - gh-auth
  - git
---

Since 2021, github underwent a major change in their platform is removing password based authentication (in both web and command line). This blog is going to re-iterate their instructions on how you can do what you did before this overhaul with the new access mechanism in-place in the platform.

## Step 1: Create Personal Access Tokens
* You can use a personal access token in place of a password when authenticating to GitHub in the command line or with the API.
* **Warning**: Treat your access tokens like passwords.
* Go to Settings > Developer Settings > Personal access tokens > click `Tokens (classic)`.
* Select Generate new token, then click Generate new token (classic).
* In the "Note" field, give your token a descriptive name.
* To give your token an expiration, select Expiration, then choose a default option or click Custom to enter a date. You must give expiry date. **Warning**: do not choose `Never`.
* Select the scopes you'd like to grant this token. To use your token to access repositories from the command line, select repo. A token with no assigned scopes can only access public information
* Click Generate token.
* Write down (or copy) the generated token as it can't be seen again in this page.
* That's it. You now have a personal access token to be used for github authentication through command line.


## Step 2: Install `Github CLI: gh` to use/store authentication tokens
* `type -p curl >/dev/null || (sudo apt update && sudo apt install curl -y)`
* `curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg`
* `sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg`
* `echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null`
* `sudo apt update`
* `sudo apt install gh`
* Check if `gh` is installed by running `gh --version`

```
gh version 2.40.1 (2023-12-13)
https://github.com/cli/cli/releases/tag/v2.40.1
```



## Step 3: login at the command line with the `gh` tool

* `gh auth login`

```
? What account do you want to log into? **GitHub.com**
? What is your preferred protocol for Git operations on this host? **HTTPS**
? Authenticate Git with your GitHub credentials? **Yes**
? How would you like to authenticate GitHub CLI? **Paste an authentication token**
Tip: you can generate a Personal Access Token here https://github.com/settings/tokens
The minimum required scopes are 'repo', 'read:org', 'workflow'.
? Paste your authentication token: ****************************************
- gh config set -h github.com git_protocol https
✓ Configured git protocol
! Authentication credentials saved in plain text
✓ Logged in as `xxx`
```

## Step 4: You are ready
* you can now invoke `git` commands on your specified repository. It won't ask for password/access token again (or until the expiry date of the token passes).
* `git clone https://github.com/xxx/xyz-mnop-rstuv.git`
```

## References
* [Managing your personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
