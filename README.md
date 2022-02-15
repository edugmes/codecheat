# Django
## How to create superuser
`python manage.py createsuperuser` and go to [admin panel](http://127.0.0.1:8000/admin/) to test it. 

# VS Code
## [Enable colorized brackets](https://dev.to/nickytonline/native-bracket-pair-colourization-in-vs-code-3f1n)
Add to your `settings.json`:

`"editor.bracketPairColorization.enabled": true`

# Git
## A better log
Full command:
```
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
```
But you can add an alias:
```
git config --global alias.plog "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

And you can simply do a `git plog` or, to see the lines that changed, `git plog -p`.
## Update local repo
Being in the branch to be updated `git pull <remote_ref> <branch_name>`
## Show list of files in conflict during merge
`git diff --name-only --diff-filter=U`
## Edit last commit message
`git commit --amend`
## Add all except
`git add <what_to_add> :!<what_to_ignore>`

Example:

`git add . :\!psql` will add everything in the current path except `psql` (`psql` here can be either a file or folder)

# Bash beautiful
## Add to your `.bashrc`
```
PROMPT_DIRTRIM=2

parse_git_branch() {
   git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\[\033[33m\]$(parse_git_branch)\[\033[00m\]\n\e[0;31m↪ \$\e[m '
```
- PROMPT_DIRTRIM=<path_deepness>

- [Tutorial on how to change shell prompt color](https://www.cyberciti.biz/faq/bash-shell-change-the-color-of-my-shell-prompt-under-linux-or-unix/)

# WSL
## Start and stop service
Open windows bash in admin mode: `net stop LxssManager` and `net start LxssManager`