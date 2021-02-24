# Web Development setup on mac

This is my setup when i need to setup a new mac for webdev.
It includes the basics to get php running with Valet, Node with NVM, OH MY ZSH on the terminal and the apps i use

## Update Mac OS

Just do it. Sit back and relax, it might take a while.

## Trackpad  

System Preferences > Trackpad > Point & Click > Tap to Click [on]  
System Preferences > Trackpad > Scroll & Zoom > Scroll Direction: Natural [off]  
System Preferences > Acessibility > Pointer Control > Trackpad options > Enable Dragging [on] (without drag lock)  

## Keyboad shortcuts  
  
System Preferences > Keyboard > Shortcuts > Mission Control > Move to left space = cmd⌘ + ←  
System Preferences > Keyboard > Shortcuts > Mission Control > Move to right space = cmd⌘ + →  

## Mission Control

System Preferences > Mission control > Automatically rearrange spaces based on most recent use [off]


## Install the Apps

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
  [Spotify](https://www.spotify.com/br/download/mac/),
  [Insomnia](https://insomnia.rest/)  

  Optional for UI/UX: 

  [Discord](https://discord.com/),
  [Skype](https://www.skype.com/en/),
  [MongoDB Compass](https://www.mongodb.com/try/download/compass),
  [Sketch](https://www.sketch.com/get/),
  [Affinity Designer, Photo and Publisher](https://store.serif.com/en-us/sign-in)

  
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

### Command line tools

After Xcode is installed run this on your terminal

```
xcode-select --install
```

## Installing Oh-my-zsh 

[http://ohmyz.sh/](http://ohmyz.sh/)

Open terminal

```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Optional: Install [Honukai Theme](https://github.com/oskarkrawczyk/honukai-iterm-zsh)

## VS Code

To allow commands like `code file.ext` in the terminal.  

Open VSCode then open the Command Palette (`⇧⌘P`) and type `shell command` to find the `Shell Command: Install 'code' command in PATH command`.

Install these extensions

`Eslint`, `Path Intellisense`, `Prettier`, `Debugger for Chrome`, `JavaScript Debugger (Nightly)`

Optional extensions

`Live Share`, `Color Highlight`, `Live Server`, `EditorConfig for VS Code`, `DotENV`, `Material Icon Theme`, `GitLens`, `graphql`

Settings 

``` json
{
    "terminal.integrated.fontSize": 12,

    "workbench.iconTheme": "material-icon-theme",
    "workbench.startupEditor": "newUntitledFile",

    "editor.tabSize": 2,
    "editor.fontSize": 12,
    "editor.lineHeight": 17,
    "editor.fontLigatures": false,

    "explorer.compactFolders": false,
    "editor.renderLineHighlight":"gutter",
    "workbench.editor.labelFormat": "short",
    "extensions.ignoreRecommendations": true,

    "javascript.updateImportsOnFileMove.enabled": "always",
    "typescript.updateImportsOnFileMove.enabled": "never",

    "breadcrumbs.enabled": true,
    "editor.parameterHints.enabled": false,
    "explorer.confirmDragAndDrop": false,
    "explorer.confirmDelete": false,

    "editor.rulers": [80, 120],

    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },

    "files.associations": {
        ".sequelizerc": "javascript",
        ".stylelintrc": "json",
        ".prettierrc": "json"
    },

    "window.zoomLevel": 0,

    "emmet.syntaxProfiles": {
        "javascript": "jsx"
    },

    "gitlens.codeLens.recentChange.enabled": false,
    "gitlens.codeLens.authors.enabled": false,
    "gitlens.codeLens.enabled": false,

    "git.enableSmartCommit": true,

    "typescript.tsserver.log": "verbose",
    "javascript.suggest.autoImports": true,
    "typescript.suggest.autoImports": true,
    "liveServer.settings.donotShowInfoMsg": true,
    "material-icon-theme.activeIconPack": "nest",

    "material-icon-theme.folders.associations": {
        "infra": "app",
        "entities": "class",
        "schemas": "class",
        "typeorm": "database",
        "repositories": "mappings",
        "http": "container",
        "migrations": "tools",
        "modules": "components",
        "implementations": "core",
        "dtos": "typescript",
        "fakes": "mock",
        "websockets": "pipe",
        "protos": "pipe",
        "grpc": "pipe"
    },

    "material-icon-theme.files.associations": {
        "ormconfig.json": "database",
        "tsconfig.json": "tune",
        "*.proto": "3d"
    }
}
```


## Install HomeBrew [http://brew.sh/](http://brew.sh)

Homebrew provides a system for managing graphical and command line software for macOS, enabling you to quickly install and update the tools and libraries that you need.


```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew --version
brew doctor
```

## Install Git

```
brew install git
```

After installing git, set your github details:  

If you have only one git user:  

```
git config --global user.name "Your Name"
git config --global user.email "you@your-domain.com"
```

For multiple git users: 

create a git config at ~/.gitconfig

```
[user]
    name = John Doe
    email = john@doe.tld

[includeIf "gitdir:~/work/"]
    path = ~/work/.gitconfig
```

Create a work specific config ~/work/.gitconfig (anything inside this subfolder will use this config)

```
[user]
    email = john.doe@company.tld
```

## Generate SSH key

run on terminal:

```
ssh-keygen
```

Follow the prompting, and enter your passphrase.
Then run the following command (copies your ssh key to the clipboard):


```
cat ~/.ssh/id_rsa.pub | pbcopy
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
```  

Update NPM  

```
npm install npm@latest -g
```

Now you can install Yarn

```
npm install -g yarn
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
valet tld local
```

It will prompt for password, and then reconfigure Dnsmasq and restart php and nginx

```
valet park
```

Now every project (i.e. folder) inside `~/Dev/sites` will be served by Valet at `Http://[project-name].local`.

#### If you need a secure connection (TLS using HTTP/2) you can run:

```
valet secure [project-name]
```

#### Now to allow sharing sites on your Local Network (via HTTP)


```
code /usr/local/etc/nginx/valet/valet.conf
```

Change line 

```
listen 127.0.0.1:80 :default_server;
```

to:

```
listen 80 :default_server;
```

Now to [solve a problem](https://github.com/laravel/valet/issues/440#issuecomment-579830736), at the time of writting of this guide, we need to modify valet's `server.php`

```
 code ~/.composer/vendor/laravel/valet/server.php
```

And add 

```php
/**
 * If the HTTP_HOST is an IP address, check the start of the REQUEST_URI for a
 * valid hostname, extract and use it as the effective HTTP_HOST in place
 * of the IP. It enables the use of Valet in a local network.
 */
if (preg_match('/^([0-9]+\.){3}[0-9]+$/', $_SERVER['HTTP_HOST'])) {
    $uri = ltrim($_SERVER['REQUEST_URI'], '/');

    if (preg_match('/^[-.0-9a-zA-Z]+\.'. $valetConfig['tld'] .'/', $uri, $matches)) {
        $host = $matches[0];
        $_SERVER['HTTP_HOST'] = $host;
        $_SERVER['REQUEST_URI'] = str_replace($host, '', $uri);
    }
}
```

After this piece of code:

```php
$valetConfig = json_decode(
    file_get_contents(VALET_HOME_PATH.'/config.json'), true
);
```

Save and then restart valet

```sh
valet restart
```

Now you can access the app using your mac's local ip (in this example `192.168.0.5`)

```
192.168.0.5/[app-name].local/
```

For other settings like changing php version, sharing sites via Ngrok, visit [valet's installation guide](https://laravel.com/docs/7.x/valet#installation)


### Shopify cli

Shopify development worflow, using [Themekit](https://shopify.github.io/themekit/).

```shell
brew tap shopify/shopify
brew install themekit
```

### Mongodb

```shell
brew tap mongodb/brew
brew install mongodb-community@4.2
```

### Logitech options

Download [logitech options](https://www.logitech.com/en-us/product/options) for keyboard.

### That's it. Done. 

:facepunch:
