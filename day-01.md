# Day 1 - 29th October 2026(started early)

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

## What went wrong, and how I fixed it
- I thought Ubuntu was mounted inside `/mnt/c`. Actually it is the opposite: my Windows C: drive appears inside Ubuntu at `/mnt/c`.
- I thought I could just run `git add day-01.md` and Git would create the file. But `git add` only stages a file that already exists. I had to create the file first with `nano day-01.md`, write the content, and save it.

## One thing I understood better
`~` means my home folder, and `/` means the root folder.

## One thing still unclear
- I am not sure how `ssh-keygen -t ed25519 -C "chethan.shankar.ks@gmail.com"` generated the key, or whether a key already existed before I ran it.
- I do not know how to read the permissions shown by `ls -la`, such as `drwxr-xr-x`.

## Résumé and LinkedIn
- I wrote my résumé from scratch. I learned that every line must be something I can explain with an example in an interview, so I replaced weak wording like "efficiently monitoring" and "followed runbooks" with plain facts about what I did.
- I moved Skills and Certifications above the older sections, because recruiters scan the top of the page first and keywords like Linux and AWS should be there.
- I updated my LinkedIn headline, About section, Experience entry and skills so they match the résumé. I will only add Python, Docker and Terraform as skills after I have built a working project with each.
- I turned on Open to Work for "Recruiters only", so there is no public banner. LinkedIn says it cannot guarantee complete privacy from my current company, so I am aware of that small risk.
- I kept internal tool names, the client name and server details out of my résumé and my public repos.

