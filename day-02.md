# Day 2 - 30 October 2026

## Hours spent
About 1 hour, including last night.

## What I practised
- I used `pwd`, `ls` and `cd` to see where I am, list files, and move between folders.
- I used `mkdir` and `touch` to create folders and empty files.
- I used `echo "hello linux" > a.txt` to write text into a file. `>` overwrites the file, and `>>` adds to the end.
- I used `cat`, `cp`, `mv` and `rm` to read, copy, rename and delete files.
- I used `chmod` to change file permissions.

## What went wrong, and how I fixed it
- I typed `~d` instead of `-d`, so Linux treated `~d` as a command and said "command not found". The correct flag is `-d`.
- I ran my commands in `~/projects` instead of `~/projects/linux-practice`, so `mv` said "cannot stat 'b.txt': No such file or directory", because `b.txt` was in the other folder. I fixed it with `cd ~/projects/linux-practice`.
- I typed `ls la` without the dash, so `ls` looked for a file named `la`.

## One thing I understood better
`chmod 644` means `-rw-r--r--`: the owner can read and write, and the group and others can only read. `chmod 000` means `----------`: nobody has any permission, so `cat` says "Permission denied".

## One thing still unclear
Nothing major today. Next I want to understand what `x` (execute) does on a file.
