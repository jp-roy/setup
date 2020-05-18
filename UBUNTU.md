# Setup instructions

Setup (largely) inspired by [Le Wagon](http://www.lewagon.org) fullstack bootcamp:

- Grab a text editor, where you will spend your days and nights... before migrating to vim
- Install a package manager
- Pimp your Terminal
- Setup git and GitHub
- Install Ruby
- Extras ...


## Install Git


```bash
sudo apt install -y git
```

## Sublime Text 3

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
```

```bash
sudo apt install -y apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt update
sudo apt install -y sublime-text
```

## Oh-my-zsh

```bash
sudo apt install -y zsh curl vim nodejs imagemagick jq
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

To make this change stick, restart your laptop

```bash
sudo reboot
```

Install zsh plugins
```bash
CURRENT_DIR=`pwd`
ZSH_PLUGINS_DIR="$HOME/.oh-my-zsh/custom/plugins"
mkdir -p "$ZSH_PLUGINS_DIR" && cd "$ZSH_PLUGINS_DIR"
if [ ! -d "$ZSH_PLUGINS_DIR/zsh-syntax-highlighting" ]; then
  echo "-----> Installing zsh plugin 'zsh-syntax-highlighting'..."
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
fi
cd "$CURRENT_DIR"
```

## Generate SSH keys
[read this article](http://sebastien.saunier.me/blog/2015/05/10/github-public-key-authentication.html) to get a better
understanding of what those keys are used for.

```bash
mkdir -p ~/.ssh && ssh-keygen -t ed25519 -o -a 100 -f ~/.ssh/id_ed25519 -C "jproy@grazr.me"
```

Then you need to give your **public** key to GitHub. Run:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy that text, then go to [github.com/settings/ssh](https://github.com/settings/ssh)

To check that this step is completed :

```bash
ssh -T git@github.com
```

If you see something like this, you're done!

```bash
# Hi --------! You've successfully authenticated, but GitHub does not provide shell access
```

If it does not work, try running this before trying again the `ssh -T` command:

```bash
ssh-add ~/.ssh/id_ed25519
```

## Dotfiles (Standard configuration)

Credit goes [here](https://harfangk.github.io/2016/09/18/manage-dotfiles-with-a-git-bare-repository.html)

```bash
echo 'alias dotfiles="/usr/bin/git --git-dir=$HOME/.dotfiles.git/ --work-tree=$HOME"' >> $HOME/.zshrc
source ~/.zshrc
echo ".dotfiles.git" >> .gitignore
git clone --bare git@github.com:jp-roy/dotfiles.git $HOME/.dotfiles.git
dotfiles checkout
dotfiles config --local status.showUntrackedFiles no
```

### Sublime Text configuration

Creates symlink for Sublime Text 3 package configuration & settings from dotfiles
```bash
SUBL_PATH=~/.config/sublime-text-3
mkdir -p $SUBL_PATH/Packages/User $SUBL_PATH/Installed\ Packages
backup "$SUBL_PATH/Packages/User/Preferences.sublime-settings"
curl https://sublime.wbond.net/Package%20Control.sublime-package > $SUBL_PATH/Installed\ Packages/Package\ Control.sublime-package
ln -s $HOME/.sublime-settings-preferences $SUBL_PATH/Packages/User/Preferences.sublime-settings
ln -s $HOME/.sublime-settings-package-control $SUBL_PATH/Packages/User/Package\ Control.sublime-settings
zsh ~/.zshrc
echo "Quit your terminal and restart it"
```

Open a new terminal and type this:

```bash
stt
```

**Close Sublime text** and open it again:

```bash
stt
```

**Wait 1 minute** for additional packages to be automatically installed (New tabs with text will automatically open, containing documentation for each new package installed). TO follow package installation, you can go to `View > Show console`.

To check if plugins are installed, open the Command Palette (`Ctrl` + `⇧` + `P` on Linux), type in `Packlist` and then `Enter`, you should see a couple of packages installed (like [Emmet](http://emmet.io/)).

If you don't, please install all of them manually. The list is referenced [here](https://github.com/lewagon/dotfiles/blob/master/Package%20Control.sublime-settings).

When it's done, you can close Sublime Text.


## Installing Ruby (with [rbenv](https://github.com/sstephenson/rbenv))

First we need to clean up any previous Ruby installation you might have:

```bash
rvm implode && sudo rm -rf ~/.rvm
# If you got "zsh: command not found: rvm", carry on. It means `rvm` is not
# on your computer, that's what we want!

rm -rf ~/.rbenv
```

Then in the terminal, run:

```bash
sudo apt install -y build-essential tklib zlib1g-dev libssl-dev libffi-dev libxml2 libxml2-dev libxslt1-dev libreadline-dev
sudo apt clean
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
```

**Close your terminal and open it again** (Alt+F4 and restart it). If you get a warning, just **ignore** it from now (Ruby is not installed yet).


Now, you are ready to install the latest ruby version, and set it as the default version.

Run this command, it will **take a while (5-10 minutes)**

```bash
rbenv install 2.6.6
```

Once the ruby installation is done, run this command to tell the system
to use the 2.6.6 version by default.

```bash
rbenv global 2.6.6
```

Then **restart** your Terminal again (close it and reopen it).

```bash
ruby -v
```

You should see something starting with `ruby 2.6.6p`. If not, ask a teacher.

## Installing some gems

```bash
gem install rake bundler rspec rubocop rubocop-performance pry pry-byebug hub colored octokit rb-readline
```

## Postgresql

```
sudo apt install -y postgresql postgresql-contrib libpq-dev build-essential
sudo -u postgres psql --command "CREATE ROLE `whoami` LOGIN createdb;"
```


## Ubuntu inotify

Ubuntu is always tracking changes in your folders, and to do this it uses inotify.
By default the Ubuntu limit is set to 8192 files monitored.

As programming involves a lot of files, we need to raise this limit.
In your terminal run:

```bash
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```

## Extras

```bash
sudo apt install libavcodec-extra -y
```

### Slack

[Install Slack for Linux (beta)](https://get.slack.help/hc/en-us/articles/212924728-Slack-for-Linux-beta-).

### FZF

```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
$ ~/.fzf/install
```

### Tmux

```bash
sudo apt-get install tmux
```

[Getting started](https://linuxize.com/post/getting-started-with-tmux/)

### Bash

[Cheat sheet](https://www.howtogeek.com/howto/ubuntu/keyboard-shortcuts-for-bash-command-shell-for-ubuntu-debian-suse-redhat-linux-etc/)
