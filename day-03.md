# Day 3 - 31 October 2026

## Hours spent
About 25 minutes, done between work.

## What I practised
- I used `ps` to see processes started from my own terminal.
- I used `ps aux` to see every process on the machine.
- I started a background job with `sleep 600 &`, found it with `ps aux | grep sleep`, and stopped it with `kill`.

## What is a PID and what does ps aux show?
A PID is the number Linux gives to every running process, like a ticket number. `ps aux` lists all processes on the machine with their user, PID, CPU and memory use, start time and the command. PID 1 was `/sbin/init`, the first process, and `systemd-journald` is the service that collects logs.

## What did & do in sleep 600 &?
The `&` runs the command in the background, so I can keep using the terminal while it runs. Without it, the terminal waits for the command to finish.

## What did kill do and how did I confirm it?
`kill 602` sent a stop signal to the `sleep` process. I confirmed it with `ps aux | grep sleep`: only the grep command itself was left, and the shell printed "Terminated sleep 600".

## One thing I want to remember
A plain `kill` asks a process to stop politely. `kill -9` forces it and should be the last resort.

## Part 2: systemd (3 November 2026)

### What is systemd and what does systemctl status show?
systemd is the service manager on Linux. It runs as PID 1 (/sbin/init) and starts, stops and restarts all the other services. `systemctl status` shows the state of the whole machine (running, number of failed units) and the tree of services under it. `systemctl status <service>` shows one service: whether it is running, its main PID, memory, CPU and its recent log lines.

### What changed after restarting cron, and what stayed the same?
After `sudo systemctl restart cron`, the main PID changed (176 to 708), the "active since" time reset, and memory and CPU were reset (1.1M to 492K, 26ms to 2ms). The service name, the command it runs, enabled, and active (running) stayed the same, so the service came straight back up.

### Enabled vs active (with my own cron stop/start example)
Enabled means the service starts automatically at boot. Active means it is running right now. I stopped cron with `sudo systemctl stop cron` and it showed inactive (dead) but still enabled. After `sudo systemctl start cron` it was active (running) again with a new PID (853).

### Commands I used
- systemctl status
- systemctl list-units --type=service --state=running
- sudo systemctl restart / stop / start cron

## Part 3: logs with journalctl (3 November 2026)

### What is journalctl, and what reads or writes the logs?
journalctl is the command I use to read the system logs. It does not show running processes (that is ps aux). systemd-journald is the service that collects and writes the logs, and journalctl reads what journald wrote.

### How do I find what happened to a service at a reported time?
I filter the service's logs by time. For a problem reported at 3 pm IST, which is 09:30 UTC, I run `journalctl -u cron --since "09:30" --until "09:40" --no-pager`. The log times are in UTC, so I convert IST to UTC first. I tried this with my own cron stop and start: the window 06:30 to 06:40 UTC showed my stop at 06:33:19 and my start at 06:33:48.

### Why is not every journalctl -p err line a real problem?
An error line is not automatically the cause. In my own output, `journalctl -p err` showed WSL graphics driver (dxg) errors, pam_lastlog and a getaddrinfo message at startup. But `systemctl status` showed Failed: 0 units and all services were running, so those lines were WSL noise and not a fault. I check the time, the service name and whether something actually stopped working.

### Commands I used
- journalctl -u cron --no-pager | tail -15
- journalctl -p err -n 10 --no-pager
- journalctl -u cron --since "06:30" --until "06:40" --no-pager
- journalctl -u cron -f (follow live, Ctrl+C to stop)

## Part 4: users and sudo (3 November 2026)

### What did id and getent passwd show me?
`id` showed my user ID (uid 1000), my main group and the other groups I belong to. Two matter for support work: `sudo` lets me run commands as root, and `adm` lets me read the log files in /var/log. `getent passwd chethan` showed the account record: username, UID, GID, home folder (/home/chethan) and login shell (/bin/bash). The `x` in the password field means the password hash is kept in /etc/shadow.

### Why could I not read /etc/shadow?
/etc/passwd is -rw-r--r--, so everyone can read it. /etc/shadow is -rw-r-----, owned by root with group shadow, so others get no access. I am not root and not in the shadow group, so `cat /etc/shadow` gave Permission denied. It holds password hashes, so it is protected.

### What does sudo do?
`sudo whoami` printed root. sudo runs one command as root, and it works because I am in the sudo group.

### Commands I used
- whoami, id, who
- getent passwd chethan
- ls -l /etc/passwd /etc/shadow
- cat /etc/shadow (permission denied)
- sudo whoami
