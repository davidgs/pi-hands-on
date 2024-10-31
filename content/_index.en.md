---
title: ""
type: "home"
reading_time: 2 minutes
---

## Welcome to the Workshop

### Overview

We will be building a Raspberry Pi (model 4B) from scratch, then using a [Zymkey](https://zymbit.com/zymkey) to secure it by encrypting the `rootfs` root filesystem. We will use [Bootware](https://zymbit.com/bootware) to create a new partition map.

We will also enable A/B partitioning to make updates resilient, reliable, and recoverable in the event of a bad update.

### Loading this tutorial

You are encouraged to load this tutorial on your laptop in order to make things easier (like the ability to copy/paste commands) by going to [https://davidgs.com/zymbit-workshop](https://davidgs.com/zymbit-workshop)

### Agenda

- An brief overview of what we'll cover in this workshop
- Some background on some of the terms, and solutions, that we'll be using
- Building hardware
- Installing software
- Configuring software
- Testing our configuration
- Send some secure messages

### Using the Command Line

Almost all of this workshop will entail interacting with your Raspberry Pi via the Command Line Interface (CLI). If you're not familiar with using the CLI, this will be an opportunity to learn about this powerful tool.

> [!TIP]
> **Windows users**
>
> The best solution for Windows is to make sure that you have WSL installed and active. This will give you access to a proper command line that you can use for `ssh`, etc. To install WSL, see the instructions provided [here](https://davidgs.com/zymbit-workshop/index.html)

> [!TIP]
> **Mac users**
>
> You already have a proper terminal application available. Simply go to your Applications folder, then to the Utilities folder, and look for Terminal.app. You can also install other terminal programs if you'd prefer.

> [!TIP]
> **Linux users**
>
> You have native terminals available with your Linux distribution. Look for 'Terminal' or 'XTerm' applications.

### Housekeeping

As we go through this workshop, you sill see some things called out in various ways. These are things I'll want you to pay special attention to.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

## Let's get started!

{{< pagenav next="chapter1/" >}}