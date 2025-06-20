# Welcome

> **Random Quote:** A gentle answer turns away wrath, but a harsh word stirs up anger.

Void Linux is an independent, rolling-release Linux distribution built from scratch. It is known for its minimalism, simplicity, and unique choices, such as its use of the `runit` init system and the in-house `xbps` package manager. Unlike other distributions, Void does not derive from Debian, Arch, or Red Hat. It stands entirely on its own.

Void supports both `glibc` and `musl` C libraries, offers fast boot times, and provides a clean base that allows users to build up only what they need. It targets users who value control, clarity, and performance.

This is further discussed [here](./00_philosophy.md).

---

## Why Deep Dive Into Void?

Studying Void Linux in depth serves two main purposes:

1. **To understand how a modern, minimalist Linux system works:** Void is relatively free from abstraction. It exposes much of its inner workings, making it an ideal distribution for learning how a Unix-like system is actually put together.

2. **To support Operating System development:** This deep dive is part of a larger project (the development of my own Operating System). By studying how Void handles core tasks like package management, process supervision, and system initialization, I aim to gain insights that will guide decisions in my own OS project.

Even if you are not building an OS, learning from Void can sharpen your understanding of Linux internals and help you become a more capable and confident system-level user.

---

## How to Use These Notes

+ **Read progressively:** The notes are structured from foundational topics to more advanced ones. The [roadmap](../roadmap/README.md) can guide you through the recommended order.
+ **Experiment as you learn:** Most sections include command examples and projects. Try them out on a Void install or in a virtual machine.
+ **Refer to the index:** Use the [SUMMARY.md](../SUMMARY.md) file to quickly locate topics.
+ **Consult external resources:** Where relevant, links to official documentation and other helpful materials are provided.

---

## Final Note

This is not a quick guide. It is a serious exploration meant for those who want to understand Void Linux deeply, not just how to use it, but how it works.

---
