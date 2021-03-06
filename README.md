wu
==

Simple "where-used" searcher

Wu simplifies searching a set of files for a certain expression.  When working 
on a large software project, I often find that I want to know which files 
contain a certain phrase.  While a combination of grep and find can do the 
job, I always get tripped up by the differences in syntax on different 
systems.

Wu searches files line-by-line, using Python regular expressions.  The fnmatch 
module is used to select which files to search, throughout the entire folder tree
under a particular root.

    $ wu --help
    usage: wu [-h] [--root ROOT] [--include INCLUDE] [--colour] pattern

    positional arguments:
      pattern               regular expression to search for

    optional arguments:
      -h, --help            show this help message and exit
      --root ROOT, -r ROOT  path to root of project
      --include INCLUDE, -i INCLUDE
                            shell pattern of files to find (eg "*.py")
      --colour, -c          colourize output

For example, to find all occurences for the phrase "test" in a set of Python 
files:

    $ wu --root /path/to/my/project -i "*.py" "test"
    /path/to/my/project/one.py:3:import test
    /path/to/my/project/one.py:5:test.test()
    /path/to/my/project/test/__init__.py:4:from f import test

To make it more convenient, when I'm working in a certain project, I use a bash
function to avoid re-typing some of the arguments:

    $ export WORKHOME=/path/to/my/project
    $ function wu() { /usr/local/bin/wu -c -r $WORKHOME -i "*.py" "$@" ; }
    $ wu "test"
    /path/to/my/project/one.py:3:import test
    /path/to/my/project/one.py:5:test.test()
    /path/to/my/project/test/__init__.py:4:from f import test
