Run windows applications under Posix envinonment
================================================

Cygwin and Msys2 use posix paths and environments which makes most of
the standard windows programs to fail because of path mismatch.
The traditional way of handling that is using Cygwin cygpath
utility which translates Cygwin (posix) paths to their windows
equivalents from shell.

For example a standard usage would be:

    program.exe "--f1=`cygpath -w /tmp/f1`" "`cygpath -w /usr/f1`" ...

This can become very complex and it requires that the shell
script is aware it runs inside the Cygwin environment.

posix2wx utility does that automatically by replacing each posix
argument that contains path element with its windows equivalent.
It also replaces paths in the environment variable values making
sure the multiple path elements are correctly separated using
windows path separator `;`.

Using posix2wx the upper example would become:

    posix2wx program.exe --f1=/tmp/f1 /usr/f1 ...

Before starting `program.exe` posix2wx converts all command line
and environment variables to windows format.

### Usage

Here is what the usage screan displays

    Usage posix2wx [OPTIONS]... PROGRAM [ARGUMENTS]...
    Execute PROGRAM [ARGUMENTS]...
    Options are:

        -D, -[-]debug      print replaced arguments and environment
                           instead executing PROGRAM
        -V, -[-]version    print version information and exit.
        -?, -[-]help       print this screen and exit.
        -C, -[-]clean      use CLEAN_PATH environment variable instead PATH
            -[-]env=LIST   pass only environment variables listed inside LIST
                           variables must be separated space character.
            -[-]cwd=DIR    change working directory to DIR before calling PROGRAM
            -[-]root=DIR   use DIR as posix root

Note that long command options are case insensitive and have one or two dashes
which means that
    -debug
    -Debug
    --DEBUG
are all valid oprions which cause processing and displaying processed
command line and arguments, without executing the PROGRAM itself


