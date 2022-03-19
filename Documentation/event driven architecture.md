Event Driven Architecture
==========================
- Libuv: strong focus on asynchronous I/O (input/output)
- implements 2 features:
    - the event loop: 
        - responsible for handling easy tasks like executing callbacks and network I/O
        - receives event such as new HTTP request, call their callback functions, and offload the most expensive task to the thread pool. 
    - the thread pool: deals with heavy work like file access or compression.


How NodeJs Works?
=================
- run in a single thread and a thread basically is just a sequence of instructions.