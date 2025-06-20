# Void's Philosophy

> **Random Quote:** In all your ways acknowledge Him, and He will make straight your paths.

## Key Topics:

+ [Void's Core Philosophy](#voids-core-philosophy)
+ [`musl` vs `glibc`](#musl-vs-glibc)
+ [Why `runit`?](#why-runit)

---

## Void's Core Philosophy

Void Linux is an independently developed, general-purpose operating system that adheres to a set of foundational principles emphasizing simplicity, minimalism, and user autonomy. The distribution distinguishes itself through technical choices that favor maintainability, performance, and transparency over automation or widespread adoption.

### Simplicity in Design

Void Linux promotes simplicity in system architecture rather than in user experience. Its tools and configurations are designed to be minimal and predictable, adhering to the UNIX philosophy of "doing one thing well."

+ System configurations are typically stored in plaintext and follow a clear logical structure.
+ Layers of abstraction are minimized to reduce complexity.
+ The system encourages manual configuration over automated solutions, giving users full visibility into system behavior.

Void Linux targets technically proficient users who prefer structural clarity over ease of use.

### No `systemd`.

A defining feature of Void Linux is its complete avoidance of the `systemd` init system. In its place, Void employs `runit`, a lightweight, fast, and modular init scheme.

+ `runit` provides a straightforward model for service supervision, where each service is represented by a directory with run scripts.
+ It enables rapid boot times  and is notably easier to audit and modify compared to `systemd`.
+ The decoupling of init functionality from other system components supports Void's principle of modularity.

### Rolling Release Model

Void Linux follows a rolling release model, wherein system packages are continuously updated rather than issued in versioned snapshots.

+ This ensures access to the latest software versions without requiring full system upgrades.
+ While the system remains cutting-edge, updates are introduced with caution to maintain system stability.
+ Users are expected to manage updates proactively to preserve long-term reliability.

### Support for Both Binary and Source Packages

Void Linux offers flexibility in software installation through two mechanisms:

+ **XBPS (X Binary Package System):** A native package manager designed specifically for Void, enabling efficient installation and removal of precompiled packages.
+ `xbps-src`: A ports-like system for building packages from source, suitable for advanced users requiring custom compilation flags or experimental versions.

This dual-mode accommodates both convenience and customization.

### Independent Development

Void Linux is not derived from any other distribution (e.g., Debian, Arch). It is a fully independent system, built from scratch with its own:

+ Package manager system (XBPS).
+ Build infrastructure (`xbps-src`).
+ Init system (`runit`).
+ Repository structure and policies.

Void reflects a deliberate, coherent engineering philosophy rather than an inherited one.

### Pragmatic Design Principles

Void Linux favors technical pragmatism over ideological strictness. Its design decisions are guided by the goals of performance, clarity, and user control.

+ There is no official desktop environment or default user interface.
+ The system avoids intrusive automation, allowing users to define their computing environment as needed.
+ While minimalist, Void does not shy away from features if they are cleanly implemented and technically justified.

It is a system built for users who prioritize technical transparency and operational control.

---

## `musl` vs `glibc`

Void Linux uniquely offers two versions of its distribution:

- one based on `glibc` (the GNU C Library),
- and another based on `musl` (musl libc).

These are both implementations of the standard C library, which is a core component of any Linux system, providing essential functions for system calls, memory management, string operations, and more.

Choosing between `glibc` and `musl` carries significant implications for compatibility, performance, system behavior and user experience. Below is a comparison of the two:

### Overview of Each C Library

+ `glibc`: The GNU C Library. The standard on most Linux distributions including Debian, Fedora and Arch. It is mature, widely supported, and highly compatible.

+ `musl`: A lightweight, fast, and standards-compliant alternative C library designed for static linking, simplicity, and correctness. Popular in minimalist and containerized environments.

### Binary Compatibility

+ `glibc` offers near-universal binary compatibility with precompiled Linux software. Most third-party binaries (e.g., proprietary software or generic `.deb`/`.rpm` builds) assume the presence of `glibc`.

+ `musl` is not binary-compatible with `glibc`. As a result:
    - Precompiled binaries built against `glibc` will often fail to run on a `musl` system unless specifically patched or rebuilt.
    - Users may encounter difficulties running closed-source or proprietary software (e.g., Google Chrome, Steam).

> Implication: If binary compatibility with commercial or closed-source software is essential, `glibc` is the safer choice.

### Performance and Size

+ `musl` is designed to be small, fast, and efficient. It has:
    - A significantly smaller footprint.
    - Faster startup times for statically linked binaries.
    - Lower memory usage in many scenarios.

+ `glibc`, while feature-rich, is heavier:
    - Larger in size due to its extensive features and internationalization support.
    - Somewhat slower in static use cases but more optimized for long running, complex processes.

> Implication: For embedded systems, containers, or minimal distributions, `musl` provides a leaner alternative.

### Standards Compliance and Behavior

Both `glibc` and `musl` aim to comply with POSIX and other standards. However:

+ `musl` is strict in adherence to standards and avoids non-standard extensions.
+ `glibc` includes many GNU-specific extensions, which are sometimes relied upon by software written for Linux.

> Implication: Software using GNU extensions may require patching or fail to compile against `musl`.

### Development Ecosystem

+ **Toolchains:** Void provides toolchains for both `glibc` and `musl`, but the majority of mainstream Linux development targets `glibc`.

+ **Porting software:**
    - Developers on `musl` may need to patch software to remove `glibc` dependencies.
    - Certain build systems and packages may assume `glibc` behavior, requiring additional effort to adapt.

> Implication: `musl` is better suited for experienced users comfortable troubleshooting build or runtime issues.

### Security and Reliability

+ `musl` emphasizes simplicity and correctness in its design, leading to a smaller attack surface. Its static linking model can improve deployment security by reducing runtime dependencies.

+ `glibc` has had more historical vulnerabilities due to its size and complexity but benefits from extensive auditing and community support.

> Implication: `musl` can provide a more predictable and potentially safer runtime environment for tightly controlled systems.

---

## Why `runit`?

Apart from the already mentioned advantages of `runit` (speed, simplicity, modularity), here are additional reasons behind its selection in Void Linux.

### Strict Separation of Concerns

`runit` adheres to a strict separation of responsibilities. It performs only the core tasks expected of an init system:

+ Process supervision.
+ Service management.
+ System boot and shutdown orchestration.

This separation avoids monolithic design and supports Void Linux's modular and minimalistic philosophy. This way, the system remains more transparent, auditable, and maintainable.

### Statelessness and Predictability

`runit` avoids persistent state files or runtime databases. Each service's state is determined by the presence of simple scripts and symlinks in predictable directories (`/etc/sv/` and `/var/service/`).

+ There is no need to "re-enable" services after changes.
+ A service is either running (linked to `/var/service/`) or not.

This design ensures predictable, repeatable behavior, ideal for debugging and recovery.

### Consistency Across All Service Phases

`runit` handles all three phases of system lifecycle consistently:

+ **Stage 1:** One-time system initialization.
+ **Stage 2:** Long-lived service supervision.
+ **Stage 3:** Graceful shutdown.

Each stage is implemented as a straightforward shell script (`/etc/runit/1`, `2`, and `3` respectively), giving full visibility and control to the user or system maintainer.

### Process Supervision Built-in

`runit` provides native process supervision:

+ It monitors services continuously.
+ Automatically restarts them on failure.
+ Logs failures and exit statuses without additional daemons.

This reduces the need for external supervision tools like `monit`, `supervisord`, or even custom `systemd` unit directives.

### Ease of Service Creation and Debugging

To define a service in `runit`, one only needs to create a directory with an executable `run` script. This offers:

+ Simple debugging via shell output.
+ No need to learn a complex unit file syntax.
+ Services can be manually invoked by running the script.

In contrast, `systemd` unit involve many optional directives and syntactic rules, increasing cognitive overhead.

### Portability and Independence from Linux-Specific Features

`runit` does not depend on Linux-specific features like, `cgroups`, `udev`, or `dbus`, unlike `systemd`. This makes it:

+ Easier to port to non-Linux systems (e.g., BSDs).
+ More robust across minimal environments or containers.

This aligns with Void Linux's desire for system independence and minimal external dependencies.
