# gres
Globally search for a Regular Expression and Substitute
## Why not just use `sed`?
A couple of reasons that I made my own script instead of just using `sed`:
* With `sed`, you have to manually iterate through a directory to replace a string of text. E.g.:
``` sh
$ mkdir tmp
$ echo "foo" > tmp/file
$ sed -i "" "s/foo/bar/" tmp
sed: tmp: in-place editing only works for regular files
$ for file in tmp/*
for> sed -i "" "s/foo/bar/" $file
$ cat tmp/file
bar
```
^ And that approach only does a search with a maxdepth of 1. So, if you don't want to manually `cd` into each subdirectory that you want to affect, then you have to utilize something like `find` with `sed`:
```sh
$ tree tmp # here i'd want to replace the text in each of these files:
tmp
├── foo
└── tmp
    ├── bar
    └── tmp
        └── baz

2 directories, 3 files
$ find tmp -type f -exec sed -i "" "s/original string/new string/" {} \;
```
^ I personally find that cumbersome, even though it handles the maxdepth problem. So, as you can see, recursively searching with `sed` sux, at least in my opinion.
* With `gres`, you just feed it a directory and it does the recursion for you (a la `grep`):
``` sh
$ mkdir tmp
$ echo "foo" > tmp/file
$ gres "s/foo/bar/" tmp
$ cat tmp/file 
bar
```
It also uses the stdlib's `Find#find` method (which is effectively the same as the `find` program), so you don't need to worry about the maxdepth, since it assumes the default behavior of `find` and recursively searches through directories and their subdirectories.
* Another reason I did this was so that I could replace text in files without `sed` adding that newline at the end. I know that is part of the POSIX definition of a file, but that newline just irks me sometimes.
