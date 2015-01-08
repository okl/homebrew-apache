# Homebrew Apache

# Q: WHY IS THIS FORKED HERE?
A: Because the maintainer of this project rearranges the damn filepaths of apache (configs, etc) every few months making it a moving target to write scripts to set up and handle dev environments.

# Q: How should an OKL user install apache 2.2?
A: Like this:
```
brew install https://raw.githubusercontent.com/okl/homebrew-apache/master/httpd22.rb
```
# Q: Should I mess with this formula?
A: Only if you really must in order for this to install on "Mac OS X 10.19 Muir Woods" or something. See if you can merge in changes from the upstream we forked from! Perhaps you will be able to avoid bringing in the random file moves he has done this time :)
----
# Original format of readme follows

## How do I install these formulae?
`brew install homebrew/apache/<formula>`

Or `brew tap homebrew/apache` and then `brew install <formula>`.

Or install via URL (which will not receive updates):

```
brew install https://raw.githubusercontent.com/okl/homebrew-apache/master/<formula>.rb
```

## Documentation
`brew help`, `man brew` or check [Homebrew's documentation](https://github.com/Homebrew/homebrew/tree/master/share/doc/homebrew#readme).

## Configuration
After installing `httpd22` or `httpd24`, the configuration files will be in `$(brew --prefix)/etc/apache2/2.2` and `$(brew --prefix)/etc/apache2/2.4`, respectively.

## Troubleshooting
A common problem with OS X 10.8 Mountain Lion and higher is an error about a missing *OSX10.8.xctoolchain*, *OSX10.9.xctoolchain*, or *OSX10.10.xctoolchain* directory:

```
/Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.10.xctoolchain/usr/bin/cc: No such file or directory
```

This is because the OS X tool `apr-1-config` returns a path for a compiler that does not exist, even with Xcode installed:

```
$ apr-1-config --cc
/Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc
$ apr-1-config --cpp
/Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc -E
$ which apr-1-config
/usr/bin/apr-1-config
```

The simplest solution is to go to the */Applications/Xcode.app/Contents/Developer/Toolchains/* directory and create a symlink named *OSX10.8.xctoolchain*, *OSX10.9.xctoolchain*, or *OSX10.10.xctoolchain* to *XcodeDefault.xctoolchain*. This requires you to have Xcode installed. If you only have the [Xcode Command Line tools](https://developer.apple.com/downloads/) or [OSX-GCC-Installer](https://github.com/kennethreitz/osx-gcc-installer), a simple symlink will not work.

This single-line command will set up the symlink if you have Xcode installed, and if you don't, it will create directories leading up to the toolchain and a symlink to /usr/bin that will satisfy the requirements needed for `apr-1-config` to find the compiler it needs:

```bash
sw_vers -productVersion | grep -E '^10\.([89]|10)' >/dev/null && bash -c "[ -d /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain ] && sudo -u $(ls -ld /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain | awk '{print $3}') bash -c 'ln -vs XcodeDefault.xctoolchain /Applications/Xcode.app/Contents/Developer/Toolchains/OSX$(sw_vers -productVersion).xctoolchain' || sudo bash -c 'mkdir -vp /Applications/Xcode.app/Contents/Developer/Toolchains/OSX$(sw_vers -productVersion).xctoolchain/usr && for i in bin include lib libexec share; do ln -s /usr/${i} /Applications/Xcode.app/Contents/Developer/Toolchains/OSX$(sw_vers -productVersion).xctoolchain/usr/${i}; done'"
```
