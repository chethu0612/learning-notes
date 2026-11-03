# Linux, Git and Cloud Ops Cheat Sheet (Days 1 to 3)

Built from my own daily practice in WSL Ubuntu. Every command here is one I have run myself.
Last updated: 3 November 2026.

---

## 1. Where am I, and what is here?

| Command | What it does |
|---|---|
| `pwd` | Print the folder I am in |
| `ls` | List files |
| `ls -la` | List all files (including hidden) with permissions, owner and size |
| `cd folder` | Move into a folder |
| `cd ..` | Go up one folder |
| `cd ~` | Go to my home folder (`/home/chethan`) |

- `~` is my home folder. The Windows drives are under `/mnt/c`, `/mnt/d`.
- A path starting with `/` is from the root of the disk. `cd /linux-practice` fails; `cd linux-practice` works from the right folder.
- Linux files are in the Linux filesystem (`~/projects`), not under `/mnt/c`, for speed.

Important folders:

| Folder | What is in it |
|---|---|
| `/home/chethan` (`~`) | My home folder and projects |
| `/etc` | Configuration files (`/etc/passwd`, `/etc/shadow`, `/etc/cron.daily`) |
| `/var/log` | Log files |
| `/mnt/c` | My Windows C: drive, seen from Ubuntu |
| `/` | The root of the whole filesystem |

---

## 2. Files and folders

| Command | What it does |
|---|---|
| `mkdir name` | Create a folder |
| `touch file` | Create an empty file |
| `echo "text" > file` | Write text into a file (**overwrites** it) |
| `echo "text" >> file` | Add text at the end (**appends**) |
| `cat file` | Show the contents of a file |
| `cp a b` | Copy a to b |
| `mv a b` | Rename or move a to b |
| `rm file` | Delete a file (no recycle bin, it is gone) |
| `grep -n "text" file` | Find a word in a file, with line numbers |

## 3. Reading output, pipes and editing

| Command | What it does |
|---|---|
| `cmd1 \| cmd2` | **Pipe**: send the output of one command into the next |
| `ps aux \| head -5` | `head` shows the **first** lines (here, the first 5) |
| `cat day-03.md \| tail -20` | `tail` shows the **last** lines (here, the last 20) |
| `ps aux \| grep sleep` | Filter long output for one word |
| `less file` | Open long output in a pager. Press `q` to quit |
| `--no-pager` | Add to a command (for example `systemctl status cron --no-pager`) so it prints and does not open `less` |
| `nano file` | Simple editor. `Ctrl+O` then Enter saves, `Ctrl+X` exits, `Ctrl+End` jumps to the bottom |
| `cat >> file <<'EOF'` ... `EOF` | Append a block of text to a file (the `EOF` line ends it; do not paste the `EOF` into an editor) |
| `sed -i '/^EOF$/d' file` | Remove any stray `EOF` line from a file |
| `grep -n "EOF" file` | Check that nothing is left (prints nothing if clean) |

---

## 4. Permissions

`-rw-r--r--` reads as: type, then owner, group, others (three letters each).
`r` = read (4), `w` = write (2), `x` = execute (1).

| Command | Result |
|---|---|
| `chmod 644 file` | `-rw-r--r--` (owner read/write, others read) |
| `chmod 600 file` | `-rw-------` (only the owner) |
| `chmod 000 file` | `----------` (nobody) |

- `d` at the start (`drwxr-xr-x`) means a folder.
- Example: `/etc/passwd` is `-rw-r--r--` (everyone can read). `/etc/shadow` is `-rw-r-----`, owner root, group shadow, so I get `Permission denied`.
- My SSH private key must be `-rw-------` (600).

## 5. Git and GitHub

| Command | What it does |
|---|---|
| `git config --global user.name "name"` | Set my name |
| `git config --global user.email "email"` | Set my email |
| `git config --global init.defaultBranch main` | Default branch name |
| `git clone git@github.com:user/repo.git` | Copy a repository |
| `git status` | What changed, what is staged |
| `git add file` | Stage a file |
| `git commit -m "message"` | Save a snapshot with a message |
| `git push` | Send commits to GitHub |
| `git diff` | Show changes not yet staged |
| `git diff --staged` | Show staged changes (check before commit) |
| `git log --oneline` | Short history (two dashes) |

Daily routine: `git add file` then `git status` then `git commit -m "Day N: topic"` then `git push` then `git log --oneline`.

Mistakes I made and learned from:
- `git push day-02.md` fails: after `git push` comes a remote name (`origin`), not a file. Plain `git push` is enough.
- `git diff` shows nothing for a brand new untracked file. Use `git add` first, then `git diff --staged`.
- `git log -oneline` fails. Long options use two dashes: `--oneline`.
- A heredoc end marker (`EOF`) can end up inside a file if pasted into an editor. Check with `grep -n "EOF" file`.

## 6. SSH key for GitHub

| Step | Command |
|---|---|
| Create a key | `ssh-keygen -t ed25519` |
| Show the public key | `cat ~/.ssh/id_ed25519.pub` |
| Test the login | `ssh -T git@github.com` |

- `id_ed25519` is the **private** key. Never share it.
- `id_ed25519.pub` is the **public** key. This is what I add to GitHub.

## 7. Processes

| Command | What it does |
|---|---|
| `ps` | Processes from my own terminal |
| `ps aux` | Every process on the machine (user, PID, CPU, memory, command) |
| `top` | Live view (press `q` to quit) |
| `sleep 600 &` | Start a test job in the background |
| `ps aux \| grep sleep` | Find a process by name |
| `kill PID` | Ask it to stop politely (SIGTERM) |
| `kill -9 PID` | Force stop (last resort) |

- A **PID** is the number Linux gives each running process.
- `&` runs a command in the background. `[1] 602` means job 1, PID 602.
- PID 1 is `/sbin/init`, which is **systemd**.
- After `kill`, check with `ps aux | grep name`. Only the `grep` line itself should remain.

## 8. Services with systemd

| Command | What it does |
|---|---|
| `systemctl status` | State of the whole machine (`running`, `Failed: 0 units`) |
| `systemctl status cron --no-pager` | One service: running? PID, memory, recent log lines |
| `systemctl list-units --type=service --state=running` | All running services |
| `sudo systemctl restart cron` | Stop and start again |
| `sudo systemctl stop cron` | Stop it |
| `sudo systemctl start cron` | Start it |

- **Enabled** = starts automatically at boot. **Active** = running right now. They are independent.
- After a **restart**: the PID changes, the "active since" time resets, memory and CPU reset. The service name, command and `active (running)` stay the same.
- After a **stop**: `inactive (dead)` but still `enabled`.

**cron** is the service that runs scheduled jobs.
- Its config lives under `/etc/cron.daily` and similar folders.
- A log line like `CMD (... run-parts --report /etc/cron.daily)` means the daily jobs ran.
- `@reboot` jobs run at system start. After my `restart cron` the log said "Skipping @reboot jobs -- not system startup", so cron knew it was a restart and not a boot.
- Other services I saw running: `rsyslog` (logging), `systemd-journald` (journal), `systemd-resolved` (DNS names), `systemd-logind` (logins), `dbus`.

---

## 9. Logs with journald and journalctl

systemd-journald **writes** the logs. `journalctl` **reads** them.

| Command | What it does |
|---|---|
| `journalctl -u cron --no-pager \| tail -15` | Last lines for one service |
| `journalctl -u cron --since "06:30" --until "06:40"` | Only a time window |
| `journalctl -p err -n 10 --no-pager` | Last 10 error-level messages |
| `journalctl -u cron -f` | Follow live (Ctrl+C to stop) |

- Log times are **UTC**. IST = UTC + 5:30. A problem at 3 pm IST is 09:30 UTC.
- An error line is not automatically the cause. On WSL, `dxg`, `pam_lastlog` and `getaddrinfo` errors are noise; `Failed: 0 units` and running services showed nothing was broken.

## 10. Users and sudo

| Command | What it does |
|---|---|
| `whoami` | My username |
| `id` | My uid, gid and groups |
| `who` | Who is logged in |
| `getent passwd chethan` | The account record (user, UID, GID, home, shell) |
| `sudo command` | Run one command as root |

- `root` is uid 0. My user is uid 1000.
- Groups that matter: `sudo` (can use sudo), `adm` (can read logs in `/var/log`).
- `/etc/passwd` is readable by all; the `x` in it means the password hash is stored in the protected `/etc/shadow`.
- Never paste the contents of `/etc/shadow` anywhere.

## 11. My first-look checklist for a service problem

1. Is it running? `systemctl status <service> --no-pager`
2. What do the logs say around the reported time? `journalctl -u <service> --since "..." --until "..."` (convert IST to UTC)
3. Is the process there? `ps aux | grep <name>`
4. Is it a permission problem? `ls -l <file>` and `id`
5. Restart if allowed, then confirm: new PID, `active (running)`, and a clean log.

## 12. AWS and safety habits

- Root account has MFA enabled. Never share passwords, OTPs or QR codes.
- A zero-spend budget is set. AWS is used only for short, cost-checked labs.
- Delete lab resources right after each lab, then check the Billing and Credits pages.
- Keep private things out of public repositories: no keys, passwords, payslips, client names or internal tool names.

## 13. WSL, Windows and tool checks (Day 1)

| Command | What it does |
|---|---|
| `wsl -d Ubuntu` | Open Ubuntu from PowerShell |
| `wsl -l -v` | List installed distros and WSL version (mine: Ubuntu, version 2) |
| `uname -a` | Kernel and system info |
| `git --version` | Git version (2.43.0 on my Ubuntu) |
| `terraform version` | Terraform version (v1.17.1, installed from the HashiCorp apt repo) |

- In PowerShell I checked which tools exist: Git, Python 3.13, VS Code, Docker and WSL were found. Terraform was missing, so I installed it inside Ubuntu.
- Typing mistakes I made and fixed: `wsl ~d` should be `wsl -d`, `ls la` should be `ls -la`, `git comit` should be `git commit`.
- Work in the Linux filesystem (`~/projects`), not under `/mnt/c`, for speed and fewer permission problems.

---

## 14. Installed but not used yet

- Terraform (v1.17.1) is installed. First use is planned for Week 3.
