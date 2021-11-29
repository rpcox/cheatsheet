## find
---

### Permissions

#### find sgid files

    # place results in $HOME/sgid.txt.  Format = mode, user, group, filename
    find /usr/bin -type f -perm -2000 -fprintf ~/sgid.txt '%#m %u %g %p\n'

#### find suid files

    # place results in $HOME/suid.txt.  Format = mode, user, group, filename
    find /usr/bin -type f -perm -4000 -fprintf ~/suid.txt '%#m %u %g %p\n'

#### find suid and sgid at same time

    find /usr/bin -type f \
        \( -perm -4000 -fprintf ~/suid.txt '%#m %u %g %p\n' \) ,  \
        \( -perm -2000 -fprintf ~/sgid.txt '%#m %u %g %p\n' \)

#### find sticky bit set directories

    find /tmp -type d -perm -1000

#### find world writeable files

    find /etc -type f -perm /002
    find /etc -type f -perm /o+w
    find /etc -type f -perm /o=w

    # find them and fix them
    find . -type f -perm /o=w -exec chmod o-w {} +

### Time

#### find files accessed OR modified within the last 5 min

    find /etc -type f -amin -5 -o -mmin -5

#### find files accessed AND modified within the last 5 min

    find /etc -type f -amin -5 -mmin -5

#### find links changed in the last 10 min

    find /etc -type l -cmin -10

#### find directories modified in the last 30-60 days

    find /usr/lib -type d -mtime +50 -mtime -100

### Size and type

    c = bytes             k = kilobytes
    M = Megabytes         G = Gigabytes

#### find any files > 50 MB and < 100 MB

    find . -type f -size +50M -size -100M

#### find specific files > 50 MB and < 100 MB and delete them

    find . -type f -name "*.mp4" -size +50M -size -100M -exec rm -f {} +

#### find and remove files

    # note: "find / -name core -type f -print | xargs /bin/rm -f" will fail
    # on munged filenames
    find / -name core -type f -print0 | xargs -0 /bin/rm -f

#### find files and create hashlist

    find . -type f -exec md5sum {} + >md5.txt
