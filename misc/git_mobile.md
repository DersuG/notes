# Git Mobile

https://www.techrepublic.com/article/how-to-install-git-on-android/

1. install FDroid
2. install Termux from FDroid
3. setup Termux:
    1. `pkg upgrade`
    2. `pkg install git openssh`
    3. `termux-setup-storage`
4. connect to GitHub:
    1. generate ssh key: `ssh-keygen -t rsa -C "put a comment here"`
    2. copy the public key's contents to your clipboard: `cat id_rsa.pub`, then select and copy it
    3. add a new ssh key to your GitHub account (Settings -> SSH and GPG keys -> New SSH key), paste in the public key
    4. check if it worked: `ssh -T git@github.com`
5. when you clone repos, use the ssh url instead of the https url
    1. you can fix an existing repo with: `git remote set-url origin git@github.com:<username>/<project>.git`
    2. https://stackoverflow.com/questions/14762034/push-to-github-without-a-password-using-ssh-key