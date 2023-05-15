# GIT

## Git basics

### Configure global account information

Important to identify who you are or git will not work. If you are using GitHub, Gitea or other remote service your email and name need to match your remote accounts - otherwise, you won't get your green blocks filled.

You can also configure account information on a repo by repo basis by omitting the --global flag.

```
git config --global user.email "you@email.com"
git config --global user.name "Your Name"
```

### Init

Initialize (e.g., create) a repository. This command will create a working directory under .git

`git init`

### Status

Show the status of the local working directory (repo)

`get status`

### Add content to repo

Must add files and folders to repo before they will be tracked. Can either add individual files, a list of files, or all files.

`git add .`
`git add {list of filenames}`

### Commit

Use this command to commit your changes to the current working branch

`git commit -m "initializing a repo"`

