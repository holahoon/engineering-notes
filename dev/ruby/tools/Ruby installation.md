
## Install Ruby

[rbenv](https://github.com/rbenv/rbenv) helps to easily manage Ruby versions.

### 1. Install `ruby-build`

Install `ruby-build` to make it possible to install Ruby version of choice:
```bash
$ brew install ruby-build
```

### 2. Install `rbenv`

Install `rbenv`:
```bash
$ brew install rbenv
```

Then, run the following command:
```bash
$ rbenv init
```

You should see one of two messages now:
```bash
# Load rbenv automatically by appending
# the following to ~/.bash_profile:

eval "$(rbenv init -)"
```
or
```bash
# Load rbenv automatically by appending
# the following to ~/.zshrc:

eval "$(rbenv init -)"
```
if using `zshrc` (for my case).

Just do as the message suggests by running the following commands:
`bash`:
```bash
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

`zshrc`:
```bash
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
```

### Let's install Ruby

Let's first upgrade `ruby-build`:
```bash
$ homebrew upgrade ruby-build
```

Now, let's install Ruby with desired version (v. 3.2.2 for our case):
```bash
$ rbenv install 3.2.2 --verbose
```
The `--verbose` flag shows what's going on so you can be sure it hasn't gotten stuck!

Once Ruby is installed, let's tell `rbenv` which version to use by default:
```bash
$ rbenv global 3.2.2
```

We can double check that this worked by:
```bash
$ ruby -v
# ruby 2.6.10p210 (2022-04-12 revision 67958) [universal.arm64e-darwin23]
```




