# Markdown for Github
[Basic writing and formatting syntax](https://help.github.com/en/articles/basic-writing-and-formatting-syntax)

[Mastering Markdown](https://guides.github.com/features/mastering-markdown/)

[GitHub Flavored Markdown Spec](https://github.github.com/gfm/)

# Table of Contents
- [1 Fundamentals of Git](#1-fundamentals-of-git)
  - What is Git
  - Git Internals
- [2 Get Started on Git](#2-get-started-on-git)
  - Install Git
  - Configuring Git
  - Initializing Repository
- [3 Core of Git](#staging-area)
  - Staging Area
  - Status Command
  - Commits
  - ignoring files
  - Viewing the Log by $ git log
  - Git Branches
  - fetch / pull
  - diff
  - merge
  - tags
  - stashing
  - rebase
- [4 Remote Repositary, Github]()
  - SSH
  - remote
  - GitHub Pages
  - Contributing

# 1 Fundamentals of Git
## 1.1 What is Git
Git is a distributed **SCM (Source Control Management)** system.

- Free and open source
- Back-up of the files
- Roll-back to a specific point
- Team development (Every developor has a full copy of the project and works in parallel.)
- Fast speed and automated efficiency
- **Github**, a popular git remote repository

## 1.2 Git Internals (Git Objects)
The core of Git is a simple **key-value** data store.

### 1.2.1 Key
For accessing Git Objects, all objects have a unique, 40-character, SHA-1 hash.
- The first 2 characters: the name of the directory
- The remaining 38 characters: the name of the file 

The shortened version of the hash has 7-character.

### 1.2.2 Type of Value
3 types of Git Objects:
1. Blob
    - Git stores **zlib compressed files** (file contents) as blobs.
2. Tree
    - Git stores **directories** (folders) as trees in the file system. Git maintains 3 kinds of trees:
      1. Working directory, which contains
          - .git folder == repository, where Git stores its objects.
          - actual project files (.c, .cpp, .etc), which are original uncompressed files.
      2. Staging area(index)
      3. HEAD, which points to the most recent commit
3. Commit
    - Commit objects are **snapshots of the repository** at a given point in time.

### 1.2.3 Data of Value
- Blob & Tree. Each item in a tree contains 4 pieces of data:
  1. Permissions for the object
  2. Type of object: tree or blob
  3. Hash of the object
  4. Filename
- Commit Objest. A commit contains 4 pieces of data:
  1. Who made the commit
  2. The commit message
  3. The hash of the parent commit. All commits, except the first, have at least one parent (parent commit).
      - Commit only stores the change of the commit.
      - If the contents of a file haven't changed, Git can just point to the content in a previous commit using its hash. 
      There is no copy of the same file.
  4. The hash of the tree that contains the content of the commit.

# 2 Get Started on Git
## 2.1 Installing Git
- Download
  - [`https://git-scm.com/`](https://git-scm.com/)
  - [`https://git-scm.com/downloads`](https://git-scm.com/downloads)

- Echo the version of Git installed
  - `$ git --version`

## 2.2 Configuring Git
- 3 different levels of config
  1. system level
  2. global level
  3. local level

- Edit **global config**
  - Via `$ git config --global ...`
    - Edit user name: `$ git config --global user.name "username"`
    - Edit user email: `$ git config --global user.email "username@email.com"`
  - Via `git config --global --edit`
    - The actual file of global config on Win32: `C:\Users\username\.gitconfig`

- [Edit **local config**](#config-file-local-config)

- Generate **git-config Manual Page** and open it in the default web browser
  - `$ git config --help`<br />
  - The file actually generated on Win32 x64: `file://.../Git/mingw64/share/doc/git-doc/git-config.html`

## 2.3 Initializing Repository
- Initialize an empty Repository (folder `.git`) in Working Directory
    - `cd` to Working Directory and then `$ git init`
    - Working Directory can be created in advance by `mkdir`

- Make Working Directory and initialize Repository
    - `$ git init "repository-name"`
    - Quite Mode without any message back: `$ git init -q "repository-name"`

### `description` file (name of the repository)
- Edit the content of this file to name the repository

### `config` file (local config)
- For example, adding username to **local config**
  - Via command `$ git config --local user.name "username"`
  - Via directly editting this file, adding
    ```
    [user]
        name = username
    ```

# 3 Core of Git
## 3.1 Staging Area
Stating Area == pre-commit area:<br />
- Intermediary step between untracked and tracked files.
- Or between un-added and added content that has changed.

### 3.1.1 Status
We use `git status` command to see the state of the repository:<br />
- the changes are unstaged for commit
- the changes are staged and ready to be commited

### 3.1.2 Staging (tracking)
We use `git add` to track these untracked files.

- For example, to track everything that has changed:<br />
`$ git add .` 

### 3.1.3 Commit
Commit is to make changes which we have staged to the repository permanent. We use `git commit` command to make a commit. After that, everything we stage is committed.

Options of `git commit`:
- `-m`
  - This is a mandatory option.
  - For example, to give the message associated with the commit:<br />
  `$ git commit -m "Added the readme.md file"`

- `-am`
  - For example, to add everything and commit:<br />
  `$ git commit -am "Added the readme.md file"`

- `--amend`
  - This option is to amend a previous commit.
  - For example, to add everything changed to the stage area and amend the previous commit:<br />
  `$ git commit -a --amend`

# ignoring files

# $ git log
- For example, to view the log of commits:<br />
`$ git log`

# Remote Repositary: Github
[GitHub Guides](https://guides.github.com/)

## SSH Settings
1. Generate rsa key pair: `$ ssh-keygen`
    - Using the provided email as a label: `$ ssh-keygen -t rsa -b 4096 -C "username@email.com"`
      - `-t dsa | ecdsa | ed25519 | rsa` Specifies the type of of key to create.
      - `-b bits` Specifies the number of bits in the key to create.
      - `-C comment` Provides a new comment.
2. Use authentication agent and start ssh-agent in the background.<br />
`$ ssh-agent -s`
3. Add rsa key to ssh-agent.<br />
`$ ssh-add ./id_rsa`
4. Paste **id_rsa.pub** in **SSH and GPG keys, Settings, Github** to make our computer trusted.
5. Check if it works.<br />
`$ ssh -T git@github.com`

## Working with Remotes
**$ git remote**<br />
default remote: origin<br />
1. List out all the remote.<br />
2. Set a new remote.<br />
`$ git remote add remote-name https://github.com/user/repo.git`
3. Verify the new remote.<br />
`$ git remote -v`

**$ git push**<br />
`$ git add .`<br />
`$ git commit -m "message"`<br />
`$ git push -u remote-name master`

## Gist
Gist, another service operated by Github, a **pastebin-style** site that is for hosting **code snippets**.<br />
Gist == traditional pastebin + **version control** for code snippets + **easy forking** + SSL encryption for private pastes.

## Github Pages
- There are two types of Github Pages:
  1. User or company pages
      - Create a repository named as `user.github.io`
      - The URL will be `http//user.github.io`
  2. Project or repository pages
      - Create a branch named as `gh-pages` for the repository
      - The URL will be `http//user.github.io/repository-name/`
- We can also use a generator to create these static sites.<br />
    `https://pages.github.com/`

## Contributors
- Contribute to other open-source projects as a **contributor**
    1. Fork (make a copy) the repository where to contribute
    2. Read CONTRIBUTING.md (optional)
    3. Edit the forked repository
    4. e.g. Commit directly to the `master` branch
    5. Create a pull request
        - What file was changed
        - diff: what the changes were
        - ![](images/create_pull_request.png)

- Handle contributions from other developers as an **owner**
     - Merge pull request or
     - Close and comment without any action

# Git Branches
**$ git branch**
- Create a new branch<br />
`$ git branch branch-name`
- Pull a branch<br />
`$ git pull remote-name gh-pages`

**$ git checkout**<br />
Switch to branch<br />
`$git checkout branch-name`

