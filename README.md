# gres
Globally search for a Regular Expression and Substitute
## Why not use `sed`?
A couple of reasons that I made my own script instead of just using `sed`:
* With `sed`, you have to manually iterate through a directory to replace a string of text. E.g.:
``` sh
$ mkdir tmp
$ echo "foo" > tmp/file
$ sed -i "" "s/foo/bar/" tmp
sed: tmp: in-place editing only works for regular files
$ for file in tmp/*
for> sed -i "" "s/foo/bar/" $file
$ cat tmp/file
bar
```
With `gres`, you just feed it a directory and it does the recursion for you (a la `grep`):
``` sh
$ mkdir tmp
$ echo "foo" > tmp/file
$ gres "foo\bar" tmp
$ cat tmp/file 
bar
```
* Another reason I did this was so that I could replace text in files without `sed` adding that newline at the end. Yes, I know that is part of the POSIX definition of a file, but that newline is always rather irksome to me.
