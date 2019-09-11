# Check Descriptions

As part of the diagnostics suite, you will find various checks that can be run on-device. Below is a description of each
check and what each means or how to triage.

A `check` in this context is defined as a function that returns a result (good/bad status plus some descriptive text), whereas a
command is simply a data collection tool without any filtering or logic built in). Checks are intended to be used by
everyone, while typically command output is used by support/subject matter experts and power users.

## `check_resin1x`
### Summary
As of SOME DATE, `resinOS 1.x` has been deprecated. These OSes are now unsupported.

### Triage
Upgrade your device to `balenaOS 2.x` (contact support).

## `check_under_voltage`
### Summary
Often seen on Raspberry Pi devices, these kernel messages indicate that the power supply is insufficient for the device and any peripherals that might be attached. These errors also precede seemingly erratic behavior.

### Triage
Replace the power supply with a known-good supply (typically supplying at least 2.5V).

## `check_memory`
### Summary
This check simply confirms that a given device is running at a given memory threshold (set to 90% at the moment).
Oversubsribed memory can lead to OOM events.

### Triage
Using a tool like `top`, scan the process table for which process(es) are consuming the most memory (`%VSZ`) and check
for memory leaks in those applications.

## `check_container_engine`
### Summary
This check confirms the container engine is up and healthy. The container engine is an integral part of the balenaCloud
pipeline.

### Triage
It is best to let balena's support team take a look before restarting the container engine. At the very least, take a
diagnostics snapshot before restarting anything.

## `check_supervisor`
### Summary
This check confirms the supervisor is up and healthy. The supervisor is an integral part of the balenaCloud pipeline.

### Triage
It is best to let balena's support team take a look before restarting the supervisor. At the very least, take a
diagnostics snapshot before restarting anything.

## `check_dns`
### Summary
Misconfigured DNS can often cause network issues if the local `dnsmasq` instance is bypassed. In the hostOS, the
aforementioned instance should always be the first entry.

### Triage
Contact support to investigate further.

## `check_diskspace`
### Summary
This check simply confirms that a given device is running beneath a given disk utilization threshold (set to 90% at the moment).
If a local disk fills up, there are often knock-on issues in the supervisor and application containers.

### Triage
Run `du -a /mnt/data/docker | sort -nr | head -10` in the hostOS shell to list the ten largest files and directories.
If the results indicate large files in `/mnt/data/docker/containers`, this result often indicates a leakage in an
application container that can be cleaned up (runaway logs, too much local data, etc).