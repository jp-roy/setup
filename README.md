# Menu
- git
- Sublime text 3 for nostalgia
- ... before migrating to vim
- oh my zsh
- generate ssh keys
- fetch my dotfiles
- Setup git and GitHub
- Install Ruby with asdf
- Extras ...


## Install dependencies, utilities, codecs, ctags, vim, FZF...
```
sudo apt-get install -y libavcodec-extra ripgrep universal-ctags tmux libreadline-dev vim-gtk3 silversearcher-ag automake pkg-config libpcre3-dev zlib1g-dev liblzma-dev libpq-dev build-essential fonts-firacode
```

## Git

```bash
sudo apt install -y git
```

## Ubuntu inotify

Ubuntu is always tracking changes in your folders, and to do this it uses inotify.
By default the Ubuntu limit is set to 8192 files monitored.
I set it to 524288 instead :

```bash
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```

## FZF
```
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
$ ~/.fzf/install
```

# Grub boot splash screen
```
sudo vim /etc/default/grub
```

Use these vars :
```
GRUB_DEFAULT=0
GRUB_TIMEOUT_STYLE=hidden
GRUB_TIMEOUT=15
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_backlight=native thinkpad-acpi.brightness_enable=1"
GRUB_CMDLINE_LINUX=""
GRUB_GFXMODE=1024x768x32
```

Comment out the rest, save file, then :
```sudo update-grub```

### Install vim plugins :

```bash
vim
```

in Vim, type :

```bash
:PlugInstall
:PlugStatus
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

```bash
mkdir -p ~/.ssh && ssh-keygen -t ed25519 -o -a 100 -f ~/.ssh/id_ed25519 -C "jproy@grazr.me"
```

```bash
cat ~/.ssh/id_ed25519.pub
```

Send the public key to [github.com/settings/ssh](https://github.com/settings/ssh)
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

## Dotfiles (using a git bare repository)

```bash
echo 'alias dotfiles="/usr/bin/git --git-dir=$HOME/.dotfiles.git/ --work-tree=$HOME"' >> $HOME/.zshrc
source ~/.zshrc
echo ".dotfiles.git" >> .gitignore
git clone --bare git@github.com:jp-roy/dotfiles.git $HOME/.dotfiles.git
dotfiles checkout
dotfiles config --local status.showUntrackedFiles no
```

## Terminal settings

```bash
GNOME_TERMINAL_PROFILE=`gsettings get org.gnome.Terminal.ProfilesList default | awk -F \' '{print $2}'`

gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ audible-bell false
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ background-color 'rgb(38,38,38)'
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ background-transparency-percent 0
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ bold-color-same-as-fg true
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ bold-is-bright true
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ cell-height-scale 1.1500000000000001
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ cell-width-scale 1.0
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ cursor-blink-mode 'system'
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ cursor-shape 'block'
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ default-size-columns 100
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ default-size-rows 40
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ font 'Fira Code 10'
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ foreground-color 'rgb(131,148,150)'
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ highlight-colors-set false
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ palette "['rgb(7,54,66)', 'rgb(220,50,47)', 'rgb(133,153,0)', 'rgb(181,137,0)', 'rgb(38,139,210)', 'rgb(211,54,130)', 'rgb(42,161,152)', 'rgb(238,232,213)', 'rgb(0,43,54)', 'rgb(203,75,22)', 'rgb(88,110,117)', 'rgb(101,123,131)', 'rgb(131,148,150)', 'rgb(108,113,196)', 'rgb(147,161,161)', 'rgb(253,246,227)']"
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ scroll-on-keystroke true
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ scroll-on-output false
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ scrollback-unlimited true
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ scrollbar-policy 'never'
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ use-system-font false
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ use-theme-colors false
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ use-theme-transparency true
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ use-transparent-background false
gsettings set org.gnome.Terminal.Legacy.Profile:/org/gnome/terminal/legacy/profiles:/:$GNOME_TERMINAL_PROFILE/ visible-name 'JP'
```

To dump current settings :
```bash
dconf dump /org/gnome/terminal/legacy/profiles:/ > ~/.gnome-terminal-profiles.dconf
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

Creates symlink for Sublime Text 3 package configuration & settings from dotfiles
```bash
SUBL_PATH=~/.config/sublime-text-3
mkdir -p $SUBL_PATH/Packages/User $SUBL_PATH/Installed\ Packages
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

**Wait 1 minute** for additional packages to be automatically installed (New tabs with text will automatically open, containing documentation for each new package installed). To follow package installation, you can go to `View > Show console`.

close Sublime Text.

## Installing Ruby (with [asdf](https://asdf-vm.com/#/core-manage-asdf-vm) and a [zsh plugin](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/asdf))

Remove previous installations using rvm or rbenv eventually :

```bash
rvm implode && sudo rm -rf ~/.rvm
# If you got "zsh: command not found: rvm", carry on. It means `rvm` is not
# on your computer, that's what we want!

rm -rf ~/.rbenv
```

Install asdf from `https://asdf-vm.com/#/core-manage-asdf?id=install`

Then in the terminal, run the following to install ruby 2.6.2 (for example) :

```bash
 asdf install ruby 2.6.6
 
 # tell the system to use the 2.6.6 version by default
 asdf global ruby 2.6.6
```

**Close your terminal and open it again**

The following creates a `.tool-versions` file to the directory you're at when you ran it. It will contain the language and version you chose

```bash
 asdf local ruby 2.6.6
```

**Restart** your Terminal again

```bash
ruby -v
```

You should see something starting with `ruby 2.6.6p`

Install a few **gems**

```bash
gem install rake bundler rspec rubocop rubocop-performance pry pry-byebug hub colored octokit rb-readline
```

## Postgresql

```
sudo apt install -y postgresql postgresql-contrib
sudo -u postgres psql --command "CREATE ROLE `whoami` LOGIN createdb;"
```
