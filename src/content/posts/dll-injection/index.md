---
title: DLL Injection
published: 2026-07-09
description: ""
tags: []
draft: true
category: ""
image: ""
updated: 2026-07-09
---
Of course. Let's break down the core concepts of DLL Injection as covered in this section.

The central idea is manipulating how a program uses Dynamic Link Libraries (DLLs) to execute your own code within the context of a legitimate, and often trusted, process. This is useful for both privilege escalation and evading security software.

Here are the main concepts covered:

### 1. DLL Injection

This is the primary technique, where you force a running process to load and execute code from a malicious DLL. The code then runs with the same permissions and memory access as the target process.

The section highlights three methods for this:

- **`LoadLibrary` Injection**: This is the most common method. It uses the standard Windows `LoadLibrary` function to load a DLL. The attack involves getting a handle to the target process, allocating memory within it, writing the path to your malicious DLL into that memory, and then creating a remote thread that calls `LoadLibrary` with that path.
- **Manual Mapping**: A much stealthier and more complex technique. Instead of using `LoadLibrary` (which security tools monitor), the attacker manually copies the DLL into the target process's memory and handles all the necessary steps to make it run, such as resolving its functions.
- **Reflective DLL Injection**: An advanced technique where the malicious DLL contains its own loader. This allows the DLL to map itself into memory from any location, minimizing its interaction with the operating system and making it very difficult to detect.

### 2. DLL Hijacking

This is a related but distinct technique. Instead of forcing a DLL into a running process, you take advantage of how Windows searches for DLLs.

- **Search Order**: When an application tries to load a DLL without specifying its full path, Windows searches for it in a specific sequence of directories (e.g., the application's directory, the system directory, directories in the PATH variable).
- **The Attack**: If you can place a malicious DLL with the same name as a legitimate but missing DLL (or a DLL in a less-trusted location), you can trick the application into loading your malicious code. The section demonstrates this by using `procmon` to find a DLL that an application fails to find (`NAME NOT FOUND`) and then creating a malicious DLL with that name.

In short, **DLL Injection** _forces_ a DLL into a process, while **DLL Hijacking** _tricks_ a process into loading the wrong DLL. Both can lead to arbitrary code execution.