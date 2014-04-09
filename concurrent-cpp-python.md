% Concurrent Programming in C++ & Python
% Zhang Yichao <zhang_yichao@vobile.cn>
% 3/10/2014

# Concurrent Programming

## Why?

- Multiple cores
- Massive incoming clients & requests on server
- Wait for server on client

## Challenges: Performance

- C10K/C10M
- Threads: startup cost & race conditions
- Python: GIL

## Our Answers

- `libevent` to handle concurrent clients
- `zmq` for sync interface upon an async implentation
- Thread/Process pools with locked queues and condition vars
- Background threads with locks and condition vars

## Challenges: Ease of use & Maintainability

- `libevent` is hard, as callbacks are hard (callback hell)
- `zmq` is too low level: typeless, and manual threading
- Thread pools are low level too; Process pool: process keepalive
- Locks and condition vars are hard to reason about: heisenbugs

# To Work with Concurrency

## Options out there

- Atomics & lock-free data structures
- Concurrency libraries
- Actor model
- Futures/promises
- Software transactional memory

## Atomics & Lockless Data Structures

- Fast and convenient: no locking, good for flags (quit, etc)
- May still be race conditions
- .. especially with more advanced tricks like CAS
- Lock-free but not wait-free: some threads may starve
- RCU: in hashidxd
- C++11's got atomics
- Boost's got lock free lib
- It's actually the easiest to understand and use
- Still have to work with threads and manage shared states

## Concurrency Libraries

- Thread Building Blocks
- OpenMP
- Easy to use
- Good performance
- Can you divide tasks into smaller pieces?

## Actor Model

- Popularized by Erlang
- Google made Go, which faces C/C++ based programmers
- C++ library: `libcppa`
- State/part of state managed by single thread, no race condition
- Good for data storage/access layer and decision makers
- Bad for fine grained data access: e.g. bank accounts processing

## Futures/Promises

- Callbacks turned inside out
    - Instead of executing function takes callbacks
    - .. the calculated value gets the callbacks
    - .. before it's calculated
- On client side, you can wait for all futures at once
- On server, consider coroutines (Boost's got it too)
- Without coroutines, still callback hell, but better (for indentation)
- With coroutines: understand coroutines first!

## Software Transactional Memory

- Is not popular in C/C++
- PyPy's got some o it, to cope with GIL, not mainstream, yet
- Treat local memory as a database, rollback and retry on race conditions

# Our Future

## Atomics for flags
- Is master?
- To quit?
- etc
- Don't use relaxed memory models!

## Actor model for data managers and decision makers:
- Feature additions/deletions
- Index worker multiplexing
- etc

## Futures/Promises for background jobs
- Let libraries handle thread pools
- Business logic only cares about data flows
- Wait on client instead of callbacks for maintainability
- Should be rare on servers, Python has better coroutine support

## Farther into Future

- Learn you some go for great good
- Seriously, go is easy to pick up
- Goroutines, spawn as much as you like, no need for pooling!
- Easier communication between threads
- If you're particularly brave: Rust
- Try Elixir to try out Erlang
