# file-size:<br>show file size in bytes, using the apparent size

Syntax:

    file-size <file>

Example:

    file-size example.txt
    256

This script has a goal to be cross-platform,
so it tries a variety of strategies, in order:

  * Try `du --apparent-size`. Standard on Linux.
  * Try `gdu --apparent-size`. Available in GNU coreutils.
  * Try `find -printf` Standard on Linux, and likely on others.
  * Try `gfind -printf`. Available in GNU fileutils.
  * Try `stat --printf ...`. Not on standard OSX or Solaris.
  * Try `stat -f%z ...`. Standard on OSX, maybe BSD, and others.
  * Try `wc -c`. Fast on some systems, but slow on others.


## Environment Variables

This script tries environment variables for command paths:

  * DU: `du` command. Example: `DU=/usr/bin/du`
  * GDU: `gdu` command. Example: `GDU=/usr/local/bin/gdu`
  * FIND: `find -printf` command. Example: `FIND=/usr/bin/find`
  * GFIND: `gfind` command: Example: `GFIND=/usr/local/bin/gfind`
  * STAT: `stat` command. Example: `STAT=//usr/bin/stat`
  * WC: `wc` command. Example: `WC=/usr/bin/wc`

Example to provide a custom `du` command path:

    DU="/foo/du --bar"  file-size example.txt

Example to export a custom `du` command path:

    export DU="/foo/du --bar"
    file-size example.txt

Using an environment variable can boost the speed of this script,
because it able to use the correct command first, rather than probing.

For example a typical BSD or OSX system has a system `du` that
doesn't have any option for `--apparent-size` or `--block-size`.

On our systems, we install GNU `du` that does have these options,
and we call the command `gdu`, and we set the environment variable:

   export DU=/usr/local/bin/gdu
   file-size example.txt

We typically set our command variables in our /etc/environment file,
and similar startup files such as our ~/.bashrc file and ~/.zshrc file,
so our command variables are available for all of our shell scripts.

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
  * Version: 2.0.0
  * Created: 2014-12-02
  * Updated: 2017-09-06
  * License: GPL
  * Contact: Joel Parker Henderson (joel@joelparkerhenderson.com)
