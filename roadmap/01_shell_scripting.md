# Shell Scripting

> **Random Quote:** Above all else, guard your heart, for everything you do flows from it.

This roadmap provides a structured and in-depth guide to mastering **shell scripting**. Shell scripting is a foundational skill for system administrators, OS developers, and power users. It enables automation, customization, and direct control over system behavior.

While many tutorials cover only the basics, this roadmap is designed to go far beyond elementary syntax. This guide assumes familiarity with the command line and basic Unix/Linux usage, but does not require prior scripting experience. Each section builds on the previous one, progressing from fundamental concepts to advanced techniques and real-world use cases.

## Key Topics

+ [Fundamentals of Shell Environments](#fundamentals-of-shell-environments)

---

## Fundamentals of Shell Environments

+ **What is a shell?**: Overview of POSIX shells, `sh`, `bash`, `dash`, `zsh`, and their roles in Unix systems.
+ **Interactive vs non-interactive shells.**
+ **Login vs non-login shells.**
+ **Shell startup files**: `.bashrc`, `.profile`, `.bash_profile`, `/etc/profile`, etc.
+ **Detecting your current shell**: `echo $0`, `ps -p $$`, `$SHELL`.
+ Understand how shell behavior changes based on invocation mode.

---

## Basic Syntax and Control Structures

+ **Variables**: Declaration, quoting (`"$var"` vs `$var`), scope (`local`, `export`).
+ **Positional parameters**: `$0`, `"$@"`, `$#`, `$?`, `$$`.
+ **Conditionals**:
    - `if`, `else`, `elif`, `fi`
    - `case`, `esac`
+ **Loops**:
    - `for`, `while`, `until`
    - `break`, `continue`

> Learn proper quoting early. Many security and logic bugs stem from misuse of quotes and whitespace handling.
