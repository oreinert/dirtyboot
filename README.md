# dirtyboot

Detect and log Linux boot not preceded by a clean shutdown.

## Mechanism

A systemd service, started early in the boot process, writes a marker file
(`/var/lib/dirtyboot` by default).  When the service is stopped, during system
shutdown, the marker file is removed.

Thus, if the marker file is already present the next time the service starts,
it means the system wasn't shut down cleanly before rebooting. Whenever this
condition occurs, the service writes a corresponding warning to the system log.

## Inspecting dirty reboot warnings

Run the command

    journalctl -t dirtyboot

## Installation

Put the systemd unit file in `/etc/systemd/system`, and the shell script to
`/usr/sbin/dirtyboot`.  Then enable the service using

    systemctl enable dirtyboot

## Motivation

I had an unstable system which tended to hang after long periods of idling.
This simple tool was used to keep track of the number of hard resets I had to
do to restore operation of a hung system.  Since system log entries are
timestamped, it also records the time interval between such hard resets.
