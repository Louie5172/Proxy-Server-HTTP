# HTTP Web Proxy with Caching, Concurrency, and Persistence

## 1. Overview

In this project, you will design and implement a **forward HTTP/1.1 web proxy**.  
The proxy sits between HTTP clients (e.g. browsers, `curl`) and origin servers, forwarding requests and responses while providing additional functionality such as:

- Concurrent handling of multiple clients
- Support for standard HTTP methods
- An LRU-based caching system
- Persistent (disk-backed) cache storage
- Transparent HTTPS tunneling via CONNECT

The final system should behave as a **standards-compliant, high-performance HTTP proxy** suitable for real-world workloads.

---

## 2. Functional Requirements

### 2.1 Supported HTTP Methods

The proxy MUST correctly support the following HTTP methods:

| Method  | Description |
|-------|-------------|
| GET    | Fetch a resource (cacheable) |
| HEAD   | Fetch headers only (cacheable) |
| POST   | Forward request with body (not cached) |
| CONNECT | Establish a TCP tunnel for HTTPS |

---

### 2.2 HTTP Protocol Support

- HTTP/1.1 only
- Absolute-form requests from clients MUST be translated to origin-form requests for upstream servers
- Persistent connections (`Connection: keep-alive`) SHOULD be supported
- Hop-by-hop headers MUST be stripped when forwarding requests

---

### 2.3 Concurrency

The proxy MUST handle multiple simultaneous client connections.

Acceptable concurrency models include:
- Thread-per-connection
- Thread pool
- Event-driven / async I/O

The implementation MUST ensure:
- No data races in shared structures (e.g. cache)
- Graceful handling of slow or stalled clients

---

### 2.4 HTTPS Tunneling (CONNECT)

For `CONNECT host:port HTTP/1.1` requests:

1. The proxy MUST establish a TCP connection to `host:port`
2. Respond with:
