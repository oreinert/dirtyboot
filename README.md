# dirtyboot

Whenever a Linux system does _not_ shut down cleanly (e.g., due to a power
outage, a system crash, etc), it is almost always an event of interest to the
system administrator. This simple script keeps track of such events by
detecting the subsequent "dirty" reboot, and writing a system log entry about
it.

## Mechanism

A systemd service, started early in the boot process, writes a marker file
(`/var/lib/dirtyboot` by default).  When the service is stopped, during system
shutdown, the marker file is removed.

Thus, if the marker file is already present when the service starts, it means
the system wasn't shut down cleanly before rebooting. Whenever this condition
occurs, the service writes a corresponding warning to the system log.

## Prerequisites

A Linux system based on [systemd](https://freedesktop.org/wiki/Software/systemd/).

## Inspecting dirty boot warnings

Run the command

    journalctl -t dirtyboot

## Installation

Put the systemd unit file in `/etc/systemd/system`, and the shell script to
`/usr/sbin/dirtyboot`.  Then enable the service using

    systemctl enable dirtyboot

## Motivation

I had an unstable system which tended to hang after long periods (days) of
being mostly idle. I used this simple tool to keep track of the number of hard
resets I had to do to restore operation of a hung system.  Since system log
entries are timestamped, it also records the time interval between such hard
resets.
