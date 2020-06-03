# Web Development setup on mac (PHP and NODE)

This is my setup when i need to setup a new mac for webdev.
It includes the basic to get php running with Valet, Node with NVM, OH MY ZSH on the terminal and the apps i use

## Apps i use

  [1password](https://1password.com/downloads/),
  [Sketch](https://www.sketchapp.com/),
  [VS Code](https://code.visualstudio.com/Download),
  [Chrome](https://www.google.com/chrome/browser/desktop),
  [Firefox](https://www.mozilla.org/en-US/firefox/new/),
  [iTerm2](https://www.iterm2.com),
  [Clipy](https://github.com/Clipy/Clipy),
  [The Unarchiver](http://unarchiver.c3.cx/unarchiver),
  [Slack](https://slack.com/downloads),
  [Whatsapp](https://www.whatsapp.com/download/),
  [Skype](https://www.skype.com/en/),
  [Spotify](https://www.spotify.com/br/download/mac/)


## Update Mac OS

Just do it. Sit back and relax, it might take a while.

## Trackpad  

System Preferences > Trackpad > Point & Click > Tap to Click [on]  
System Preferences > Trackpad > Scroll & Zoom > Scroll Direction: Natural [off]  
System Preferences > Acessibility > Pointer Control > Trackpad options > Enable Dragging [on] (without drag lock)  

## Keyboad shortcuts  
  
System Preferences > Keyboard > Shortcuts > Mission Control > Move to left space = cmd + ←  
System Preferences > Keyboard > Shortcuts > Mission Control > Move to right space = cmd + →  


## Installing Oh-my-zsh 

[http://ohmyz.sh/](http://ohmyz.sh/)

Open terminal

```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Optional: Install [Honukai Theme](https://github.com/oskarkrawczyk/honukai-iterm-zsh)


## Dock spacing

Add an empty separator to the dock.
Enter the code below in the terminal. One for each separator.

```
defaults write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}'
```

Reset the dock

```
killall Dock
```

## Xcode setup

Install Xcode via Apple's App Store

### install command line tools

```
xcode-select --install
```

## Install HomeBrew [http://brew.sh/](http://bew.sh)

Homebrew provides a system for managing graphical and command line software for macOS, enabling you to quickly install and update the tools and libraries that you need.


```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew --version
brew doctor
```

## Install the Apps

```
brew cask install \
visual-studio-code \
slack \
google-chrome \
firefox-developer-edition \
iterm2 \
1password \
sketch
```

## Install Git

```
brew install git
```

After installing git, set your github details before you create or clone repositories on a new system. This requires two commands:

```
git config --global user.name "Your Name"
git config --global user.email "you@your-domain.com"
```

## Generate SSH key

run on terminal:

```
ssh-keygen
```

Follow the prompting, and enter your passphrase.
Then run the following command (copies your ssh key to the clipboard):


```
pbcopy < ~/.ssh/id_rsa.pub
```

You can now go to GitHub, click on your profile pic, and go to settings/SSH and GPG Keys. click ‘New SSH Key’.
Give it a catchy title (or not), and add your key below.
Click ‘Add SSH Key’ and you are all set!


## NVM and Yarn for NODE

To install NVM, visit https://github.com/nvm-sh/nvm#install--update-script to follow the steps.
But at the time of writting this is the command:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

The command above will append nvm's PATH to `~/.zshrc` so make sure it exists, or any other shell rc file.

Then just install the need node version

```
nvm install stable
nvm alias default stable
```

Now you can install Yarn

```
brew install yarn --ignore-dependencies
```

And if you feel like it, you can update NPM

```
npm install npm@latest -g
```


## Composer and Valet for PHP

Follow [valet's installation guide](https://laravel.com/docs/7.x/valet#installation) to get PHP with valet working. It's quite easy.

Worth mentioning that during `install composer` you can install it globally by:

```
php PATH/TO/COMPOSER/composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

and rembember to change `PATH/TO/COMPOSER` to its correct path.  

#### After Valet is installed

```
cd ~/Dev/sites
```

Create the folders if nedeed, then run:

```
valet tld localhost
```

It will prompt for password, and then reconfigure Dnsmasq and restart php and nginx

```
valet park
```

Now every project (i.e. folder) inside `~/Dev/sites` will be served by Valet at `Http://[project-name].localhost`.

#### If you need a secure connection (TLS using HTTP/2) you can run:

```
valet secure [project-name]
```

#### Now to allow sharing sites on your Local Network


```
code /usr/local/etc/nginx/valet/valet.conf
```

change line 

```
listen 127.0.0.1:80 :default_server;
```

to:

```
listen *:80 :default_server;
```

then: 

```
valet restart
```

#### If allowing local network sharing using HTTPs

```
code ~/.config/valet/Nginx/app-name.localhost
```

change line 

```
listen 127.0.0.1:80;
```

to:

```
listen *:80;
```

then: 

```
valet restart
```

For other settings like changing php version, sharing sites via Ngrok, visit the documentation again.


## That's it. Done.
