## What is a Git repository?

A [[Git]] repository is a virtual storage of your project. It allows you to save versions of your code, which you can access when needed.
## config git

The following configuration levels are available:

- `--local`

Local configuration values are stored in a file that can be found in the repo's .git directory: `.git/config`  

-  `--global`

Global level configuration is user-specific, meaning it is applied to an operating system user. Global configuration values are stored in a file that is located in a user's home directory. `~ /.gitconfig` on Unix systems and `C:\Users\\.gitconfig` on Windows  

-  `--system`

System-level configuration is applied across an entire machine. This covers all users on an operating system and all repos.

```css
git config --global user.email "your_email@example.com"
```

This example writes the value `your_email@example.com` to the configuration name `user.email`. It uses the `--global` flag so this value is set for the current operating system user.
## aliases usage

It is important to note that there is no direct `git alias` command. Aliases are created through the use of the `git config` command and the Git configuration files. As with other configuration values, aliases can be created in a local or global scope.

To better understand Git aliases let us create some examples.

```
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

You can use alias to create new commands. For example:

```
git config --global alias.unstage 'reset HEAD --'
```

This action allows to use `git unstage` command. This makes the following two commands equivalent.

```
git unstage fileA
$ git reset HEAD -- fileA
```
## initialize repository

To create a new repo, you'll use the `git init` command. `git init` is a one-time command you use during the initial setup of a new repo. Executing this command will create a new `.git` subdirectory in your current working directory. This will also create a new main branch.

First `cd` to the root project folder and then execute the `git init` command.

```
cd /path/to/your/existing/code 
git init
```

Pointing `git init` to an existing project directory will execute the same initialization setup as mentioned above, but scoped to that project directory.

```
git init <project directory>
```
## clone repository

If a project has already been set up in a central repository, the clone command is the most common way for users to obtain a local development clone.

```
git clone <repo url>
```

