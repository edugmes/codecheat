# Django
## How to create superuser
[ `python manage.py createsuperuser` ] and go to [admin panel](http://127.0.0.1:8000/admin/) to test it. 

# VS Code
## [Enable colorized brackets](https://dev.to/nickytonline/native-bracket-pair-colourization-in-vs-code-3f1n)
Add to your `settings.json`:

`"editor.bracketPairColorization.enabled": true`

# Git
## Update local repo
Being in the branch to be updated `git pull <remote_ref> <branch_name>`
## Show list of files in conflict during merge
`git diff --name-only --diff-filter=U`
## Edit last commit message
`git commit --amend`