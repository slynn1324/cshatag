[![Build Status](https://travis-ci.org/rfjakob/cshatag.svg?branch=master)](https://travis-ci.org/rfjakob/cshatag)
[![Go Report Card](https://goreportcard.com/badge/github.com/rfjakob/cshatag)](https://goreportcard.com/report/github.com/rfjakob/cshatag)
[Changelog](CHANGELOG.md)

```
CSHATAG(1)                       User Manuals                       CSHATAG(1)

NAME
       cshatag - compiled shatag

SYNOPSIS
       cshatag FILE

DESCRIPTION
       cshatag is a minimal and fast re-implementation of shatag
       (  https://bitbucket.org/maugier/shatag  ,  written in python by Maxime
       Augier )
       in a compiled language.

       cshatag is a tool to detect silent data corruption. It writes the mtime
       and  the sha256 checksum of a file into the file's extended attributes.
       The filesystem needs to be mounted with user_xattr enabled for this  to
       work.   When  run  again,  it compares stored mtime and checksum. If it
       finds that the mtime is unchanged but  the  checksum  has  changed,  it
       warns  on  stderr.   In  any case, the status of the file is printed to
       stdout and the stored checksum is updated.

       File statuses that appear on stdout are:
            outdated    mtime has changed
            ok          mtime has not changed, checksum is correct
            corrupt     mtime has not changed, checksum is wrong

       cshatag aims to be format-compatible with  shatag  and  uses  the  same
       extended attributes (see the COMPATIBILITY section).

       cshatag was written in C in 2012 and has been rewritten in Go in 2019.

EXAMPLES
       Typically, cshatag will be called from find:
       # find . -xdev -type f -print0 | xargs -0 cshatag > cshatag.log
       Errors  like  corrupt  files will then be printed to stderr or grep for
       "corrupt" in cshatag.log.

       To remove the extended attributes from all files:
       # find . -xdev -type f -exec setfattr -x  user.shatag.ts  {}  \;  -exec
       setfattr -x user.shatag.sha256 {} \;

RETURN VALUE
       0 Success
       1 Wrong number of arguments
       2 File could not be opened
       3 File is not a regular file
       4 Extended attributs could not be written to file
       5 File is corrupt

COMPATIBILITY
       cshatag  writes  the  user.shatag.ts field with full integer nanosecond
       precision, while python uses a double for the whole mtime and loses the
       last few digits.

AUTHOR
       Jakob                Unterwurzacher               <jakobunt@gmail.com>,
       https://github.com/rfjakob/cshatag

COPYRIGHT
       Copyright 2012 Jakob Unterwurzacher. MIT License.

SEE ALSO
       shatag(1), sha256sum(1), getfattr(1), setfattr(1)

Linux                              MAY 2012                         CSHATAG(1)
```
