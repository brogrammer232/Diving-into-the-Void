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

Void Linux favors technical pragmatism over ideological strictness. Its design decisions are guided by the goals of performance, clarith, and user control.

+ There is no official desktop environment or default user interface.
+ The system avoids intrusive automation, allowing users to define their computing environment as needed.
+ While minimalist, Void does not shy away from features if they are cleanly implemented and technically justified.

It is a system built for users who prioritize technical transparency and operational control.
