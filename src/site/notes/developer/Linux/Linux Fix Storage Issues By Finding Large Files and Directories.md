---
{"dg-publish":true,"tags":["linux","hardware","bash"],"permalink":"/developer/linux/linux-fix-storage-issues-by-finding-large-files-and-directories/","dgPassFrontmatter":true}
---

## Ask Ubuntu | Dan King
```
du -aBM 2>/dev/null | sort -nr | head -n 50 | more
```

`du` arguments:

- `-a` for "all" files and directories. ==Leave it off for just directories==
- `-BM` to output the sizes in megabyte (M) block sizes (B)
- `2>/dev/null` - exclude "permission denied" error messages (thanks @Oli)

`sort` arguments:

- `-n` for "numeric"
- `-r` for "reverse" (biggest to smallest)

`head` arguments:

- `-n 50` for the just top 50 results.
- Leave off `more` if using a smaller number

Note: Prefix with `sudo` to include directories that your account does not have permission to access.

Example showing top 10 biggest files and directories in /var (including grand total).

```
cd /var
sudo du -aBM 2>/dev/null | sort -nr | head -n 10
7555M   .
6794M   ./lib
5902M   ./lib/mysql
3987M   ./lib/mysql/my_database_dir
1825M   ./lib/mysql/my_database_dir/a_big_table.ibd
997M    ./lib/mysql/my_database_dir/another_big_table.ibd
657M    ./log
629M    ./log/apache2
587M    ./log/apache2/ssl_access.log
273M    ./cache
```

## Benjamin Rancourt
```bash
sudo du --all / | sort --numeric-sort --reverse | head --lines 15
```

Shorthand
```bash
sudo du -a / | sort -n -r | head -n 15
```

specify a directory
```bash
sudo du -a /var/log | sort -n -r | head -n 15
```

---
## Credits
- [How to Find the Largest Files and Directories on Debian - Benjamin Rancourt](https://www.benjaminrancourt.ca/largest-files-directories/#:~:text=To%20uncover%20the%20largest%20files%20and%20directories%20eating,%7C%20sort%20-n%20-r%20%7C%20head%20-n%2015)
- [disk usage - What's a command line way to find large files/directories to remove and free up space? - Ask Ubuntu](https://askubuntu.com/questions/36111/whats-a-command-line-way-to-find-large-files-directories-to-remove-and-free-up)