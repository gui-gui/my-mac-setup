# WEB Development setup for new mac with El Capitan

This is my setup when i need to setup a new mac for webdev. 
It includes the basic to get apache to work with multiple virtual hosts , multiple php versions with homebrew, git, mysql (mariadb). Use at your own risk.

This is compiled from pieces of some great tutorials i found online. This is only a personal guideline.

Credits
* [Mallinson osx-web-development](https://mallinson.ca/osx-web-development/)
* [Get Grav Apache with multiple php versions](https://getgrav.org/blog/mac-os-x-apache-setup-multiple-php-versions)
* [Set Up OS X For Web Development in 10 Minutes](https://www.youtube.com/watch?v=bjgZ93oEZF0)
* [How to run multiple PHP versions simultaneously](https://medium.com/@wvervuurt/how-to-run-multiple-php-versions-simultaneously-under-os-x-el-capitan-using-standard-apache-98351f4cec67#.rfalrc99o)


##- Apps##

  [1password](https://1password.com/downloads/), 
  [Sketch](https://www.sketchapp.com/),
  [Atom](https://atom.io/),
  [Chrome](https://www.google.com/chrome/browser/desktop),
  [Firefox](https://www.mozilla.org/en-US/firefox/new/),
  [iTerm2](https://www.iterm2.com),
  [CyberDuck](https://cyberduck.io/),
  [SequelPro](http://www.sequelpro.com),
  [SourceTree](https://www.sourcetreeapp.com/download/),
  [Clipmenu](http://www.clipmenu.com/),
  [Codekit](https://incident57.com/codekit/),
  [The Unarchiver](http://unarchiver.c3.cx/unarchiver),
  [Slack](https://slack.com/downloads),
  [Whatsapp](https://www.whatsapp.com/download/),
  [Skype](https://www.skype.com/en/)

-

###> Atom Setup
Atom menu > intall shell commands
  
-

###> Oh-my-zsh [http://ohmyz.sh/](http://ohmyz.sh/) 

Abrir iterm2

```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

open `~/.zshrc`

add these aliases at the end of the file

```
alias zzz="atom ~/.zshrc"
alias ohmy="atom ~/.oh-my-zsh"
alias rundb="mysql.server start"
```

Install [Honukai Theme](https://github.com/oskarkrawczyk/honukai-iterm-zsh)

-

###> Dock spacing

Add an empty separator to the dock. 
Enter the code below in the terminal. One for each separator.

```
defaults write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}'
```

Reset the dock

```
killall Dock
```


###> iTerm2 or/and Terminal setup 

####-> Install command line tools
-

```
xcode-select --install
```

####-> Install HomeBrew [http://brew.sh/](http://bew.sh)
-

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew --version
brew doctor
```

####-> Install Git
-

```
brew install git
which git
```
which git will give the current git path, probably `/usr/bin/git`

change the current oneto git-apple so that brew version is used

```
sudo mv /usr/bin/git /usr/bin/git-apple
```

check git version again

```
which git
```

the response should be `/usr/local/bin/git`

or check 

```
git --version
```

####-> Generate SSH key
-

Follow this link and generate it

[Github - Generate SSH] (https://help.github.com/categories/ssh/)

To copy the generated ssh key to the clipboard

```
pbcopy < ~/.ssh/id_rsa.pub
```


####-> Instalando dnsmasq 
-

This is a great little tool to that allows us to use wildcard subdomain names.

With the default apache settings, you can add as many sites as you like in subfolders of the web root. You can have sites like this:

* http://home.dev/client1
* http://home.dev/client2
* http://home.dev/client3


However, that creates a problem. When you have each site in a folder, it’s more difficult to manage the settings for each site. Each one must then have a different absolute root. The solution is to create a subdomain for each site, and use URLs like these:

* http://client1.dev
* http://client2.dev
* http://client3.dev


We can accomplish this by placing all three sites in our /private/etc/hosts file, but then we need to keep adding entries every time we add a new site. dnsmasq allows us to do this by interrupting each request that ends with .dev and forwarding it to a designated IP address (127.0.0.1 in our case).

To install dnsmasq, we use the previously installed Homebrew. The following commands will install dnsmasq, configure it to point all requests to the .dev top-level domain to our local machine, and make sure it starts up and runs all of the time.

```
brew install dnsmasq
cd $(brew --prefix)
mkdir etc
echo 'address=/.dev/127.0.0.1' > etc/dnsmasq.conf
sudo cp -v $(brew --prefix dnsmasq)/homebrew.mxcl.dnsmasq.plist /Library/LaunchDaemons
sudo launchctl load -w /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
sudo mkdir /etc/resolver
sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/dev'
```

We’re now done with dnsmasq, and if all goes well, you’ll never need to think about it again. Now, to get Apache going.

-
###> Apache httpd.conf
-

Start Apache Forever :)

```
sudo apachectl start
```

That’s it. Test it out by visiting [http://localhost](http://localhost) in your browser. You should see a simple page that says “It Works”. Apache is up and running, and is ready to serve your sites. It will stay on until you turn it off (I never turn it off), and will start up automatically each time you start your computer. Don’t worry about taxing your computer’s resources by running Apache. It won’t be a problem.

You should also try [http://home.dev](http://home.dev), which should work since dnsmasq is pointing all *.dev domains to the local IP. You can try [http://ANYTHING.dev](http://ANYTHING.dev)
as well.


####-> apache + php
-

Apache will serve up sites as is, but there are a few quick changes we need to make to the configuration files before we are ready to go. Using your favorite text editor, open up `/private/etc/apache2/httpd.conf`

If you’re going to be using PHP, you need to tell Apache to use the PHP module to handle `.php` files. On line 117 (line 169 in Yosemite/El Capitan but could be different on your system), you need to uncomment this line (remove the “#”)

```
169  #LoadModule php5_module libexec/apache2/libphp5.so
```

becomes

```
169  LoadModule php5_module libexec/apache2/libphp5.so
```

Starting with OS X 10.10 (Yosemite), Apple moved from Apache 2.2 to Apache 2.4, and that means there are a few additional changes we need to make. First, there is a directive that helps secure your machine by denying access to the entire file system by default. I’ll show you how to remove this directive, since I find that easier on a machine meant for development. The section of code runs from line 220 through 223. You can comment out (place ‘#’ in front of each line) or just remove this section.

```
220 <Directory />
221     AllowOverride none
222     Require all denied
223 </Directory>
```

Then, the ```mod_vhost_alias``` module needs to be activated. We must uncomment line 160, so:

```
159  #LoadModule dav_lock_module libexec/apache2/mod_dav_lock.so
160  #LoadModule vhost_alias_module libexec/apache2/mod_vhost_alias.so
161  LoadModule negotiation_module libexec/apache2/mod_negotiation.so
```

becomes

```
159  #LoadModule dav_lock_module libexec/apache2/mod_dav_lock.so
160  LoadModule vhost_alias_module libexec/apache2/mod_vhost_alias.so
161  LoadModule negotiation_module libexec/apache2/mod_negotiation.so
```

####-> Creating document root folder - www###
-

Create a folder named www anywhere in the system. 
```/Users/YOUR-USER/www ```
is a good place

Now Search for the term DocumentRoot in the ```httpd.conf``` file, and you should see the following line:

```
DocumentRoot "/Library/WebServer/Documents"
```

Change this to point to your document root path:

```
DocumentRoot "/Users/your_user/www"
```

You also need to change the <Directory> tag reference right below the DocumentRoot line. This should also be changed to point to your new document root also:

```
<Directory "/Users/your_user/www">
```

In that same <Directory> block you will find an AllowOverride setting, this should be changed as follows:

```
AllowOverride All
```


####-> User & Group###
-

Now we have the Apache configuration pointing to a Sites folder in our home directory. One problem still exists, however. By default, apache runs as the user ```_www``` and group ```_www```. This will cause permission problems when trying to access files in our home directory. About a third of the way down the ```httpd.conf``` file there are two settings to set the User and Group Apache will run under. Change these to match your user account (replace your_user with your real username), with a group of staff:

```
User your_user
Group staff
```

####-> PHP 7.0 Additional Changes####
-

Although this is not needed for PHP 5.X, it is needed for PHP 7.0 and as we cover that below it's safer to just add it now. So scroll down until you see something like:

```
<FilesMatch "^\.([Hh][Tt]|[Dd][Ss]_[Ss])">
    Require all denied
</FilesMatch>
```

Below this paste the following:

```
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
```

Also you must set the Directory Indexes for PHP explicitly, so search for this block:

```
<IfModule dir_module>
    DirectoryIndex index.html
</IfModule>
```

and replace it with this:

```
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>
```

####-> Multiple hosts####
-

Overwriting the document root folder is easy, and makes it possible to use wildcards to point to specific sites inside your documment root folder. On line 500 in Yosemite/El Capitan, in order to allow us to add multiple websites to Apache:

```
499  # Virtual hosts
500  #Include /private/etc/apache2/extra/httpd-vhosts.conf
```

becomes

```
499  # Virtual hosts
500  Include /private/etc/apache2/extra/httpd-vhosts.conf
```

Save the file.


This last change tells apache to load the information in the httpd-vhosts.conf file in the /private/etc/apache2/extra/ directory. We need to edit that file. You can delete or comment out everything in that file, and replace it with the following:


```

  <Directory "<PATH_TO_WWW_FOLDER>">
    Options Indexes MultiViews FollowSymLinks
    AllowOverride All
    Order allow,deny
    Allow from all
  </Directory>

  <Virtualhost *:80>
    VirtualDocumentRoot "<PATH_TO_WWW_FOLDER>/home/wwwroot"
    ServerName home.dev
    UseCanonicalName Off
  </Virtualhost>

  <Virtualhost *:80>
    VirtualDocumentRoot "<PATH_TO_WWW_FOLDER>/sites/%1/wwwroot"
    ServerName sites.dev
    ServerAlias *.dev
    UseCanonicalName Off
  </Virtualhost>

  <Virtualhost *:80>
    VirtualDocumentRoot "<PATH_TO_WWW_FOLDER>/sites/%-7+/wwwroot"
    ServerName xip
    ServerAlias *.xip.io
    UseCanonicalName Off
  </Virtualhost>
```

Then run the following to force Apache to load your new config files:

```
sudo apachectl restart
```

##- How to create a new project ##

Inside the `www` folder create two folders: `sites` and `home`.

Inside `home` add a `wwwroot` folder. The `wwwroot` folder is the root of your [http://home.dev](http://home.dev) website

Inside the `sites` folder you will create one folder for each project you make, and inside each you will add a `wwwroot` folder which will be the root of that project. accessible via `<project-name>.sites.dev`

i.e.
`Users/YOURUSER/www/sites/blog/wwwroot/index.php` would be accessible via [http://blog.sites.dev](http://blog.sites.dev)



##- Multiple php versions with sphp ##

With HomeBrew installed it is a simple procedure to install PHP. We will proceed by installing both PHP 5.4, PHP 5.5, PHP 5.6, and PHP 7.0 and using a simple script to switch between them as we need. First though we have to tap into the PHP formulas and run the following commands.


```
brew tap homebrew/dupes
brew tap homebrew/versions
brew tap homebrew/homebrew-php

brew install php54 --with-fpm
brew install php54-mcrypt
brew install php54-xdebug
brew unlink php54

brew install php55 --with-fpm
brew install php55-mcrypt
brew install php55-xdebug
brew unlink php56

brew install php56 --with-fpm
brew install php56-mcrypt
brew install php56-xdebug
brew unlink php56

brew install php70 --with-fpm
brew install php70-mcrypt
brew install php70-xdebug
brew unlink php70
```

Now we need to make apache use these php versions istead of the default one

change both lines

```
168 # LoadModule rewrite_module libexec/apache2/mod_rewrite.so
169	LoadModule php5_module libexec/apache2/libphp5.so
```

becomes

```
168	LoadModule rewrite_module libexec/apache2/mod_rewrite.so
169	LoadModule php5_module /usr/local/opt/php54/libexec/apache2/libphp5.so
```

save and restart apache

```
sudo apachectl restart
```

check the php version with phpinfo();

###> PHP Switcher Script ##
-

We hard-coded Apache to use PHP 5.4, but we really want to be able to switch between versions. Luckily, some industrious individual has already done the hard work for us and written a very handy little PHP switcher script.

It is useful to have a your own local bin folder in your home directory. We will create one and setup the sphp script:

```
mkdir -p ~/bin/
curl -L https://raw.githubusercontent.com/conradkleinespel/sphp-osx/master/sphp > ~/bin/sphp
chmod +x ~/bin/sphp
```


###> Updating the Path
-

Now we want to ensure this script is easily found when we are in the terminal, so we need add our `/Users/your_user/bin` directory to our path, as well as putting HomeBrew's preferred `/usr/local/bin` and `/usr/local/sbin` before OS X's default `/usr/bin` folder. Depending on your shell your using, you may need to add this line to `~/.profile`, `~/.bash_profile`, or `~/.zshrc`.

We will assume you are using the default bash shell, so add this line to a your .profile (create it if it doesn't exist) file at the root of your user directory:

```
export PATH=/usr/local/bin:/usr/local/sbin:$PATH:/Users/your_user/bin
```

CLOSE THE TERMINAL AND OPEN IT AGAIN TO RELOAD THE PATH


####-> Brew PHP LoadModule for `sphp` switcher
-

Again, on `httpd.conf`


```
LoadModule php5_module /usr/local/opt/php54/libexec/apache2/libphp5.so
```

becomes

```
# Brew PHP LoadModule for `sphp` switcher
LoadModule php5_module /usr/local/lib/libphp5.so
#LoadModule php7_module /usr/local/lib/libphp7.so
```

##- Install MariaDB with Brew:

```
brew install mariadb
unset TMPDIR
mysql_install_db
```

After a successful installation, you can start the server:

```
mysql.server start
```

You should get some positive feedback on that action:

```
Starting MySQL
. SUCCESS!
```

