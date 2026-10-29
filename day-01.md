# Day 1 - 1 November 2026

## Hours spent
About 1.5 hours.

## What I installed or built
- I checked that Python, WSL, Docker and Git were already installed on my laptop.
- I opened Ubuntu in WSL with `wsl -d Ubuntu`. I used `cd` to change folders, `pwd` to see which folder I am in, `ls` to list files, and `ls -la` to list files with their permissions.
- I learned that `~` is not the root. It is my home folder, `/home/chethan`. The root of the tree is `/`.
- I learned that my Windows C: drive is mounted inside Ubuntu at `/mnt/c`. Ubuntu's own files live under `/`, and mine are in `/home/chethan`.
- I set up Git with my user name and email.
- I created an SSH key and added the public key to GitHub.
- I used `cat ~/.ssh/id_ed25519.pub` to show my public key, and `ssh -T git@github.com` to test the connection to GitHub.
