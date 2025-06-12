# Services on Void Linux

> **Random Quote:** The function of good software is to make the complex appear simple.

A service is a background process, also called a daemon, which starts at boot or is triggered by events. Void Linux uses **runit** as its init system and service manager. It is fast, easy to understand and modular (each service is independent).

---

## Key Topics

+ [Service Directories](#service-directories)
+ [Basic Service Management](#basic-service-management)
+ [Creating a Custom Service](#creating-a-custom-service)

---

## Service Directories

Services in runit live in two locations.

+ `/etc/sv`: Contains all available service definitions.
+ `/var/service`: Contains enabled (active) services.

**Note:** The `/var/service` directory mostly contains links to services in `/etc/sv`.

**How it works:**

+ **At boot:** `runit` launches `runsvdir`. `runsvdir` watches `/var/service`. For every directory/link it sees, it spawns a `runsv` process to supervise the service. Any service symlinked into `/var/service` at boot time, will be started immediately and monitored.

+ **At runtime:** `runsvdir` keeps monitoring `/var/service`. If you add a symlink later, `runsvdir` instantly detects it, spins up `runsv`, and `runsv` starts the service's `run` script (launches the service).
    
    When you remove a service from `/var/service`, `runsvdir` notices and stops the `runsv` process responsible for the removed service. If a `finish` script is present in the service, it will be executed.

---

## Basic Service Management

Starting, stoping, restarting, enabling, disabling and checking status.

### Checking the Status of a Service

This command is used to check if a service is running, down, supervised, or restarting:

```bash
sudo sv status foo # For enabled services.

sudo sv status /etc/sv/foo # For disabled services.
```

#### Possible Outputs

1. **Example 1**

```bash
run: sshd: (pid 1234) 2h 15m
```

+ `run:` Service is running
+ `sshd:` The name of the service
+ `(pid 1234)`: Process ID of the service
+ `2h 15m`: How long it's been running

2. **Example 2**

```bash
down: foo: 5s, normally up
```

+ `down:` Service is down (not running).
+ `foo:` The name of the service.
+ `5s`: The service has been down for 5s.
+ `normally up`: The service is expected to be running.

You may get this message if:
+ The service crashed and has not been restarted yet.
+ You manually stopped the service.

3. **Example 3**

```bash
run: foo: (pid 1111) 1s, normally up; runsv: fatal: unable to start ./run: permission denied
```

+ `run:` The service tried to start.
+ `foo:` The name of the service.
+ `(pid 1111)`: The PID of the process that just started.
+ `1s`: It's been running for only 1 second.
+ `normally up`: Service is expected to stay running.
+ `runsv: fatal: ...` Error message from the supervisor, it couldn't launch the service properly.
+ `unable to start ./run: permission denined`: The `run` script exists but isn't executable or readable by runit.

You may see this message if:

+ A less privileged user owns the `run` script. It should be owned by root.
+ The file lacks the executable permission.
+ `run` is in the wrong place.

4. **Example 4**

```bash
fail: foo: unable to control /var/service/foo: file does not exist
```

+ `fail:` The `sv` command failed to run the service.
+ `foo:` The name of the service.
+ `unable to control /var/service/foo:` The command failed to interact with the supervision directory.
+ `file does not exist` The service is not enabled (no symlink in `/var/service`) or the path is wrong or the service if wrongly named.

You'll get this if:

+ You forgot to `ln -s /etc/sv/foo /var/service/`.
+ You misspelled the name of the service.
+ You're trying to `sv status` a service that was only started with `sv up` and isn't currently supervised.

#### What happens behind the scenes

`sv status` communicates with the `runsv` process supervising the service via a UNIX domain socket in the service's directory (e.g., `/run/service/foo/supervise`). If that socket isn't there, supervision is broken or nonexistent.

### Enabling a Service

Enabling a service means that the service will start automatically at boot. It will be started by `runsvdir`.

It involves creating a symbolic link from the service definition directory (`/etc/sv`) to the service supervision directory (`/var/service`) so that `runsvdir` will start and supervise it at boot.

**How it's done:**

```bash
sudo ln -s /etc/sv/foo /var/service/
```

+ This command creates a symbolic link of the `/etc/sv/foo` service in the `/var/service/` directory.
+ At runtime, `runsvdir` detects it and starts the service.
+ At boot, `runsvidir` will start it together with every other service in the `/var/service` directory.

### Disabling a Service

Disabling a service is preventing the service from automatically starting at boot. It involves removing the service's symbolic link from the service supervision directory (`/var/service`).

**How it's done:**

```bash
sudo rm /var/service/foo
```

+ This removes the symbolic link. Now, `runsvdir` won't start the `foo` service at boot.
+ Also, `runsvdir` detects that the symlink is removed and automatically stops the service.

### Starting a Service

There are different ways of starting a service, each with different use cases and levels of persistence.

1. `sv up /etc/sv/foo`:
    + This command starts the service `/etc/sv/foo`, without adding it to `/var/service` (without enabling it). The service will not auto-start at boot.
    + **Note:** This command requires the full path to the service.

2. `sv start foo`:
    + This command starts the `foo` service.
    + This command only works for enabled services. It assumes that the `foo` service is located in `/var/service`. This means that the command will fail if `foo` is not in `/var/service`.
    + **Note:** This command does not require the full path to the service, only the service name.

3. `runsv /etc/sv/foo`:
    + This runs a service without needing `runsvdir`. The service is not added to `/var/service`. This service will also not auto-start at boot.

### Stopping a Service

There are multiple ways of stopping a service.

1. `sv down /etc/sv/foo`: This command stops the service `/etc/sv/foo`. This is used to stop services that were started by `sv up <service>`.
    + **Note:** This command requires the full path to the service.

2. `sv stop foo`:
    + This command stops the `foo` service.
    + Like `sv start foo`, this command only works for enabled services. It assumes the `foo` service is located in `/var/service`. This command will fail if `foo` is not in `/var/service`.
    + The service is stopped but remains enabled.
    + **Note:** This command does not require the full path to the service, only the service name.

3. Manually kill the `runsv` process. If `sv down` fails, or if you like doing things manually, you can:
    + Find the `runsv` PID:
    ```bash
    ps aux | grep runsv | grep /etc/sv/foo
    ```

    + Kill it:
    ```bash
    sudo kill <pid>
    ```
    
    + This stops both the supervising `runsv` process, and the actual service it was running.

---

## Creating a Custom Service

+ Creating and running a service.
+ Logging services. Learn about other commands that read files (e.g., reading the last 10 lines of a file).
+ One time services.
+ Debugging services.
