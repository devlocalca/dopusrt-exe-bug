# dopusrt-exe-bug

This repo demonstrates a GPSoftware Directory Opus problem with `dopusrt.exe`.

Scripts are provided that will help replicate a dopusrt.exe issue that I believe is a bug and have hopes that it
will be fixed.

---
After setup, run these two or three bash scripts at the same time to replicate the problem:
The important thing is to run a find script (for /api/find-d for example), while the two dopus processes are running,
and then run another find script at the same time or more at the same time.

Then go to the Directory Opus lister and click between the file collection lists. The same output will be
displayed.

```
$cd find/support
$./find-d

$cd find/api
$./find-d

$cd find/aws
$./find-d
```

After running these, open a dopus lister and click between the file collections - both display the same file list
instead of the distinct results found in each script.

The output from the find scripts can be inspected and will show the expected ouput.

---

### Environment:

- Windows 10 Pro
- cygwin bash (`dos2unix` package installed)

---

### setup:

- install cygwin, install dos2unix package
- look at `set-env-laptop` and modify variables that match your environment
    - set an env var in your personal account or system variables: DOPUS_DIR
        - to point to the directory where `dopusrt.exe` is located
        - this can also be set in: $proj/set-env-laptop (inspect that file for relevant settings)

---

### run script

```
1. open a cygwin bash shell
2. cd $proj/find/support
3. ./find-d     (you may need to change the SRC directory /d says D:\
    - references: bin/p2w <file.txt>
    - references: bin/create-dopus-file-collection <col-name> <file-dos.txt>
4. repeat steps 1..3 inside directory $proj/api    
5. open dopus and click between the file collections
```

---

###

`<keyword>/find-d` script notes:

1. The `find-d` (creates output: `file-collection.txt`)
2. The script convert `file-collection.txt` to dos file format: `files-dos.txt` (runs `unix2dos`)
3. The script runs `dopusrt.exe` to create a file collection
4. => observe that all commands and all output files have the correct expected content, but
   the dopus lister does not display the correct results when clicking between the file collections

---
To replicate the problem, you have to comment out `taskkill` commands at the top and bottom of each `find-d` script
With those commands there, and running each script sequentially after one completes, multiple file collections can
successfully be created.

---
The ideal scenario is that they could be run at the same time, or even more at the same time and have the results
displayed in the Directory Opus lister display as expected.
