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
