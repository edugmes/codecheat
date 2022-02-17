# Django

## How to create superuser
`python manage.py createsuperuser` and go to [admin panel](http://127.0.0.1:8000/admin/) to test it. 

## Migrations

1 - `python manage.py makemigrations` to create migration files. Prerequisite:

- have app listed in INSTALLED_APPS

- have a new attribute assigned in a model (trust me you may forget to assign variables and that'll drive you crazy)

2 - `python manage.py migrate` to apply the migrations

3 - If you want to revert to a migration: `python manage.py migrate <app_name> <migration_number_to_restore>`. Example: `python manage.py migrate my_app 0011`. Obs: no need to specify the whole migration name, just the numbers.

## Show all migrations
`python manage.py showmigrations <optional_app_name>`

## Get user model and a users
`python manage.py shell`
```
from django.contrib.auth import get_user_model

User = get_user_model()

usr = User.objects.filter(is_active=True)[0]
```

## Get all permissions
`python manage.py shell`
```
from django.contrib.auth.models import Permission
from django.contrib.auth import get_user_model

perms = set()

for bk in auth.get_backends():
    if hasattr(bk, "get_all_permissions"):
        perms.update(bk.get_all_permissions(get_user_model()(is_active=True, is_superuser=True)))

sorted(list(perms))
```
[ref](https://timonweb.com/django/how-to-get-a-list-of-all-user-permissions-available-in-django-based-project/)


## Workaround for async deadlock with multiple local daphne resquests
```
from asgiref.sync import sync_to_async
import asgiref, os, logging

if os.getenv('DEBUG') == 'True':
    logging.getLogger(__name__).info("APPLYING PATCH TO ASGIREF")
    # patch idea: https://github.com/django/channels/issues/1722#issuecomment-1032965993    
    def patch_sync_to_async(*args, **kwargs):
        """
        Monkey Patch the sync_to_async decorator
        ---------------------------------------
        ASGIRef made a change in their defaults that has caused major problems
        for channels. The decorator below needs to be updated to use
        thread_sensitive=False, thats why we are patching it on our side for now.
        https://github.com/django/channels/blob/main/channels/http.py#L220
        """
        kwargs['thread_sensitive'] = False
        return sync_to_async(*args, **kwargs)

    asgiref.sync.sync_to_async = patch_sync_to_async
```

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
## Reset to commit without losing changes
`git reset --soft <commit_hash|HEAD^N>`
## Remove from stage without losing changes
`git reset HEAD` or `git reset HEAD -- <file|directory>`

Example:

`git add . :\!psql` will add everything in the current path except `psql` (`psql` here can be either a file or folder)

# Bash beautiful
## Add to your `.bashrc`
```
PROMPT_DIRTRIM=2

parse_git_branch() {
   git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\[\033[33m\]$(parse_git_branch)\[\033[00m\]\n\e[0;31m↪ \$\e[m\]'
```
- PROMPT_DIRTRIM=<path_deepness>

- [Tutorial on how to change shell prompt color](https://www.cyberciti.biz/faq/bash-shell-change-the-color-of-my-shell-prompt-under-linux-or-unix/)

# WSL
## Start and stop service
Open windows bash in admin mode: `net stop LxssManager` and `net start LxssManager`