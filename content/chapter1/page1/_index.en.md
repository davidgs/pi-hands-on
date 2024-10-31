---
title: 'Background Info'
date: 2024-10-24T12:23:03-04:00
weight: 1
reading_time: 2 minutes
---

## Why are we doing this?

### SD Card vulernability

While that micro SD card boot mechanism is certainly convenient, it does leave the Pi extremely vulnerable to physical tampering. After all, someone can simply walk up to the Pi, remove the SD card, and they have access to all of the programs and data that was running. They can put that card into their own Pi and they have full access to everything. Well, with a little password hacking, etc.

Making that Pi absolutely secure against physical tampering as well as electronic tampering is a critical step in making a Raspberry Pi a secure device for deploying applications in the field.

### Updates and recoverability

Seamless updates of your Pi is also, often, a hassle. Especially if you have more than a handful of them. You have to login to each one, run the updates, and then hope that nothing goes wrong.

Which leads me to recoverability. What happens if one of those updates fails for some reason? especially if it’s in some remote location. How do you ensure that device is recoverable, and how can you get it back online as quickly as possible?

{{< pagenav prev="chapter1/" next="../page2/" >}}