## redirection
---

### single line redirection
These redirection commands automatically "reset" after each line.

    1>filename                     Redirect stdout to file "filename."
    1>>filename                    Redirect and append stdout to file "filename."
    2>filename                     Redirect stderr to file "filename."
    2>>filename                    Redirect and append stderr to file "filename."
    &>filename                     Redirect both stdout and stderr to file "filename."

    M>N
      "M" is a file descriptor, which defaults to 1, if not explicitly set.
      "N" is a filename.
      File descriptor "M" is redirect to file "N."

    M>&N
      "M" is a file descriptor, which defaults to 1, if not set.
      "N" is another file descriptor.

    M>&N-
      "M" is a file descriptor, which defaults to 1, if not set.
      "N" is another file descriptor.
      "N" is closed after duplicated to "M"
      
    2>&1
       Redirects stderr to stdout.
       Error messages get sent to same place as standard output.
        >>filename 2>&1
            bad_command >>filename 2>&1
            # Appends both stdout and stderr to the file "filename" ...
        2>&1 | [command(s)]
            bad_command 2>&1 | awk '{print $5}'   # found
            # Sends stderr through a pipe.
            # |& was added to Bash 4 as an abbreviation for 2>&1 |.

    i>&j
       Redirects file descriptor i to j.
       All output of file pointed to by i gets sent to file pointed to by j.

    >&j
       Redirects, by default, file descriptor 1 (stdout) to j.
       All stdout gets sent to file pointed to by j.

### input / output redirection

    command < input-file > output-file

non-standard equivalent

    < input-file command > output-file

### closing file descriptors

    n<&-                                Close input file descriptor n
    0<&-, <&-                           Close stdin
    n>&-                                Close output file descriptor n
    1>&-, >&-                           Close stdout

### equivalence

Redirect stdout and stderr to file (preferred: &>file)

    &>file	>&file		>file 2>&1

Redirect stdout and stderr to append file (preferred: &>>file)

    &>>file	>>file 2>&1

### here doc

    $ cat >file <<EOM
    -------------------------------------
    This is line 1 of the message.
    This is line 2 of the message.
    This is line 3 of the message.
    This is line 4 of the message.
    This is the last line of the message.
    -------------------------------------
    EOM
    $ cat file
    -------------------------------------
    This is line 1 of the message.
    This is line 2 of the message.
    This is line 3 of the message.
    This is line 4 of the message.
    This is the last line of the message.
    -------------------------------------
    $

Suppress leading tabs

    $ cat >file <<-EOM
            line 1
            line 2
            line 3
            line 4
            last line
    EOM
    $ cat file
    line 1
    line 2
    line 3
    line 4
    last line
    $

Parameter substitution

    $ NAME=ric
    cat <<EOM
    > hello $NAME
    > EOM
    hello ric
    $

### redirect stdout & stderr to a file

    $ command &>file

equivalent

    $ command >file 2>&1

does not do what you might thing

    command >file 2>&1           (!= command >file 2>&1)

### discard sdtout

    $ command > /dev/null

### discard stdout & stderr

    $ command >/dev/null 2>&1

equivalent

    $ command &>/dev/null

### redirect file content to stdin

    $ command < file 

read the first line of a file

    read -r line < file

read the contents of a file

    while read -r line; do echo $line; done < file

### redirect a line of text to stdin

    $ command <<< "foo bar baz"

equivalent

    $ echo "foo bar baz" | command

### redirect stderr of all commands to a file

    $ exec 2>file
    $ command1
    $ command2
    $ ...

### read from a file on designated file descriptor

    $ exec 5< file

read from the designated file descriptor

    $ while read -r -u 5 line; do echo $line; done 

operate on the file contents

    $ grep spud <&5

close the descriptor when done

    $ exec 5>&-                 read as dupe '5' to '-'

### write to a file on designated file descriptor

    $ exec 5>file

write to the file

    $ echo "spud" >&5

close it when done

    $ exec 5>&-

### redirect multiple command output to a file

    $ (command1; command2) >file

### redirect through a fifo

In terminal #1 (shell instance #1) read from fifo named 'spud'

    $ mkfifo spud
    $ $ exec <spud
    
In terminal #2 (shell instance #2) write to fifo named 'spud'

    $ exec 3> spud
    $ echo "echo from $$" >&3
    $ echo "date" >&3

Viewed in terminal #1 (note this executes the command in other shell)

    $ echo from 52648
    from 52648
    $ date
    Tue Jan 25 05:40:49 AM MST 2022
    $

### redirect stdout & stderr of one process to stdin of another process

    $ command1 |& command2

### redirection order

    $ cat /tmp/example
    echo1
    $ echo >/tmp/example echo2
    $ cat /tmp/example
    echo2
    $ >/tmp/example echo echo3
    $ cat /tmp/example
    echo3
    $ 

### redirect stdout to command1 and stderr to command2

    $ command > >(stdout_cmd) 2> >(stderr_cmd)

### get exit codes of all commands in pipe

    $ command1 | command 2 | command3 | command4
    $ echo ${PIPESTATUS[@]}

### redirect stderr to stdout, stdout to stderr, close the temp file descriptor

    $ command 3>&1 1>&2 2>&3 3>&-

