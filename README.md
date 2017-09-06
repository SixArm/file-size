# file-size:<br>show file size in bytes, using the apparent size

Syntax:

    file-size <file>

Example:

    file-size example.txt
    123


## Exit

Exit codes:

  * 0 is success
  * Anything else is an error.
  

## Strategies

This script has a goal to be cross-platform,
so this script tries these strategies in order:

  * `$FILE_SIZE`
    * Any command you want.

  * `du --apparent-size`
    * Standard on current Linux.
    
  * `gdu --apparent-size`
    * Available in current GNU coreutils.

  * `find -printf`
    * Standard on current Linux, and likely on many others.
    
  * `gfind -printf`
    * Available in GNU fileutils.
    
  * `stat --printf`
    * Not standard on macOS, OSX, or Solaris.
    
  * `stat -f%z`
    * Standard on macOS, OSX, maybe BSD, and others.
    
  * `wc -c`
    * Fast on some systems, but very slow on others.


## Environment variables

This script has a goal to be customizable at runtime,
so this script accepts envirnment variables for commands:

  * `FILE_SIZE`
     * Any command you want; it must print the size first.

  * `DU`
    * du command (disk usage)
    * Example: `DU=/usr/bin/du`

  * `GDU`
    * gdu command (GNU disk usage)
    * Example: `GDU=/usr/local/bin/gdu`
    
  * `FIND`
    * find command (find files and print sizes)
    * Example: `FIND=/usr/bin/find`

  * `GFIND`
    * gfind command (GNU find files and print sizes)
    * Example: `GFIND=/usr/local/bin/gfind`
    
  * `STAT`
    * stat command (file statistics)
    * Example: `STAT=/usr/bin/stat`
    
  * `WC`
    * wc command (word count of characters a.k.a. bytes.)
    * Example: `WC=/usr/bin/wc`

Example to provide a custom `du` command path:

    DU="/foo/du" file-size example.txt

Example to export a custom `du` command path:

    export DU="/foo/du"
    file-size example.txt

We typically set our command variables in our /etc/environment file,
and similar startup files such as our ~/.bashrc file and ~/.zshrc file,
so our command variables are available for all of our shell scripts.


## Environment variables for optimization

Using an environment variable can boost the speed of this script,
because it able to use the correct command first, rather than probing.

For example, suppose your system command `wc` is very fast.

You can use it this way:

    export FILE_SIZE="wc -c"

Now the script will prefer your system command.


## For OSX users

If your system is a typical OSX system, a way to get the GNU commands
is by using the `brew` package manager and installing GNU packages:

    brew install coreutils findutil

If you want to use the GNU commands without a `g` prefix,
instead of your default system commands, one way is this:

    brew install --default-names coreutils findutil

Another way is to add the GNU commands to your PATH, such as:

    export PATH=/usr/local/opt/coreutils/libexec/gnubin:$PATH


## Tracking

  * Program: file-size
  * Version: 3.0.0
  * Created: 2014-12-02
  * Updated: 2017-09-06
  * License: GPL
  * Contact: Joel Parker Henderson (joel@joelparkerhenderson.com)
