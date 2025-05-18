# GITLAB TICKET NUMBER #627833


* Generating a new SSH key for GitLab won’t affect your GitHub setup at all. You can have multiple SSH keys, each assigned to different services.
Since your SSH authentication is failing with GitLab, it looks like the key in use (id_ed25519) might not be linked to your GitLab account. 

* You can fix this by generating a new SSH key and adding it specifically to GitLab:

- Steps to Generate & Add a New SSH Key for GitLab
- Generate a new SSH key (making sure to give it a different name so it doesn't overwrite your existing GitHub key):

`ssh-keygen -t ed25519 -C "your_email@example.com"`

When prompted, save it as a unique filename, like:

`/c/Users/Bradf/.ssh/id_gitlab_ed25519`

- Add the new key to your SSH agent:

`ssh-add /c/Users/Bradf/.ssh/id_gitlab_ed25519`

- Copy the public key and add it to GitLab:

`cat /c/Users/Bradf/.ssh/id_gitlab_ed25519.pub`

- Go to GitLab → Settings → SSH Keys
- Paste the copied public key
- Save it

- Create an SSH config file to differentiate GitHub & GitLab keys:

`nano ~/.ssh/config`

- Add the following:

```bash
Host gitlab.com
IdentityFile /c/Users/Bradf/.ssh/id_gitlab_ed25519
User git
```
- Test SSH authentication for GitLab:

`ssh -T git@gitlab.com`

- If successful, it should return:

`Welcome to GitLab, @yourusername!`

- Try the 2FA recovery command again:

`ssh git@gitlab.com 2fa_recovery_codes`


