
Make sure to have `mysql` shell installed. If not, open up `.zshrc` file to configure our zsh.
Run `open ~/.zshrc` and type in
```
# mysql
export PATH=/usr/local/mysql/bin:$PATH
```

## Connecting to Database Servers from localhost

```bash
$ mysql --host=localhost --user=root -p
```
If you want to enter the password on the command line, you can just `--password=<password>` instead of `-p`. This show a warning because using a password on the command line interface can be insecure.

Now, it will show:
```bash
mysql>
```

To exit out of mysql terminal, simply run
```bash
mysql> exit
# or
mysql> quit
```
