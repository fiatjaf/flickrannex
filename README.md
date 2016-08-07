flickrannex
=========

Hook program for gitannex to use flickr as backend

# Requirements:

    python2

Credit for the png tEXt patch goes to: https://code.google.com/p/pypng/issues/detail?id=65

# Install

Clone the git repository in your home folder.

    git clone git://github.com/fiatjaf/flickrannex.git 

This should make a `flickrannex` folder in the current dir.  Move into it and
use `easy_install` to install it, e.g. to install it system-wide use

    easy_install .

# Setup

Run the program once to set it up. 

    flickrannex

After the setup has finished, it will print the git-annex configure lines.

# Notes

## Unencrypted mode
The photo name on flickr is currently the GPGHMACSHA1 version.

To setup git-annex preferred content run the following command in your annex directory:

```
   git annex content flickr uuid include=*.jpg or include=*.jpeg or include=*.gif or include=*.png
```

## Encrypted mode
The current version base64 encodes all the data, which results in ~35% larger filesize.

I might look into yyenc instead. I'm not sure if it will work in the tEXt field.

To setup git-annex preferred content run the following command in your annex directory:

```
   git annex content flickr exclude=largerthan=30mb
```

## Including directories as tags
Get get each of the directories below the top level git directory added as tags to uploads:

    git config annex.flickr-hook 'GIT_TOP_LEVEL=`git rev-parse --show-toplevel` /usr/bin/python2 %s/flickrannex.py'

In this case the image:

```
   /home/me/annex-photos/holidays/2013/Greenland/img001.jpg
```

would get the following tags:  `"holidays"` `"2013"` `"Greenland"`.  (assuming "/home/me/annex-photos" is the top level in the annex)

Caveat Emptor - Tags will *always* be NULL for indirect repos - we don't (easily) know the human-readable file name.

## Debugging

Make git-annex pass a `dbglevel` flag to flickrannex, like this:

```
git config annex.flickr-hook "/path/to/the/executable/flickrannex --dbglevel 2"
```

Logs will be output to stderr and you'll be able to see them from git-annex commands.
