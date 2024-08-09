# 数据库论文

If you're a database core developer, studying database papers can offer several benefits:

* **Staying Updated**: 
You keep up with the latest advancements and research in the field of databases.

* **Improving Skills**: You can enhance your understanding of database systems and improve your technical skills.
* **Innovation**: You may get new ideas for features or optimizations in your own work.
* **Problem Solving**: You can learn how others have solved complex problems, which can help you in your own problem-solving process.
* **Networking**: Engaging with the academic community can help you build a professional network.
* **Research and Development**: It can provide a solid foundation for your own research and development efforts.
* **Technical Depth**: It deepens your technical knowledge, which is crucial for a core developer.
* **Career Growth**: It can contribute to your professional growth and make you more competitive in the job market.
* **Understanding Trends**: It helps you understand the trends and future directions of database technology.
* **Academic Insight**: It provides a deeper insight into the theoretical underpinnings of practical database systems.


## System programming

* What Every Programmer Should Know About Memory

[What Every Programmer Should Know About Memory](./pdf/cpumemory.pdf) (2007)

This paper explains the structure of memory subsystems in use on modern commodity hardware, illustrating why CPU caches were developed, how they work, and what programs should do to achieve optimal performance by utilizing them.

* What Every Systems Programmer Should Know About Concurrency

[What Every Systems Programmer Should Know About Concurrency](pdf/concurrency.pdf) (2018)

Seasoned programmers are familiar with tools like mutexes, semaphores, and condition variables. But what makes them work? How do we write concurrent code when we can’t use them, like when we’re working below the operating system in an embedded environment, or when we can’t block due to hard time constraints? And since your system transforms your code into things you didn’t write, running in orders you never asked for, how do multithreaded programs work at all? Concurrency — especially on modern hardware — is a complicated and unintuitive topic, but let’s try to cover some fundamentals.

* Everything You Always Wanted to Know About Synchronization but Were Afraid to Ask

[Everything You Always Wanted to Know About Synchronization but Were Afraid to Ask](pdf/synchronization.pdf) (2013)

This paper presents the most exhaustive study of synchronization to date. 
We span multiple layers, from hardware cache-coherence protocols up to high-level concurrent software. 
We do so on different types of architectures, from single-socket – uniform and non-uniform – to multi-socket – directory and broadcast-based – many-cores.
