# Linux Revision

- Kernel - interface between hardware and software.
  - It gets the commands from the shell and communicates that to the hardware
  - It is a computer program that forms the core of an operating system and manages critical tasks like:
    - Memory management
    - Task scheduling
    - Manage hardware
- OS: the coupling of the shell and the kernel
- Shell: the shell is like a container. The interface between users and kernel/OS. CLI is a shell.
  - It is an interface to an operating system.
  - It exposes the OS’s services to human users or other programs.
  - It take commands and gives them to the OS to perform

- `echo $0`: displays the shell
- Terminal: a program that runs a shell

## Common Commands

- `ncal/cal`: new calendar
  - `ncal march 1983`
  - `ncal 1983`
  - `ncal -j` (julian days counts from Jan 1st)
  - `ncal -M` (tells ncal to use Monday as first day of the week)
  - `ncal -3` (prints present, past and next month)
  - `ncal -A 4`/ `ncal -A4` (prints 4 months after current month)
  - `ncal -B3` (prints 3 months before the current month)
  - `ncal -B4 -A1 January 1976`
- `date`: Prints current date
  - `date --universal`
  - `date -u`
  - `date`
- `sort`: sorts the contents of a file.
  - `sort colors.txt` (will sort file in alphabetical order)
  - `sort -r colors.txt` (will sort reverse alphabetical order) or `sort --reverse`
  - `sort --unique or sort -u` (will only list unique items in the file)
- `which` - prints where the command is located
- `type` - tells which type of command
  - `type date`
  - `type echo`
- For commands that are builtin into the shell, they might not have man pages and that is where the help command comes in.
  - E.g.
    - `help cd`
    - `help pwd`
- ls - lists the contents of a folder
  - ls -l (long list of files and folders)
  - ls -a (lists all folders and files including hidden)
  - ls -la (long list of all visible and hidden folders and files)
- `shutdown`
  - `shutdown -h now`: this'll shut down the system immediately.
    - The -h stands for halting it with no time delay and then it's going to halt the system and then shut it down all the way to powered off state.
  - `shutdown -r now`: (reboots the system).
    - This will shutdown and reboot the system, instead of stopping it and powering it off
- __Changing password__
  - `passwd` (you will then be prompted to provide the old password followed by the new password you want to create)
  - `passwd <user name>` (only root can specify a user name)
    - E.g. `passwd phyllisnl`
- `whoami` / `who am i`: tells you who is logged in
- __Copying directories:__
  - `cp -R <source_folder> <destination_folder>`
    - This will copy the source folder and its subfolders
- There are two main commands to find files and directories: find and locate
  - `find <directory> -name “string”`
    - `find . -name “phyllis”`
    - `sudo find / -name “ifcfg-enp0s3”`
  - `locate <string>`
    - If `locate` command does not output any result, then as root run `updatedb`
    - Also make sure you have `mlocate` package installed
    - To check: `rpm -qa | grep mlocate`
    - To install: `yum install mlocate`
    - `locate` uses a pre-built database, which should be regularly updated, while `find` iterates over a filesystem to locate files. Thus, `locate` is much faster than `find`, but can be inaccurate if the database/cache is not updated
- __WildCards(*, ?, [])__
  - A wildcard is a character that can be used as a substitute for any of a class of characters in a search
  - `*` - represents zero or more characters
  - `?` - represents a single character
  - `[]`- represents a range of characters
  - `touch abcd{1..9}-xyz` will create folders abcd1-xyz to abcd9-xyz
  - `mkdir {a..c}{1..3}`will create 9 directories (a1, a2, a3, b1…)
  - `rm abc*-xyz`: will remove all folders starting with abc characters
  - `ls abc*` - lists all folders starting with the characters abc
  - `ls -l ?bcd*` - will list all folders/files with characters bcd in them regardless of what the first character is
  - `ls -l *[cd]*` - will list all folders with either c or d characters
  - `rm *xy*` - will remove all files/folders with x and y in them
  - Other wildcards:
  - `\` = (backward slash): escape character
  - `^` = (caret): the beginning of the line
  - `$` = (dollar sign): the end of the line

- __Hard and Soft Links (`ln`)__
  - `inode`: Pointer or number of a file on the hard disk. To check the inode:
  - `ls -ltri`
  - Soft link `ln -s`: link will be removed if the file is removed or renamed
  - Hard link `ln`: link will not be removed if the original file is removed or renamed
  - Note: You cannot create a soft/hard link within the same directory with the same name

- __File and directory permissions:__
  - There are 3 types of permissions
    - `r` - read = 4
    - `w` - write = 2
    - `x` - execute/ running a program = 1
  - Each permission (`rwx`)is controlled at three levels:
    - `u` - user/yourself
    - `g` - group . eg. people in the same project
    - `o` - other/ everyone on the system
  - Command to change permission: `chmod`
    - `chmod g-w file.txt`: This will remove the group permission to write to the file
    - `chmod a-r file.txt`: this will remove read permissions for all
    - `chmod u-w file.txt`: this will remove permission from yourself to write to the file
    - `chmod g+rwx file2.txt`: will add read write permissions for group
  - You have to have executable rights to be able to `cd` into a directory
  - You can also assign several people the permissions at once:
    - `chmod ugo+r file3.txt`
    - `chmod 444 file3.tf`
    - `-r--r--r--`
- The table below assigns numbers to permission types:

| Number    | Permission Type         | Symbol      |
|-----------|-------------------------|-------------|
| 0         | No permission           | ---         |
| 1         | Execute                 | --x         |
| 2         | Write                   | -w-         |
| 3         | Execute + Write         | -wx         |
| 4         | Read                    | r--         |
| 5         | Read + Execute          | r-x         |
| 6         | Read + Write            | rw-         |
| 7          | Read + Write + Execute  | rwx         |

- `chmod 764 file5.txt` = -rwxrw-r--
- __File Ownership__
  - There are two owners of a file or directory
    - User and group
  - Command to change file ownership:
  - `chown` and `chgrp`
    - `chown` changes the ownership of a file
    - `chgrp` change the group ownership of a file
  - Recursive (`-R`) ownership change option
    - It cascades the changes through the folder- it will change the parent directory and any subfolders under it
  - `chown root file4.txt`: Will change ownership or file4.txt to user root
  - `chgroup root file4.txt`: will change group ownership to root
  - `chown -R root dir1`: Will recursively change directory ownership of dir1 to root
  - `chgroup -R root dir2`: will recursively change group ownership of dir2 to root
  - `chown phyllis:phyllis file1`: will give user and group permissions to the user phyllis for file1
- __Access Control List (ACL)__
  - Access Control List(ACL) provides an additional, more flexible permission mechanism for file systems.
  - It allows you to give permissions for any user or group to any disc resource.
  - e.g. if a particular user is not a member or a group created by you but you still want to give some read or write access, and you have to do it without making the user a member of a group.
  - Commands to assign and remove ACL permissions are:
    - `setfacl`: set file ACL
    - `getfacl`: gives you information about the existing permissions
    - To add read, write, execute permissions for user:
    - `setfacl -m u:user:rwx /path/to/file` (-m for modify)
      - `setfacl -m u:phyllis:rwx /tmp/newfile.MD`
    - To add read write permissions for a group:
      - `setfacl -m g:group:rw /path/to/file`
        - `setfacl -m g:pod-air:rw /tmp/bigfile`
    - To allow all files or directories to inherit ACL entries from the directory it is within
      - `setfacl -Rm “entry” /path/to/dir`
    - To remove a specific entry
      - `setfacl -x u:user /path/to/file` (for a specific user)
    - To remove all entries for everyone:
    - `setfacl -b path/to/file` (for all users)
    - ** As you assign the ACL permission to a file/directory, it adds + sign at the end of the permission
    - ** setting w permission with ACL does not allow to remove a file
      - `getfacl file.md`: will allow you to get info on the existing permissions

- __Help Commands__
  - There are three types of help commands:
  - `whatis <command>`
    - `whatis chmod`
  - `<command> --help`
    - `chmod --help`
  - `man <command>`
    - `man chmod`

- __Input and Output Redirects(>, >>, <, stdin, stdout, stderr)__
  - There are three redirects in linux:
  - Standard input (`stdin`), file descriptor number: 0
    - Input is used when feeding file contents to a file
    - `mail -s “office memo” allusers@abc.com < memoletter`: Will send email with the subject line “office memo” and will send it to <allusers@abc.com>.
    - The content of the letter is written inside of the memoletter
    - `cat findpath == cat < findpath`
  - Standard output (`stdout`), file descriptor number: 1
    - By default when running a command, its output goes to the terminal.
    - The output of a command can be routed to a file using the `>` symbol
    - If using the same file for additional output or to append to the same file, then use `>>`
    - If you use one `>` in a file that already has some content, the content will be overwritten. So you have to use `>>` to append to the file
  - Standard error (`stderr`), file descriptor number 2
    - When a command is executed, we use a keyboard and that is also considered (stdin - 0)
    - That command output goes on the monitor and that output is (stdout - 1)
    - If the command produces any error on the screen then it is considered (stderr - 2)
    - We can use redirects to route errors from the screen:
      - `ls -l /root 2> errorfile`
      - `telnet localhost 2> errorfile`
  - Standard output to a File(tee)
    - “tee” command is used to store and view (both at the same time) the output of any command
    - It basically breaks the output of a program so that it can both be displayed and saved in a file.
      - It does both tasks simultaneously; copies the result into the specified files or variables and also displays the result on the terminal
    - command → tee → stdout → file
    - `echo “karen is a verb” && echo “karen is a verb” > verbs.txt` == `echo “karen is a verb” | tee verbs.txt`
    - To append to the same file use the `-a` option:
      - `echo “just like love is a verb” | tee -a verbs.txt`
    - You can also output to multiple files:
      - `echo “karen is a verb” | tee file1 file2 file3`

- __Pipes( | )__
  - A pipe is used by the shell to connect the output of one command directly to the input of another command
  - Syntax:
  - `command1 [arguments] | command2 [arguments]`
    - `ls -ltr /etc | more`
    - `ls -l /etc | tail -10`

- __File display commands__
  - `cat`
  - `more`
  - `less`
  - `head`
  - `head -n <file>` will show the first n lines of the file
  - `tail`
  - `tail -n <file>` will show the last n lines of the file
  - `tail -f <file>` the `-f` option will keep sniffing on a log and every time a new record is updated to the log, the `tail -f` will get that newest log at the bottom.
  - E.g.: `sudo tail -f /var/log/secure`
- __Filters/Text Processing Commands__
  - `cut`
  - Allows you to cut parts of lines from specified files and piped data and print the result to stdout. It can be used to cut parts of a line by delimiter, byte position, and character
  - `cut --version`
  - `cut --help`
  - `man cut`
  - `cut -cn <file>`: will cut the n characters of every line in the file and display it to stdout without changing the file.
  - Eg. `cat theJays`

  ```theJays
    James
    Jill
    Jamboree
    Jim Bob
    Jones Jerry
    Jerry Jones
    ```

  - `cut -c1 theJays`
  - stdout:

  ```stdout
  J
  J
  J
  J
  J
  J
  ```

- `cut -c1,2,4`: will pick and choose characters and parse them together as one word
  - `cut -c1,2,4 theJays`

  ```stdout
  Jae
  Jil
  Jab
  Ji 
  Joe
  Jer
  ```

- `cut -c1-3 theJays`: Will list range of characters
- `cut -c1-3 theJays`

    ```stdout
    Jam
    Jil
    Jam
    Jim
    Jon
    Jer
    ```

- `cut -c1-3,6-8 theJays`: Will list the range of characters specified
- `cut -b1-3 <filename>`: list by byte size
- `cut -d: -f 6 /etc/passwd`: list first 6th column separated by `:` will give you the sixth field in the /etc/passwd file with the delimiter(:)the delimiter can be anything like(, ? /) etc.
- `cut -d: -f 6-7 /etc/passwd`: list first 6th and 7th column separated by :
- `ls -l | cut -c2-4`: will limit the output of the command to the specified characters. For example, this command will only print user permissions of files/dir to specified characters
- ls -l

```stdout
total 4
-rw-r--r-- 1 phyllis phyllis 52 Jul 22 23:48 theJays
```

- ls -l | cut -c2-4

```stdout
ota
rw-
```

- `awk`: it is a utility/language designed for data extraction. Most of the times it is used to extract fields from a file or from an output
  - `awk -W version/ awk --version`: check version
  - `man awk`: awk manual
  - `awk ‘{print $1}’ <file>`: will list the first column/field from a file
    - `cat theJays`

    ```stdout
    James
    Jill
    Jamboree
    Jim Bob
    Jones Jerry
    Jerry Jones
    ```

    - `awk '{print $1}' theJays`

    ```stdout
    James
    Jill
    Jamboree
    Jim
    Jones
    Jerry
    ```

    - `awk '{print $2}' theJays`: will print the second column

    ```stdout
    Bob
    Jerry
    Jones
    ```

  - `ls -la | awk '{print $1,$3}'`: will list 1st and 3rd field of the ls -l output

    ```stdout
    total 
    drwxr-xr-x phyllis
    drwxr-xr-x root
    -rw------- phyllis
    -rw-r--r-- phyllis
    -rw-r--r-- phyllis
    -rw-r--r-- phyllis
    drwx------ phyllis
    -rw------- phyllis
    -rw-r--r-- phyllis
    ```

- `ls -l | awk ‘{print $NF}’`: will print the last field/column of the output
- `ls -la`

  ```stdout
  total 36
  drwxr-xr-x 3 phyllis phyllis 4096 Jul 22 23:48 .
  drwxr-xr-x 3 root    root    4096 May 13 20:05 ..
  -rw------- 1 phyllis phyllis  303 Jul 22 23:57 .bash_history
  -rw-r--r-- 1 phyllis phyllis  220 Mar 27  2022 .bash_logout
  -rw-r--r-- 1 phyllis phyllis 3526 Mar 27  2022 .bashrc
  -rw-r--r-- 1 phyllis phyllis  807 Mar 27  2022 .profile
  drwx------ 2 phyllis phyllis 4096 Jul 22 23:55 .ssh
  -rw------- 1 phyllis phyllis  756 Jul 22 23:48 .viminfo
  -rw-r--r-- 1 phyllis phyllis   52 Jul 22 23:48 theJays
  ```

- `ls -la |awk '{print $NF}'`

  ```stdout
  36
  .
  ..
  .bash_history
  .bash_logout
  .bashrc
  .profile
  .ssh
  .viminfo
  theJays
  ```

- `awk ‘/Jerry/ {print}’ file`: will search for a specific word (case sensitive)
- `awk '/Jerry/ {print}' theJays`

  ```stdout
  Jones Jerry
  Jerry Jones
  ```

- `awk -F: ‘{print $1}’ /etc/passwd`: will output only the 1st field of /etc/passwd file (delimiter :)
- `awk -F: '{print $1}' /etc/passwd`

    ```stdout
    root
    daemon
    bin
    sys
    sync
    games
    man
    lp
    mail
    news
    uucp
    proxy
    www-data
    backup
    list
    irc
    gnats
    nobody
    _apt
    …
    ```

  - `echo “Hello Tom” | awk ‘{$2=”Adam”; print $0}’`: replace words, field words
  - `echo “Hello Tom”` : will print out Hello Tom. But we can replace that 2nd field with a new word, in this case Adam, with:
  - `echo “Hello Tom” | awk ‘{$2=”Adam”; print $0}’`: Now the output will be Hello Adam
  - `cat theJays | awk '{$2="Phyllis"; print $0}'`: will replace the second field with Phyllis in the file “theJays”
  - `cat theJays`

    ```stdout
    James
    Jill
    Jamboree
    Jim Bob
    Jones Jerry
    Jerry Jones
    ```

  - Now to replace all the second names with Phyllis:
    - `cat theJays | awk '{$2="Phyllis"; print $0}'`

    ```stdout
    James Phyllis
    Jill Phyllis
    Jamboree Phyllis
    Jim Phyllis
    Jones Phyllis
    Jerry Phyllis
    ```

    - `awk ‘length($0) >n’ file`: will get lines that have more than n-byte size
      - `awk 'length($0) >10' theJays`

        ```stdout
        Jones Jerry
        Jerry Jones
        ```

    - `ls -l |awk ‘{if ($9 == “seinfeld”) print $0;}’` Will get the field matching seinfeld in /home/phyllis
    - `ls -la | awk '{if ($9 == "theJays") print $0;}'`
      - ```-rw-r--r-- 1 phyllis phyllis 52 Jul 22 23:48 theJays```
    - `ls -la | awk '{if ($NF == "theJays") print $0;}'`
      - ```-rw-r--r-- 1 phyllis phyllis 52 Jul 22 23:48 theJays```
    - `ls -l |awk ‘{print NF}’`: Number of Fields
      - ```ls -l | awk '{print NF}'```

    ```stdout
    2
    9
    ```

- __grep and egrep__: stands for global regular expression print.
- It processes text line by line and prints any line which match a specified pattern
  - `grep --version`
  - `grep --help`
  - `man grep`
- `grep keyword file`: will grab the lines that match the keyword in the named file
  - `grep Jones theJays`: will grab the lines with Jones in theJays file (case sensitive)

```stdout
Jones Jerry
Jerry Jones
```

- `grep -c keyword file`: search for a keyword and count
- `grep -c Jerry thejays`

```stdout
2
```

- `grep -i KEYword file`: search for a keyword, ignore case sensitive
  - `grep -i jerry theJays`
  - `grep -ci jerry theJays`: will count the number Jerry occurs without considering the case sensitivity of the keyword “Jerry”

```stdout
  2
```

- `grep -n keyword file`: Display the matched lines and their line numbers
- `grep -ni jerry theJays` or `grep -n Jerry the Jays`

```stdout
5:Jones Jerry
6:Jerry Jones
```

- `grep -v keyword file`: Display everything but the keyword specified
  - `grep -vi jerry theJays` or `grep -v Jerry theJays`

```stdout
James
Jill
Jamboree
Jim Bob
```

- `grep keyword file | awk ‘{print $1}’`
  - `grep -i jerry theJays | awk '{print $1}'`

```stdout
Jones
Jerry
```

- `grep -i jerry theJays | awk '{print $1}' | cut -c1-3`

```stdout
Jon
Jer
```

- `ls -l | grep -i desktop`: Search for a Keyword

- `egrep` comes into play when you want to search for two keywords
- `egrep -i “keyword|keyword2” file`: search for 2 keywords
  - `egrep -i "jim|jones" theJays`

```stdout
Jim Bob
Jones Jerry
Jerry Jones
```

- __sort: sorts in alphabetical order__
  - `sort --version`
  - `sort --help`
  - `man sort`
  - `sort file`: will sort file in alphabetical order
  - `sort theJays`

  ```stdout
  Jamboree
  James
  Jerry Jones
  Jill
  Jim Bob
  Jones Jerry
  ```

  - `sort -r file`: will sort file in reverse alphabetical order
    - `sort -r theJays`

  ```stdout
  Jones Jerry
  Jim Bob
  Jill
  Jerry Jones
  James
  Jamboree
  ```

  - `sort -kn file`: sort by field number
  - `cat theJays`

  ```stdout
  James Bill
  Jill Hoarder
  Jamboree Zimmerman
  Jim Bob
  Jones Jerry Smith
  Jerry Jones
  ```

  - `sort -k2 thejays`: will sort the file by the 2nd column/field

  ```stdout
  James Bill
  Jim Bob
  Jill Hoarder
  Jones Jerry Smith
  Jerry Jones
  Jamboree Zimmerman
  ```

  - `ls -la | sort -k9`

  ```stdout
  total 36
  drwxr-xr-x 3 phyllis phyllis 4096 Jul 23 03:18 .
  drwxr-xr-x 3 root    root    4096 May 13 20:05 ..
  -rw------- 1 phyllis phyllis  303 Jul 22 23:57 .bash_history
  -rw-r--r-- 1 phyllis phyllis  220 Mar 27  2022 .bash_logout
  -rw-r--r-- 1 phyllis phyllis 3526 Mar 27  2022 .bashrc
  -rw-r--r-- 1 phyllis phyllis  807 Mar 27  2022 .profile
  drwx------ 2 phyllis phyllis 4096 Jul 22 23:55 .ssh
  -rw------- 1 phyllis phyllis 1060 Jul 23 03:18 .viminfo
  -rw-r--r-- 1 phyllis phyllis   81 Jul 23 03:18 theJays
  ```

  - `uniq`: filters out the repeated or duplicate lines in a **sorted file**
  - `sort file | uniq`: always `sort` first before using `uniq` for the `uniq` command to work
  - `cat theJays`

  ```stdout
  James Bill
  Jill Hoarder
  Jamboree Zimmerman
  Jim Bob
  Jones Jerry Smith
  Jerry Jones
  Jim Bob
  Jill Hoarder
  ```
  
  - `sort theJays | uniq`

  ```stdout
  Jamboree Zimmerman
  James Bill
  Jerry Jones
  Jill Hoarder
  Jim Bob
  Jones Jerry Smith
  ```

  - `sort file | uniq -c`: Will sort the file first then uniq and list count
  - `sort theJays |uniq -c`

    ```stdout
    1 Jamboree Zimmerman
    1 James Bill
    1 Jerry Jones
    2 Jill Hoarder
    2 Jim Bob
    1 Jones Jerry Smith
    ```

  - `sort file |uniq -d`: only show repeated lines
  - `sort theJays |uniq -d`

  ```stdout
  Jill Hoarder
  Jim Bob
  ```

  - `sort theJays |uniq -dc`: Will show both the repeated lines and the number of times they have been repeated

    ```stdout
    2 Jill Hoarder
    2 Jim Bob
    ```

- `wc`: the command reads either stdin or a list of files and generates: newline count, word count,and byte count
- `wc --version`
- `wc --help`
- `man wc`
- `cat -n theJays`

     ```stdout
     1  James Bill
     2  Jill Hoarder
     3  Jamboree Zimmerman
     4  Jim Bob
     5  Jones Jerry Smith
     6  Jerry Jones
     7  Jim Bob
     8  Jill Hoarder
     ```

- `wc file`: check file line count, word count and byte count
  - `wc theJays`

  ```8  17 102 theJays```
- `wc -l file`: get the number of lines in a file
  - `wc -l theJays`

    ```8 theJays```

- `wc -w file`: get the number of words in a file
  - `wc -w theJays`

    ```17 theJays```
- `wc -c file`: get the number of bytes in a file
  - `wc -c theJays` or `wc --bytes theJays`

    ```102 theJays```

- `ls -l | wc -l`: will count the number of lines in the output of the command `ls -l` or number of files/dir in pwd
  - ** please note this includes the first total line
- `ls -l | grep drw | wc -l`
- `grep keyword | wc -l`: Number of Keyword lines
  - `grep -i jerry theJays | wc -l`

    ```2```

- Compare Files(`diff` and `cmp`)
- `cat theJays`

```stdout
James Bill
Jill Hoarder
Jamboree Zimmerman
Jim Bob
Jones Jerry Smith
Jerry Jones
Jim Bob
Jill Hoarder
```

- `cat theJays2`

```stdout
James Bill
Jill Hoarder
Jamboree Zimmerman
Jim Bob
Jones Jerry Smith
Jerry Jones
Jim Bob
Jill Hader
```

- `diff`: compares two files line by line
  - `diff file1 file2`
    - `diff theJays theJays2`

        ```stdout
        8c8
        < Jill Hoarder
          ---
          > Jill Hader
        ```

- `cmp`: compares byte by byte
  - `cmp file1 file2`
  - `cmp theJays theJays2`

      ```theJays theJays2 differ: byte 96, line 8```

- Compress and Uncompress(`tar`, `gzip`, `gunzip`)
  - `tar`: takes a bunch of files and puts it in one container/file
    - Does not compress as much as the gzip command
    - `tar cvf file.tar <files path>`
    - To untar the files:
      - `mkdir <newDir>`
      - `mv file.tar newDir`
      - `cd newDir`
      - `tar xvf file.tar`
  - `gzip`: compress a file(produces a .gz file)
    - `gzip file.tar`
    - `gzip -d` or `gunzip`: uncompress a file
      - `gzip -d file.tar.gz` or `gunzip file.tar.gz`
- Truncate file size(`truncate`)
  - `truncate` is used to shrink or extend the size of a file to the specified size.
  - ** Caution: this command actually manipulates the file
    - `truncate -s 10 fileName`: will cut the file down to 10 bytes
    - `truncate -s 60 fileName`: will extend the file up to 60 bytes.
    - NOTE: This will not bring back the lost characters in the file that were truncated by cutting it down to 10 bytes. It will only add `^@` characters to extend the file to 60 bytes.

- Combining and Splitting Files
  - Multiples files can be combined into one and one file can be split into multiple files
    - `cat file1 file2 file3 > file4`: Will combine file1, file2, file3 into file4
  - `split file4`
  - `split -l 300 file.txt childfile`: will split file.txt into 300 lines per file and output to childfileaa, childfileab and childfileac …

- __Linux v. Windows Commands__

| Command Description                       | Windows       | Linux         |
|-------------------------------------------|---------------|---------------|
| Listing of a directory                    | dir           | ls -l         |
| Rename a file                             | ren           | mv            |
| Copy a file                               | copy          | cp            |
| Move file                                 | move          | mv            |
| Clear screen                              | cls           | clear         |
| Delete file                               | del           | rm            |
| Compare contents of files                 | fc            | diff          |
| Search for a word/string in a file        | find          | grep          |
| Display command help                      | command /?    | man command   |
| Display your location in the file system  | chdir         | pwd           |
| Display the time                          |time           | date          |

- __System Administration__
- In `vi` or `vim` while in escape mode:
  - `dd`: deletes the whole line
  - `u`: will bring back the last line deleted
  - `x`: deletes one character at a time
  - `r`: replaces one character at a time
  - `o`: creates a new line and simultaneously gets into insert mode
  - `a`: advances to the next space
  - `/<word>`: will look for a word in the file

- `sed` command
  - Replace a string in a file with a new string
  - `sed 's/old word/new word/g' <file name>`: (only makes the change to the displayed file in stdout, but not inside the file)
    - `sed 's/Jim/Jemimah/g' theJays`
  - `s` = substitute
  - `g` = global
  - `cat theJays`

    ```stdout
    James Bill
    Jill Hoarder
    Jamboree Zimmerman
    Jim Bob
    Jones Jerry Smith
    Jerry Jones
    Jim Bob
    Jill Hoarder
    ```

  - `sed 's/Jim/Jemimah/g' theJays`

  ```stdout
  Jill Hoarder
  Jamboree Zimmerman
  Jemimah Bob
  Jones Jerry Smith
  Jerry Jones
  Jemimah Bob
  Jill Hoarder
  ```

- `sed -i 's/Smith/Jones/g' theJays`(changes the file)
  - `i` = insert
- To remove a word and not replace it with another word:
  - `sed 's/old word//g' <file name>`
    - `sed -i 's/Zimmerman/new word/g' theJays`
- Find and delete a line
  - Find and delete every line that has a keyword/word in it:
  - `sed ‘/Jim/d’ theJays`
    - `d` = delete
- Remove empty lines
  - `sed -i '/^$/d' theJays`
- Remove the first or nth lines in a file
  - `sed -i '1d' theJays`: will remove the first line in the file
  - `sed -i '3d' theJays`: will remove the third line in the file
  - `sed -i '1,2d' theJays`: will remove the first and second line in the file
- To replace tabs with spaces
  - `sed -i 's/\t/ /g' theJays`: will globally replace tabs with a space in the file
- Replace spaces with tabs:
  - `sed -i 's/ /\t/g' theJays`
- Show defined lines from a file
  - `sed -n 12,18p theJays`: will only display the lines 12-18 from the file
  - `sed 12,18d theJays`: will display everything but lines 12-18 from the file
- Display an empty line after every line:
  - `sed G theJays`

   ```stdout
    James Bill

    Jill  Hoarder

    Jamboree Zimmerman

    Jim Bob

    Jones Jerry Smith

    Jerry Jones

    Jim Bob

    Jill Hoarder

  ```

- Replace every Jill with J except on line 8:
  - `sed '8!s/Jill/J./g' theJays`: will replace Jill with J. globally except for line 8
- Substitute within the vi editor on (esc mode):
  - `:%s/Hoarder/Hayden` → Will globally substitute Hoarder with Hayden in the file

- __User Account Management:__
- `useradd` or `adduser`: create a new user
- `groupadd` or `addgroup`:create a new group
- `usermod`: modify a user account
- `userdel`: delete an existing user
- `groupdel`: delete and existing group

- Files:
- `/etc/passwd` → contains users
- `/etc/group` → contains groups and the users in those groups
- `/etc/shadow` → contains passwords

- Example:
- `useradd -g superheroes -s /bin/bash -c “user description” -m -d /home/spiderman spiderman`
- `g` = group
- `s` = shell
- `c` = define the user description
- `m` `d` = user’s home directory and the user itself

- `useradd spiderman`: will add a user called spiderman
- `id spiderman`: will verify if a user spiderman exists/is created
  - uid=1001(spiderman) gid=1002(spiderman) groups=1002(spiderman)
- `groupadd superheroes`: will create a group called superheroes
- `cat /etc/group`: will display the new group, superheroes
- `userdel -r spiderman`: will delete the user and the home directory of superman
- `groupdel nonewgroup`: will delete nonewgroup from /etc/group file
- `usermod -G superheroes spiderman`: Will add user spiderman to the group superheroes
- `useradd spiderman`
- `usermod -G superheroes spiderman`
- `grep spiderman /etc/group`
  - superheroes:x:1003:spiderman → spiderman has been added to the group superheroes
  - spiderman:x:1004: → spiderman has its own group
- `chgrp -R superheroes spiderman`: will change the group spiderman into superheroes
- `cat /etc/passwd`
  - spiderman:x:1001:1004::/home/spiderman:/bin/sh
  - Name:password:userid:groupid:description:user home:shell
- `cat /etc/group`
  - `superheroes:x:1002:spiderman`
  - Group name:password:group id:users that are part of the group
- `sudo useradd njambi`
- `sudo usermod -G superheroes njambi`
- `grep njambi /etc/group`
  - superheroes:x:1003:spiderman,njambi
  - njambi:x:1002:
- `cat /etc/shadow`
- `passwd <user> or passwd <group>` will create a password in the /etc/passwd file for the user/group

- __Enable password aging__
- In the `/etc/login.defs` file:
- The `chage` command - per user (change age)
- E.g. `chage [-m mindays] [-M maxdays] [-d lastday] [-I inactive] [-E expiredate] [-W warndays] user`
- This (chage)command does not make changes to the user account itself, that would be(`usermod`)but to the parameters surrounding the user password
- `/etc/login.defs` (system-wide changes)
- Password aging controls:

```password aging
PASS_MAX_DAYS   Maximum number of days a password may be used.
PASS_MIN_DAYS   Minimum number of days allowed between password changes.
PASS_WARN_AGE   Number of days warning given before a password expires.
PASS_MAX_DAYS   99999
PASS_MIN_DAYS   0
PASS_MIN_LEN    5
PASS_WARN_AGE   7
```

- To check the password settings of a user:
  - `grep <user> /etc/shadow`
  - `grep phyllis /etc/shadow`

- Switch users and SuperDoer access (su, sudo):
- Commands:
  - `su - <username>`: switch to another user ( will prompt you for the password)
  - `sudo` command: used when you do not have root privileges/cannot become root but need to run certain commands
  - `visudo`: edits the /etc/sudoers file, a configuration file that allows user to add or remove the rights to certain commands
- To add user to the wheel to run any sudo commands:
  - `visudo`
  - `usermod -aG wheel Phyllis`
  - `grep wheel /etc/group`

- Monitor Users:
- `who`: will tell you how many people are logged in, their user ID, terminal ID and the time they were logged in
- `last`: tells you all the details of every user that has been logged in since day 1
  - `last | more`
  - `last | awk ‘{print $1}’`: will reveal the first column of the last command
  - `last | awk ‘{print $1}’ | sort | uniq`: will reveal the first column and sort then remove duplicates
- `w`: works pretty much as the `who` command but also gives you the log in time, idle time and what processes are being used by the user
- `finger`: a user information lookup command which gives details of all the users logged in to a host
  - This command has to be installed into the system:
  - In CentOS7: `yum install finger -y`
  - In CentOS8+ `finger` command is now replaced with `pinky`
- `id`: without a username specified,it gives you information about yourself, your user ID, group ID and what part of other groups you are.
  - `id james`: will give you information about james
- Talking to Users:
  - `users`: used to show the user names of users currently logged in to the current host.
  - `wall`: allows you to send a message or the contents of a file to all the currently logged-in users.
  - `wall` broadcasts your message to everyone who is logged into the linux system
  
  ```wall
  wall
  This system will shut down in ten minutes for an upgrade. Please save your work and log off for maintenance. - Phyllis
  ```

  - To exit: `ctrl + d`
  - The message is then broadcast to all the people logged in to the host:
  - This system will shut down in ten minutes for an upgrade. Please save your work and log off for maintenance. - Phyllis
- `write`: dedicated to one user. It does not broadcast your message to everyone.

  ```write
  write James
  Hi James! Your system is getting throttled by the kubernetes application. Please kill that process - phyllis
  ```

  - To exit: `ctrl + d`
  - The writer can communicate back to the sender with `write phyllis`

- __Linux Account Authentication__
- Types of accounts:
  - Local accounts
  - Domain/Directory Accounts
- System Utility Commands
  - `date`: shows the current date.
  - `uptime`: displays how long the system has been up and how many users have been logged into the system and the load average
  - Each field in uptime:
    - 11:18:23 up 83 days, 18:29, 4 users, load average: 0.16, 0.03, 0.01
    - 11:18:23: the current time in the Linux system time
    - up 83 days, 18:29 - shows for how long your system has been running
    - 4 users - number of users currently logged into your Linux System
    - load average: 0.16, 0.03, 0.01 - the average CPU load (average number of jobs in your system’s run queue) for the 1,5 and 15 minutes.
- `hostname`: used to obtain the DNS (Domain Name System) name and set the system's hostname or NIS (Network Information System) domain name.
  - A hostname is a name given to a computer and attached to the network. Its main purpose is to uniquely identify over a network.
- `uname`:displays the information about the system
  - `uname -a`: gives more detailed info about the system
- `which`: tells the location of the command you are trying to run
  - `which pwd`  
- `cal`: calendar
  - `cal 9 1977`: will display calendar for september 1977
  - `cal 2015`: will display the calendar for the whole of 2015
- bc: binary calculator
- To exit, type `quit` or `ctrl + D`

- __Processes, Jobs and Scheduling__
- Application: service. A program that runs on your computer
- Script: commands/list of instructions written in a file and packaged to run in the background
- Process: when an application starts it generates a process
- Daemon: something that continuously run in the background until interrupted
- Threads: each process could have multiple threads associated with it
- Job: job or work order created by a scheduler to run services/applications or processes at a scheduled time
- `systemctl`: used to start your application
  - Used to control system services
  - It is available in version 7+ and it replaces the service command
  - Usage example:
    - `systemctl start | stop| status servicename.service (firewalld)`
    - `systemctl status firewalld.service`
    - `systemctl enable servicename.service`: helps the systems to restart at boot/reboot of the linux machine
    - `systemctl restart | reload servicename.service`: useful to restart the service after a change in the configuration
    - `systemctl list-units --all`
  - The output has the following columns:
    - UNIT: the systemd unit name
    - LOAD: whether the unit’s configuration has been parsed by systemd. The configuration of loaded units is kept in memory
    - ACTIVE: A summary state about whether the unit is active. This is usually a fairly basic way to tell if the unit has started successfully or not
    - SUB: this is a lower level state that indicates more detailed information about the unit. This often varies by unit type, state, and the actual method in which the unit runs
    - DESCRIPTION: A short textual description of what the unit is/does
  - To add a service under systemctl management:
    - Create a unit file in `/etc/systemd/system/servicename.service`
  - To control system with systemctl:
    - `systemctl poweroff`
    - `systemctl halt`
    - `systemctl reboot`
- `ps`: (process status)
  - It displays all the currently running processes in the Linux system
  - Usage examples:
    - `ps`: shows the processes of the current shell
    - `ps -e` : shows all running processes
    - `ps aux`: shows all running processes in BSD (Berkeley Systems Description) format
    - `ps -ef`: shows all running processes in full format listing
    - `ps -u username`: shows all processes by username
    - `ps -ef | grep PID`: you can grep for the process by the process ID (PID)
    - `ps -ef | grep firewalld`: grep for the firewalld process by the process name
  - Output:
  - PID → the unique process ID
  - TTY → terminal type that the user logged in to
  - TIME → amount of CPU in minutes and seconds that the process has been running
  - CMD → name of the command
- `top`: used to show the linux processes and it provides a real view of the running system
  - This command shows the summary information of the system and the list of processes or threads which are currently managed by the Linux Kernel
  - It puts you in interactive mode and you can exit by hitting q
  - Usage:
    - `top`
    - `top -u <username>` → shows tasks/processes by user/owned
    - `top then press c when in interactive mode` → shows absolute path of the command
    - `top then press k` → kill a process by PID within top session
    - `top then press M and P` → to sort all Linux running processes by memory usage
  - ** the top command refreshes every three seconds
  - Columns displayed:
    - PID: shows task’s unique ID
    - USER: username of task’s owner
    - PR: the scheduling priority of the process from the perspective of the kernel
    - NI: represents a Nice Value of a task. A negative nice value implies higher priority, and positive nice value means lower priority
    - VIRT: total virtual memory used by the task
    - RES: memory consumed by the process in RAM
    - SHR: Represent the amount of shared memory used by a task
    - S: This field shows the process state in the single-letter form
    - %CPU: represents the CPU usage
    - %MEM: shows the memory usage of a task
    - TIME+: CPU time, the same as TIME, but reflecting more on the granularity through hundredths of a second
  - *Zombie process*: It happens if a process has a parent process, and the parent process has been killed. In other words, a zombie process is a child process whose parent process has been killed
- `kill`: kills the process by name or ID
  - It is used to terminate processes manually
  - It sends a signal which ultimately terminates or kills a particular process or group of processes
  - Usage:
    - `kill [OPTION] [PID]`
    - OPTION: signal name or signal number/ID
    - PID: Process ID
      - `kill -l`: to get a list of all signal names or number
  - Most used signals are:
    - `kill PID`: kill a process with default signal
    - `kill -1`: restart a process
    - `kill -2`: interrupt from the keyboard just like CTRL C
    - `kill -9`: Forcefully kill the process
    - `kill -15`: kill a process gracefully
  - Other similar kill commands:
    - `killall`: will kill a process and its related/child processes
    - `pkill`: will allow you to kill a process by its process name, if you don’t have a PID.
  - Why `kill` and not `systemctl stop <process-name>`?
  - Sometimes a process is hung and can be stopped with a kill command
  - Sometimes a process child becomes a zombie process and it requires kill command to clean up all the remaining processes
  - Sometimes a process or a service does not respond to regular commands such as `systemctl` or `service`
- `crontab`: used to schedule tasks
- Usage:
- `crontab -e`: edit the crontab table
- `crontab -l`: list the crontab entries
- `crontab -r`: Remove the crontab
- crond: this is the crontab daemon/service that manages scheduling
- `systemctl status crond`: to manage the crond service

  ```crontab
  *          *           *                      *           *
  min(0-59)  hour(0-23)  day of the month(1-31) month(1-12) day of the week(0-6)
                    7 is also Sunday on some systems
  Create a crontab entry y scheduling a task:
  crontab -e (puts you in vi mode)
  * * * * * echo “this is my first crontab entry” >> crontab-entry 
  ```

- → This will schedule  “this is my first crontab entry” > crontab-entry for every minute

- `at`: like crontab; allows you to schedule jobs but only once
  - When the command is run, it enters interactive mode and you can exit by `Ctrl D`
  - Usage:
    - `at HH:MM PM or AM` → schedule a job
      - `at 4:45PM + enter`
      - `echo “this is my first at entry” >> at-entry`
      - `Ctrl D`
    - `atq` → list the at entries
    - `atrm #` → remove at entry
    - `atd` → at daemon/service that manages scheduling
    - `systemctl status atd` → to manage the atd service
    - `at 2:245AM 101623` → Schedule a job to run on Oct 16th, 2023 at 2:45am
    - `at 4PM + 4 days` → schedule a job at 4pm, four days from now
    - `at now +5 hours` → schedule a job to run 5 hours from now
    - `at 8:00AM Sun` → schedule a job to run at 8am on coming Sunday
    - `at 10:00AM next month` → Schedule a job to run 10am next month
  - Additional Cronjobs (hourly, daily, weekly, monthly)
    - By default there are 4 different types of cronjobs:
      - Hourly
      - Daily
      - Weekly
      - Monthly
    - All the above crons are set up in:
      - `/etc/cron.__`  (directory) → daily, hourly etc
    - The timing for each are set in:
      - `/etc/anacrontab` -- except hourly
    - For hourly:
      - `/etc/cron.d/0hourly`
    - You can find the directories:
      - `cd /etc`
      - `ls -l | grep cron`
  - Ps. If you have a script that runs daily and you would like to make it run monthly instead, you can move it to the monthly directory… etc

- __Process Management (bg,fg,nice)__
  - Anything that has to do with the process: start, stop, bring it to background, foreground
  - Putting a process in the background: first you run the process then:
    - `CTRL+Z`: stop the process
  - `jobs`: list what is being stopped
  - `bg`: puts the process in the background
  - Foreground (`fg`): So if you are done with all other tasks that you are doing on your terminal and you want to bring the process that you had put in the background initially and you wanted to see the process of it, then you type FG for foreground and that process will become live again on your console to run a process even after you exit.
  - `nohup`(no hangup): When you exit or close the terminal while your process is still running it will stop the process. To keep that from happening we can use `nohup` which means it doesn’t send any kind of signals to stop it.
    - `nohup <process> &`
    - `nohup sleep 75 &` or `nohup process > /dev/null 2>&1 &`
    - `nohup sleep 70 > /dev/null 2>&1 &`
  - Everytime you run `nohup`, it creates a file: `nohup.out`.
  - The second command is useful because some commands like nohup give a warning which you might not want to necessarily view so you can redirect it to /dev/null which is kind of outputting to a door that you don’t want to see output for.
  - To kill a process by name: `pkill`
  - Process priority: `nice`
    - e.g. `nice -n 5 <process>`
      - `nice -n 5 sleep 10`
    - If you want to prioritize by a different level of process, meaning it is very important you use the `nice` scale. It goes from __-20 to 19__.
    - The lower the number the more priority that task gets.

- __Process monitoring: `top`__
  - List process: ps
  - System Monitoring Commands (`df`, `dmesg`, `iostat 1`, `netstat`, `free`, `top`)
  - `top`: show the linux processes
  - `df`: disk partition information. It displays the amount of disk space available on the file system containing each file name argument.
    - Usage:
      - `man df`
      - `df`
      - `df -h`:
  - `du`: display disk usage statistics
  - `dmesg`: displays the output of the system, related warnings, error messages, failures, etc
  - `iostat`(input output statistics):ref: <https://linux.die.net/man/1/iostat>
    - The `iostat` command is used for monitoring system input/output device loading by observing the time the devices are active in relation to their average transfer rates.
    - The `iostat` command generates reports that can be used to change system configuration to better balance the input/output load between physical disks.
    - The first report generated by the `iostat` command provides statistics concerning the time since the system was booted. Each subsequent report covers the time since the previous report.
    - `iostat 1`: will run the statistics every second and to quit you can type CTRL C
  - `netstat`: gateway information. ref: <https://linux.die.net/man/8/netstat>
    - Print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships
    - `netstat -rnv`
  - `free`: displays the physical memory, swap space which is the virtual memory utilization
  - `cat /proc/cpuinfo`: cpu info
  - `cat /proc/meminfo`: memory info

- __System Log Monitoring (/var/log)__
  - Log directory: the primary log directory is `/var/log` unless specified or changed in the configuration of an application to change the log location
  - `boot`: the log that is generated when the systems boots up or reboots
  - `chronyd`: it is the newer version of NTP
  - `cron`: records all cron activity
  - `maillog`
  - `secure`: records all log in/out activity
  - `messages`: contains all hardware, software, application and processes information
  - `httpd`:
  - System maintenance commands (`shutdown`, `init`, `reboot`, `halt`)
  - ref: <https://linux.die.net/Intro-Linux/sect_04_02.html>
  - `shutdown`: used to bring down a linux system
  - `init`: init run levels are from 0-6. init has a different level of bringing the system down or rebooting.
    - The idea behind operating different services at different run levels essentially revolves around the fact that different systems can be used in different ways. Some services cannot be used until the system is in a particular state, or mode, such as being ready for more than one user or having networking available.
    - There are times in which you may want to operate the system in a lower mode. Examples are fixing disk corruption problems in run level 1 so no other users can possibly be on the system, or leaving a server in run level 3 without an X session running. In these cases, running services that depend upon a higher system mode to function does not make sense because they will not work correctly anyway. By already having each service assigned to start when its particular run level is reached, you ensure an orderly start up process, and you can quickly change the mode of the machine without worrying about which services to manually start or stop.
  - Available run levels are generally described in `/etc/inittab`, which is partially shown below:
    - 0 - halt (Do not set `initdefault` to this)
    - 1 - single user mode
    - 2 - Multiuser, without NFS (same as 3 if you do not have networking)
    - 3 - Full multiuser mode
    - 4 - Unused
    - 5 - x11
    - 6 - reboot (Do not set `initdefault` to this)
  - `reboot`: reboots the linux machine
  - `halt`: shuts down the computer and if there are any processes that are running that need some time to bring down other processes, `halt` does not consider them
- Changing the System Hostname (`hostnamectl`)
  - To change the hostname:
    - `hostnamectl set-hostname newhostname`
    - `hostnamectl set-hostname phyllis-mac`
  - For the new hostname to take effect either reboot the shell or the system with reboot or init 6
  - Version 7: Edit this in `/etc/hostname`
  - Version 6: Edit this in `/etc/sysconfig/network`
- Finding System Information (`uname`, `dmidecode`)
  - `cat /etc/redhat-release`: displays system name
  - `uname -a`: displays the operation system information plus the architecture
  - `dmidecode`: more detailed. It contains BIOS system information, serial number, etc
  - Finding the System Architecture(`arch`)
  - Differences between a 32-bit and 64-bit CPU
    - The biggest difference between a 32-bit and 64-bit processor is the number of calculations they can perform, which affects the speed at which they can complete tasks. 64-bit processors can come in dual-core, quad core, six-core and eight-core versions for home computing. Multiple cores allow for an increased number of calculations per second that can be performed, which can increase the processing power and help make a computer run faster. Software programs that require many calculations to function smoothly can operate faster and more efficiently on the multi-core 64-bit processors.
  - You can run a 32-bit job in a 64-bit processor but not the other way around.
  - Commands:
    - `arch`
    - `uname -a`
- Terminal:
  - Control Keys
    - CTRL U: erase everything typed on the command line
    - CTRL C: stop/kill a command
    - CTRL Z: suspend a command. E.g. when you are stuck in a process
    - CTRL D: exit from an interactive program (signals end of data).
- Commands(`clear`, `exit`, `script`)
  - `clear`: clears your screen
  - `exit`: exit out of a shell, user session or terminal
  - `script`: the script command stores terminal activities in a log file that can be named by a user, when a name is not provided by a user, the default file name, typescript is used
    - `script mylogfile.log` will record all activity in the file
    - To exit out of the script, type `exit`.
    - To view the contents of mylogfile.log:
      - `more mylogfile.log`
    - It records both the inputs and the outputs of every command run while under script
- Recover Root Password
  - Restart computer
  - reboot
  - Edit grub file:
    - >> `e` to edit
    - >> look for `ro` (read only). For CentOs and RedHat 9, add `rd.break` to the end of that line.
  - `CTRL x` to start
- Change password
  - `chroot /sysroot`
  - `passwd root`
  - `exit`
  - Reboot
- SOS Report
  - It collects and packages diagnostic and support data
  - Package name:
    - `sos-<version>`
  - Command:
    - `sosreport`.
  - NOTE: you will need the case number from redhat to complete the report
  - ftp the report to RedHat

- __Environmental Variables__
  - A dynamic named value that can affect the way running processes will behave on a computer.
  - They are part of the environment in which a process runs
  - They are a set of defined rules and values to build an environment
  - To view all environmental variables:
    - `printenv` or `env`
  - To view one environmental variable
    - `echo $SHELL`
    - `echo $PATH`
  - To set the environmental variables (temporary)
    - `export TEST=1`
    - `echo $TEST`
  - To set environmental variable permanently
    - `vi .bashrc`(depending on the shell. On mac: .zshrc)
    - `TEST=’123’`
    - `export TEST`
    - `source /etc/zshrc` or `source /etc/bashrc` or logout and log back in
  - To set global environmental variable permanently
    - `vi /etc/profile` or /etc/bashrc ...
    - `TEST=’123’`
    - `export TEST`
    - `source ~/.zshrc` or `source ~/.bashrc` or logout and log back in

- Special permissions with `setuid`, `setgid`, and `sticky bit`
  - All permissions on a file or directory are referred as bits

  ```permissions
  -rwx    rwx     rwx
  users   groups  others
  ```

  - The `r`, `w`, `x` are referred to as bits and they can be modified with the `chmod` command
  - There are 3 additional permissions in linux:
    - These are not actual commands to set these bits. You would still have to run the `chmod` command
    - `setuid` : bit tells Linux to run a program with effective user id of the owner instead of the executor: (eg. passwd command) → /etc/shadow

      ```setuid
      -rwsx   rwx      rwx
      users   groups   others
      ```
  
    - To assign special permissions at the user level:
      - `chmod u+s xyz.sh`
    - To remove special permissions at the user or group level:
      - `chmod u-s xyz.sh`
    - `setgid` : bit tells Linux to run a program with effective group id of the owner instead of the executor: (eg. locate or wall command) → /etc/shadow
    - *note*: This bit is present for executable files only

      ```setgid
      -rwx    rwsx    rwx
      users   groups  others
      ```

    - To assign special permissions at the group level:
      - `chmod g+s xyz.sh`
    - To remove special permissions at the user or group level:
      - `chmod g-s xyz.sh`
    - To find all executables in Linux with `setuid` or `setgid` permissions
      - `find / -perm /6000 -type f`
  - *sticky bit*: a bit set on files/directories that allows only the owner or root to delete those files
  `- It is assigned to the last bit of permissions

      ```sticky bit
      -rwx    rwx     rwt
      users   groups  others
      ```

    - E.g:
      - `cd /`
      - `ls -l /tmp`
      ```drwxrwxrwt```
    - Any user can write to this file but only root/owner can delete the file to prevent it from accidental deletion from the system
    - To set the sticky bit:
      - `chmod +t <file>`
    - These bits work on C programming executables not on bash shell scripts

- __Shell Scripting__
  - To assign special permissions at the user level:
  - To find your shell: `echo $0`
  - To view available shells: `cat /etc/shells`
  - Your shell: `/etc/passwd`
  - Shebang: `#!/bin/bash`
  - Shell scripts should have executable permissions and have to be called from an absolute path
    - If on the same path ./script.sh
Basic scripts:

```bash
#!/bin/bash
a=Phyllis
b=Africa
c='linux class'
echo "My name is $a, from $b, and I am delighted to be in this $c"
My name is Phyllis, from Africa, and I am delighted to be in this linux class
```

- Input/output
- Create script to take input from the user
- `read` will wait for an input from the user
- `echo` will output to the screen

```bash
#!/bin/bash
echo Hello, my name is AI Helper. What is your name?
read name
echo Hello $name! Nice to meet you!
Hello, my name is AI Helper. What is your name?
# Phyllis  # → (user input)
Hello phyllis! Nice to meet you!
```

- If you want it to read a command inside an echo command:

```bash
#!/bin/bash
server_name=`hostname`
echo My name is $server_name
```

output: → My name is PhyllisVM

- `if-then` scripts:

```bash
#!/bin/bash
count=98
if [ $count -eq 100 ]
then
echo the count is 100
else
echo "The count is not quite 100; the current count is $count"
fi


count=100
if [ $count -eq 100 ]
then
echo count is 100
else
echo count is not 100
fi
The count is not quite 100; the current count is 98
count is 100
```

- Check if a file exists:

```bash
#!/bin/bash
if [ -e /Users/naisenya/code/Linux-practice ]
then
echo "file exists"
else
echo "file does not exist"
fi
```

- File operations:
  - `-s`: file exists and is not empty
  - `-f`: file exists and is not a directory
  - `-d`: directory exists
  - `-x`: file is executable
  - `-w`: file is writable
  - `-r`: file is readable

- `if test -s error.txt`: Test if the error.txt file exist and its size is greater than zero
- `if [ $? -eq 0 ]`: Checks if input is equal to zero (0)
- `if [ -e /export/home/filename ]`: Will check if the file exists
- `if [ "$a" != "" ]`: Checks if the variable does not match
- `if [ error_code != "0" ]`: Checks if the file is not equal to zero

- AND operator: `&&`
- OR logical operator: `||`
  - Check if the output is either Monday or Tuesday
    - `if [ “$a” = Monday ] || [ “$a” = Tuesday ]`

- Comparisons
  - `-eq`: equal to for numbers
  - `==`: equal to for letters
  - `-ne`: not equal to
  - `!==`: not equal to for letters -lt less than
  - `-le`: less than or equal to -gt greater than
  - `-ge`: greater than or equal to

- For Loop scripts
  - For loop scripts keep running until a specified variable is met
- E.g. variable = 10: run the script 10 times

```bash
#!/bin/bash
for i in {1..5} # 1 2 3 4 5
do
echo Welcome $i times
done
```

  ```output
  output:
  Welcome 1 times
  Welcome 2 times
  Welcome 3 times
  Welcome 4 times
  Welcome 5 times
  ```

- Simple for loop output

```bash
#!/bin/bash
for i in eat run jump play
do
echo See Phyll $i
done
```

```output
output:
See Phyll eat
See Phyll run
See Phyll jump
See Phyll play
```

- for loop to create 5 files named 1-5

```bash
for i in {1..5}
do
touch $i
done
```

- for loop to delete 5 files named 1-5

```bash
for i in {1..5}
do
rm $i
done
```

- Specify days in a for loop

```bash
i=1
for day in Mon Tue Wed Thu Fri
do
echo "Weekday $((i++)) : $day"
done
```

```output
output
Weekday 1 : Mon
Weekday 2 : Tue
Weekday 3 : Wed
Weekday 4 : Thu
Weekday 5 : Fri
```

- List all users one by one from /etc/passwd file

```bash
i=1
for username in `awk -F: '{print $1}' /etc/passwd`
do
echo "Username $((i++)) : $username" >> users.txt
done
```

- Do While scripts:
  - The while statement continually executes a block of statements while a particular condition is true or met
  - Most daemon programs run while scripts

```bash
  while [ condition ]
    do
      command1
      command2
      command-n
    done
  ```

```bash
#!/bin/bash
c=10
while [ $c -le 20 ]
do
echo "Welcome $c times"
(( c++ ))
done
```

- Script to run for a number of seconds

```bash
count=0
num=10
while [ $count -lt 10 ]
do
echo
echo $num seconds left to stop this process $1
sleep 1
num=`expr $num - 1`
count=`expr $count + 1`
done
echo $1 process is stopped!
```

- Case Statement Scripts

```bash
#!/bin/bash
echo please choose one of the options below:
echo 'a = Display Date and Time'
echo 'b = List files and directories'
echo 'c = List users logged in'
echo 'd = Check system uptime'

read choices
case $choices in
a) date;;
b) ls;;
c) who;;
d) uptime;;
*) echo Invalid choice - bye!
esac
```

- Check Other Server's Connectivity
  - Script to check the status of remote hosts

```bash
#!/bin/bash
# -t (timeout)
# -c count: the number of ping times
ping -c1 -t2 8.8.8.8
if [ $? -eq 0 ]
then
echo OK
else
echo NOT OK
fi
```

- direct output to a file:

```bash
ping -c1 -t2 192.168.1.1 &> /dev/null
if [ $? -eq 0 ]
then
echo OK
else
echo NOT OK
fi
```

- Define a variable:

```bash
host="8.8.8.8"
ping -c1 -t2 $host &> /dev/null
if [ $? -eq 0 ]
then
echo $host is reachable
else
echo $host NOT reachable
fi
```

- Aliases (`alias`)
  - `alias` is a popular command used to cut down on length and repetitive commands
    - `alias ls="ls -al"`
    - `alias pl="pwd; ls"`
    - `alias tell="whoami; hostname; pwd"`
    - `alias dir="ls -l | grep ^d"`
    - `alias lmar="ls -l | grep Mar"`
    - `alias wpa="chmod a+w"`
    - `alias d="df -h | awk '{print \$6}' | cut -c1-4"`
  - To find out the existing aliases: `alias`
  - To remove an alias using unalias: `unalias ls`
  - Creating User or Global Aliases
    - User: Applies only to a specific profile
      - `/home/user/.bashrc`  or `~.zshrc`
    - Global: Applies to everyone who has an account on the system
      - `/etc/bashrc` or `/etc/zshrc`
  - Go to the end of the file by typing `shift g`
    - Remember to either logout of the system or source the shell when done modifying it for the aliases to take effect
    - `source ~.zshrc` or `source /etc/bashrc`
- History
  - history: To view previously executed commands:
    - history
    - history | more
    - history | more

  ```history
  829  ping help
  830  help ping
  831  man ping
  832  ./ping.sh
  833  ./ping.sh
  834  ./ping.sh
  835  ./ping.sh
  836  $?
  837  $1
  838  man ping
  839  ./ping.sh
  840  ./ping.sh
  841  ./ping.sh
  842  ./ping.sh
  843  alias
  844  history | more
  ```

  - To grep for a specific history, e.g awk:
    - `history | grep “awk”`
  - How to run a previous command from history listing:
    - `!nn` where nn is the number of the command
      - `!838`  from the history above will give you the manual pages for the ping command
