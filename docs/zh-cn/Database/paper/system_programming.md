# System programming

## What Every Programmer Should Know About Memory

[What Every Programmer Should Know About Memory](../../../_media/assets/Database/pdf/cpumemory.pdf) (2007)

This paper explains the structure of memory subsystems in use on modern commodity hardware, illustrating why CPU caches were developed, how they work, and what programs should do to achieve optimal performance by utilizing them.

## What Every Systems Programmer Should Know About Concurrency

[What Every Systems Programmer Should Know About Concurrency](../../../_media/assets/Database/pdf/concurrency.pdf) (2018)

Seasoned programmers are familiar with tools like mutexes, semaphores, and condition variables. But what makes them work? How do we write concurrent code when we can’t use them, like when we’re working below the operating system in an embedded environment, or when we can’t block due to hard time constraints? And since your system transforms your code into things you didn’t write, running in orders you never asked for, how do multithreaded programs work at all? Concurrency — especially on modern hardware — is a complicated and unintuitive topic, but let’s try to cover some fundamentals.

## Everything You Always Wanted to Know About Synchronization but Were Afraid to Ask

[Everything You Always Wanted to Know About Synchronization but Were Afraid to Ask](../../../_media/assets/Database/pdf/synchronization.pdf) (2013)

This paper presents the most exhaustive study of synchronization to date. 
We span multiple layers, from hardware cache-coherence protocols up to high-level concurrent software. 
We do so on different types of architectures, from single-socket – uniform and non-uniform – to multi-socket – directory and broadcast-based – many-cores.
