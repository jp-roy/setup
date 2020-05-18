Credit goes [here](https://harfangk.github.io/2016/09/18/manage-dotfiles-with-a-git-bare-repository.html)

## Setup
Git is the only dependency. The following four lines will set up the bare repository.

```bash
git init --bare $HOME/.dotfiles.git
echo 'alias dotfiles="/usr/bin/git --git-dir=$HOME/.dotfiles.git/ --work-tree=$HOME"' >> $HOME/.zshrc
source ~/.zshrc
dotfiles config --local status.showUntrackedFiles no
```

- Create a git bare repository at ~/.dotfiles.git to track files.
- Add alias setting to shell configuration file. I use zsh so it’s .zshrc. For bash, it’d be .bashrc. Note how the paths for git directory and working tree are set.
- Reload the shell setting.
- Prevent untracked files from showing up when we call dotfiles status.

That finishes the setup.
Use the aliased command from the home directory to manage files, and use git remote repo if you want to manage the files online.

## Usage

```bash
dotfiles status
dotfiles add .vimrc
dotfiles commit -m "Add vimrc"
dotfiles remote add origin https://www.github.com/username/repo.git
dotfiles push origin master
```

## Installing dotfiles to another system
It just needs two shell commands before fetching the remote repo.

```bash
echo 'alias dotfiles="/usr/bin/git --git-dir=$HOME/.dotfiles.git/ --work-tree=$HOME"' >> $HOME/.zshrc
source ~/.zshrc
echo ".dotfiles.git" >> .gitignore
git clone --bare https://www.github.com/username/repo.git $HOME/.dotfiles.git
dotfiles checkout
dotfiles config --local status.showUntrackedFiles no
```

- Creates alias to ensure that the git bare repository works without problem.
- Reload the shell setting to use that alias.
- Add .dotfiles.git directory to .gitignore to prevent recursion issues.
- Clone the repo.
- Check if it works fine. If you already have configuration files with identical names, checkout will fail. Back up and remove those files. Skip back up if you don’t need them.
- Prevent untracked files from showing up on dotfiles status.
