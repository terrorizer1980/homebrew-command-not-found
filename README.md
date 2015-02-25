# homebrew-command-not-found

This project tries to reproduce Ubuntu’s `command-not-found` for Homebrew users
on OSX.

On Ubuntu, when you try to use a command that doesn’t exist locally but is
available through a package, Bash will suggest you a command to install it.
Using this script, you can replicate this feature on OSX:

```
# on Ubuntu
$ when
The program 'when' is currently not installed.  You can install it by typing:
sudo apt-get install when

# on OSX with Homebrew
$ when
The program 'when' is currently not installed. You can install it by typing:
  brew install when
```

## Install

First, tap this repository:

    brew tap bfontaine/command-not-found

Then source the `handler.sh` script in your `.bashrc`:

    . $(brew --prefix)/Library/Taps/bfontaine/homebrew-command-not-found/handler.sh


### Upgrade from 0.1.1

Before the 0.2.0 version this was a simple `handler.sh` script which `grep`-ed
in formulae to find the binaries. It worked well for most formulae but wasn’t
exhaustive.

The 0.2.0 version changed how it works. If you’d like to keep your existing
handler script, it’s fine, it’ll continue to work. If instead you’d like to
upgrade to 0.2.0 just remove it and follow the install instructions above.

### Requirements

This tool only supports Bash for now.

## How does it work?

When you tap the repo you’ll get two more `brew` commands: `brew which-formula`
and `brew which-update`. The first one uses a file database to gives you which
formula you have to install in order to get a specific command. The file is
generated by the second command by crawling all installed formulae and collect
their binaries. If we regularly run this update script on multiple Homebrew
installations I hope we’ll cover most of the formulae. Having this as a tap
means you get an up-to-date binaries database each time you run `brew update`.

The `handler.sh` script defines a `command_not_found_handle` function which is
used by Bash when you try a command that doesn’t exist. The function calls
`brew which-formula` on your command, and if it finds a match it’ll print it to
you. If not, you’ll get an error as expected.

Over [2000 formulae][progress] have been imported in the database, representing
more than 8100 different commands (67% of the main Homebrew repo).

[progress]: https://github.com/bfontaine/homebrew-command-not-found/wiki/Progress
