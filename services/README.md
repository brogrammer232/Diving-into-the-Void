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

### Enabling a service
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
    + This command starts the service, `/etc/sv/foo` without adding it to `/var/service`. The service will not auto-start at boot.
    + **Note:** This command requires the full path to the service.

2. `sv start foo`:
    + This command starts the `foo` service.
    + This command only works for enabled services. It assumes that the `foo` service is located in `/var/service`. This means that the command will fail if `foo` is not in `/var/service`.
    + **Note:** This command does not require the full path to the service.

3. `runsv /etc/sv/foo`:
    + This runs a service without needing `runsvdir`. The service is not added to `/var/service`. This service will also not auto-start at boot.

### Stopping a Service


---

## Creating a Custom Service

+ Creating and running a service.
+ Logging services. Learn about other commands that read files (e.g., reading the last 10 lines of a file).
+ One time services.
+ Debugging services.
