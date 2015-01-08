# 1. Stop all apache
```
sudo apachectl stop ; apachectl stop
```

# 2. Remove all apache var/log / var/run (may also be in var/apache2)
```
find /usr/local/var -name apache2 -exec echo rm -i {} \;
```

# 3. Remove all apache and php configs (in etc)
```
tar -cjf ~/Desktop/old_apache_configs.tbz -C /usr/local etc/php etc/apache2 && rm -rf /usr/local/etc/apache2 /usr/local/etc/php
```

# Remove old apache webroot
```
find /usr/local/var -name www -depth -4 -exec echo 'Consider removing:  {}' \;
```

# Brew update
```
brew update
```

# Remove old apache builds

```
brew uninstall php53-apc php53-memcached php53-mcrypt php53 httpd22
```


# Install apache from the OKL fork:
```
brew tap josegonzalez/homebrew-php
brew install zlib
brew install homebrew/apache/httpd22 --enable-so --disable-unique-id
brew install https://raw.githubusercontent.com/okl/homebrew-apache/master/httpd22.rb --enable-so --disable-unique-id
```

# Install PHP & Friends
```
brew install php53 --with-mysql --homebrew-apxs
brew install php53-memcached --homebrew-apxs
brew install php53-apc --homebrew-apxs
brew install mhash
brew install php53-mcrypt --homebrew-apxs
```

# Do this
```
touch "$HOME/Desktop/quit_after_step_10.txt"
```

Now run the Setup Script!

# start things
```
APACHECTL=$(brew list httpd22 | grep 'bin/apachectl$')
$APACHECTL stop 2>/dev/null
$APACHECTL start

```

# Configure things
Switch to the branch "dev_standard_apache"
```
c ewok; git checkout dev_standard_apache
restartnginx
```

# test things

2. hit localhost.newokl.com:8884
3. Try to add to cart
4. Try to hit a PHP url on the site such as /e-gift-card


# Now upgrade your RVM and preinstall the latest Rubies!
```
rvm get stable

rvm install ruby-1.9.3-p551
rvm install ruby-2.2.0
```
