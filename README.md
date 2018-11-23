# my-mac-setup

## Prerequisites

- [Homebrew](https://brew.sh/) should be installed (Command line tools for Xcode are included).

## Getting start

### #01 Initialize
- Turn off FileVault Encryption
- Sign-in Internet Accounts
- Enable all iCloud sync
- Personal Setting
  - Dock Size, Magnification, Auto Hide
  - Language
  - Keyboard
  - Short Cut Keys
- Software Updates (wait for a while)

### #02 Setup zsh and [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

- Check lastest update from Mac App Store. Sometime there have XCode Command line tools updated.
  - `softwareupdate -l`
- Install zsh
  - `brew install zsh`
- Use `vim` to edit default shells list of OSX. Add `/usr/local/bin/zsh` to the end of file.

  - Run `sudo vim /etc/shells`

  > \* Vim Note:
  > - Use arrow ← ↑ → ↓ for **cursor** navigation.
  > - Press `i` on keyboard Vim Mode change to **INSERT**
  > - Press `ESC` to **READ-ONLY** mode
  > - Press `Shift + ;` (for typing `:` _colon_) to Enter `Command Mode`
  >   - `:w` **WRITE** the opened file
  >   - `:q` **QUIT** from vim
  >   - `:!` **FORCE** to do a command
  >   - Combination
  >     - `:wq` **WRITE & QUIT**
  >     - `:q!` **FORCE QUIT**
  >   - Other **COMMAND** type `man vim`

  ```bash
    # List of acceptable shells for chpass(1).
    # Ftpd will not allow users to connect who are not using
    # one of these shells.

    /bin/bash
    /bin/csh
    /bin/ksh
    /bin/sh
    /bin/tcsh
    /bin/zsh
    /usr/local/bin/zsh # Add this line
  ```

- Then reboot your Terminal
  _(if didn't reload your terminal at here. **oh-my-zsh** can't execute command to change default shell ENV `$SHELL`)_

- Install [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

- Then reboot your Terminal again (This is for reload `$SHELL`)

- Check default shell by
  - `echo $SHELL`
  > If $SHELL is not `/usr/local/bin/zsh` You can change default shell of terminal by.
    `chsh -s /usr/local/bin/zsh`
- Reboot your's Mac (Optional but I would like)

### #03 Install brew packages

- Common

  ```bash
  brew install ack \
  curl \
  git \
  heroku/brew/heroku \
  macvim \
  mariadb \
  mycli \
  nvm \
  nginx \
  pgcli \
  postgresql \
  rbenv \
  redis \
  ruby-build \
  tree \
  sqlite \
  wget \
  yarn --without-node
  ```

- Funny things (Optional)

  ```bash
  brew install asciinema \
  asciiquarium \
  cowsay \
  figlet \
  fortune \
  sl
  ```

- Virtual Machine (Optional)
  ```bash
  brew install ansible \
  docker \
  vagrant \
  vagrant-manager \
  virtualbox
  ```

- Brew Tasks
  - `brew services start mariadb` then `mysql_secure_installation`
  - `brew services start nginx`
  - `brew services start postgresql`
  - `brew services start redis`
  - `brew cleanup`
  - `brew doctor`
  - `brew missing`

### #03 Install Casks via [caskroom/homebrew-cask](https://caskroom.github.io/)

- Common

  ```bash
  brew cask install dropbox \
  google-backup-and-sync \
  google-chrome \
  google-drive-file-stream \
  numi
  ```
- Media

  ```bash
  brew cask install handbrake \
  kap \
  vlc
  ```

- Social

  ```bash
  brew cask install caprine \
  discord \
  flume \
  steam
  ```

- Paid Apps

  ```bash
  brew cask install 1password \
  cleanmymac \
  istat-menus \
  rubymine \
  sketch \
  spotify
  ```

- Development (Optional)

  ```bash
  brew cask install bitbar \
  postman \
  psequel \
  sequel-pro \
  visual-studio-code \
  epubquicklook \
  qlcolorcode \
  qlimagesize \
  qlprettypatch \
  qlstephen \
  quicklook-csv \
  quicklook-json \
  qlmarkdown
  ```

  ```bash
  brew cask install atom \
  java8
  ```

- Brew cask Tasks
  - `brew cask cleanup`
  - `brew tap buo/cask-upgrade`
  - `brew cu -a`

### #03 Setup system files for zsh and development

- Create `.gemrc` by `vim ~/.gemrc`
  ```bash
  # ~/.gemrc

  gem: --no-ri --no-rdoc
  benchmark: false
  verbose: true
  update_sources: true
  sources:
  - http://gems.rubyforge.org/
  - http://rubygems.org/
  backtrace: true
  bulk_threshold: 1000
  ```

- Modify `~/.zshrc`, Here is example `.zshrc`
  ```bash
  export ZSH=$HOME/.oh-my-zsh

  ZSH_THEME="ys"
  HYPHEN_INSENSITIVE="true"

  # User configuration
  export LANG=en_US.UTF-8
  plugins=(osx git zsh-completions zsh-syntax-highlighting aws ruby rails bundler gem brew node npm yarn web-search)

  source $ZSH/oh-my-zsh.sh

  export PATH="/usr/local/sbin:$PATH"

  # zsh-completions
  autoload -U compinit && compinit

  # Config JAVA 8
  # export JAVA_HOME=$(/usr/libexec/java_home)

  # NVM
  export NVM_DIR=~/.nvm
  source $(brew --prefix nvm)/nvm.sh

  # rbenv for ruby
  if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi

  # ColorLS
  source $(dirname $(gem which colorls))/tab_complete.sh
  alias lc='colorls -lA --sd'

  # alias
  alias hyperconfig="code ~/.zshrc && code ~/.hyper.js"
  alias nginxconfig="code /usr/local/etc/nginx/nginx.conf"
  alias fix_maria_db="mkdir /usr/local/etc/my.cnf.d"

  RELAUNCH_FINDER="killall Finder /System/Library/CoreServices/Finder.app"
  alias show_dot_files="defaults write com.apple.finder AppleShowAllFiles YES; eval $RELAUNCH_FINDER"
  alias hide_dot_files="defaults write com.apple.finder AppleShowAllFiles NO; eval $RELAUNCH_FINDER"

  # Ref : https://github.com/mbadolato/iTerm2-Color-Schemes/blob/master/tools/screenshotTable.sh
  function color_table {
      T='awesome'   # The test text

      echo -e "\n                 40m     41m     42m     43m\
          44m     45m     46m     47m";

      for FGs in '    m' '   1m' '  30m' '1;30m' '  31m' '1;31m' '  32m' \
              '1;32m' '  33m' '1;33m' '  34m' '1;34m' '  35m' '1;35m' \
              '  36m' '1;36m' '  37m' '1;37m';
      do FG=${FGs// /}
      echo -en " $FGs \033[$FG  $T  "
      for BG in 40m 41m 42m 43m 44m 45m 46m 47m;
          do echo -en "$EINS \033[$FG\033[$BG  $T  \033[0m";
      done
      echo;
      done
      echo
  }

  # Bashhub.com Installation
  if [ -f ~/.bashhub/bashhub.zsh ]; then
      source ~/.bashhub/bashhub.zsh
  fi
  ```
- Restart Shell
- You will get an error about autolad `colorls` gem

  ```bash
  With the advent of their 1.0 release, Homebrew has decided to bundle
  the zsh completion as part of the brew installation, so we no longer
  ship it with the brew plugin; now it only has brew aliases.

  If you find that brew completion no longer works, make sure you have
  your Homebrew installation fully up to date.

  You will only see this message once.

  ERROR:  Can't find ruby library file or shared library colorls
  usage: dirname path
  ~/.zshrc:source:28: no such file or directory: /tab_complete.sh
  ```

- Install `ruby` via `rbenv`, `rails` and funny gems
  - `rbenv install 2.4.1`
  - `rbenv global 2.4.1`
  - `rbenv versions`
  - `ruby -v`
  - `gem install colorls lolcat`
  - `gem install rails` optional: `-v 5.2.0`
<br>

- Create `~/.gitconfig`

  ```bash
  [color]
        ui = true
  [user]
          name = YOUR_NAME_HERE
          email = YOUR_EMAIL_HERE
  [core]
          autocrlf = input
          excludesfile = /Users/<YOUR_ACCOUNT_NAME>/.gitignore
          editor = vim
  [pager]
          branch = cat
  [credential]
          helper = osxkeychain
  ```

- Create `~/.gitignore`

  ```bash
  # Jetbrain
  .idea/

  # Folder view configuration files
  .DS_Store
  Desktop.ini

  # Thumbnail cache files
  ._*
  Thumbs.db

  # Files that might appear on external disks
  .Spotlight-V100
  .Trashes

  # Compiled Python files
  *.pyc

  # Compiled C++ files
  *.out

  # Node
  node_modules
  ```

- Create `.vimrc`

  ```bash
  set number
  color pablo
  syntax on
  ```

- Clone zsh-plugin `zsh-completions` & `zsh-syntax-highlighting`
  - `git clone https://github.com/zsh-users/zsh-completions ~/.oh-my-zsh/custom/plugins/zsh-completions`
  - `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting`

- Reload Your Terminal

- Install node via nvm

  - `nvm ls-remote`
  - `nvm install --lts='Carbon'`
  - `nvm use default`

- Install [hyper.is](http://hyper.is/)
  - `brew cask install hyper`
  - Example hyper configuration`.hyper.js`

  ```javascript
  // See https://hyper.is#cfg for all currently supported options.

  module.exports = {
    config: {

      verminal: {
        // fontFamily: 'Knack Nerd Font',
        fontSize: 14
      },

      // 'stable' or 'canary'
      updateChannel: 'stable',
      // fontSize: 14,
      // fontFamily: 'Knack Nerd Font',
      fontFamily: 'Menlo, "DejaVu Sans Mono", Consolas, "Lucida Console", monospace',
      fontWeight: 'normal',
      fontWeightBold: 'bold',
      cursorColor: 'rgba(1, 112, 178, 0.5)',
      cursorAccentColor: '#000',
      // `'BEAM'` for |, `'UNDERLINE'` for _, `'BLOCK'` for █
      cursorShape: 'BLOCK',
      cursorBlink: true,
      foregroundColor: '#96A8B5',
      backgroundColor: '#000',
      selectionColor: 'rgba(248,28,229,0.3)',
      borderColor: '#13222E',

      css: '',
      termCSS: '',
      showHamburgerMenu: '',
      showWindowControls: '',
      padding: '12px 14px',

      colors: {
        black: '#282629',
        red: '#FF4050',
        green: '#A4CC35',
        yellow: '#FFD24A',
        blue: '#66BFFF',
        magenta: '#F553BF',
        cyan: '#26C99E',
        white: '#E0DCE0',
        lightBlack: '#474247',
        lightRed: '#F28144',
        lightGreen: '#A4CC35',
        lightYellow: '#FFD24A',
        lightBlue: '#66BFFF',
        lightMagenta: '#F553BF',
        lightCyan: '#26C99E',
        lightWhite: '#FFFCFF',
      },

      shell: '',

      // for setting shell arguments (i.e. for using interactive shellArgs: `['-i']`)
      // by default `['--login']` will be used
      shellArgs: ['--login'],

      env: {},

      bell: false,
      copyOnSelect: false,
      defaultSSHApp: true,
    },

    plugins: [
      // 'hyperpower',
      'hyperterm-close-on-left',
      'hyper-tab-icons',
      'verminal',
      // 'hyper-sync-settings'
    ],

    localPlugins: [],

    keymaps: {
      // Example
      // 'window:devtools': 'cmd+alt+o',
    },
  };
  ```

- Install [heroku/heroku-accounts](https://github.com/heroku/heroku-accounts) plugin
  - `heroku plugins:install heroku-accounts`

- [Setup multiple Github Accounts](https://code.tutsplus.com/tutorials/quick-tip-how-to-work-with-github-and-multiple-accounts--net-22574)

  ##### TL; DR
  - `ssh-keygen -t rsa -C "<YOUR_PERSONAL_EMAIL>"`
  
  ```bash
  $ ssh-keygen -t rsa -C 'YOUR_PERSONAL_EMAIL'
  Generating public/private rsa key pair.
  Enter file in which to save the key (/Users/'<YOUR_ACCOUNT_NAME>'/.ssh/id_rsa): '<press-enter>'
  Enter passphrase (empty for no passphrase): '<press-enter> or <add-your-passphrase>'
  Enter same passphrase again: '<press-enter> or <confirm-your-passphrase>'
  Your identification has been saved in /Users/'<YOUR_ACCOUNT_NAME>'/.ssh/id_rsa.
  Your public key has been saved in /Users/'<YOUR_ACCOUNT_NAME>'/.ssh/id_rsa.pub.
  ```

  - `ssh-keygen -t rsa -C "<YOUR_WORK_EMAIL>"`
  
  ```bash
  $ ssh-keygen -t rsa -C 'YOUR_WORK_EMAIL'
  Generating public/private rsa key pair.
  Enter file in which to save the key (/Users/'<YOUR_ACCOUNT_NAME>'/.ssh/id_rsa): /Users/'<YOUR_ACCOUNT_NAME>'/.ssh/id_rsa_WORK
  Enter passphrase (empty for no passphrase): '<press-enter> or <add-your-passphrase>'
  Enter same passphrase again: '<press-enter> or <add-your-passphrase>'
  Your identification has been saved in /Users/'<YOUR_ACCOUNT_NAME>'/.ssh/id_rsa_WORK.
  Your public key has been saved in /Users/'<YOUR_ACCOUNT_NAME>'/.ssh/id_rsa_WORK.pub.
  ```

  - `vim ~/.ssh/config`

  ```bash
  # ~/.ssh/config

  #Default GitHub
  Host github.com
    HostName github.com
    User git 
    IdentityFile ~/.ssh/id_rsa
  Host github-WORK
    HostName github.com
    User git 
    IdentityFile ~/.ssh/id_rsa_WORK
  ```

  - Add ssh public_key to your Github Accounts
  - Clone SSH URL example: `git clone git@github-WORK:foo/bar.git`

- Install [Bashhub](https://bashhub.com)
  - `curl -OL https://bashhub.com/setup && zsh setup && say 'Done'`

<!-- 
# Getting Start
1. On Initialize Wizard, when clean OSX installed
    - Sign-in iCloud (skip if you won't sign in now)
    - Turn off siri
    - Turn off file encryption

2. After initialize
    - System Preferences
      - General
        - Highlight color: Orange
        - [x] Use dark menu bar and dock
        - [x] Ask to keep changes when closing documents
      - Sign-in Internet Account
    - Software update
    - Setting
    - General
      - Highlight color: Orange
      - [x] Use dark menu bar and dock
      - [x] Ask to keep changes when closing documents
    - Desktop & Screen Saver
      - Set to wallpaper folder
    - Dock
      - Size: [------[ ]------------------]
      - [x] Magnification: [------------------[ ]------]
      - [x] Automatically hide and show the Dock
    - Turn off guest
-->
