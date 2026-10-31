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
