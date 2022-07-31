# Questions and Answers

## Async
- Why files really are not the strong point of async IO?

The details differ from OS to OS, but on Linux it is because Tokio will use an API provided by the OS called epoll, which is basically a way to ask Linux "please wake me up when any of these sockets in the large list have an event", which is used to sleep on many sockets at once.

However epoll does not work with files. For this reason, Tokio will instead call the corresponding std file method in a separate thread outside the runtime, but this has an overhead compared to just calling the std file method directly.

## What Else