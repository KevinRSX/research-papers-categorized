# Category QUIC

## PQUIC

## Summary

- Category: system design
- Context: Papers related: QUIC and other extensible tools
- Correctness
  - Transport protocols allow users to propose the utilization of extensions during the handshake
  - A modern TCP stack supports a long list of TCP extensions
  - Measurements indicate that it remains difficult to deploy TCP extensions
  - Cause of slow deployment of TCP extensions
    1. Pupular stacks rarely implement TCP extensions unless approved by IETF
    2. TCP is part of OS, client and server are not upgraded at the same speed
    3. Middleboxes interfere with the deployment of new protocol extensions
  - QUIC prevents most of the interferences from middleboxes
  - TCP etc. are tuned by using configuration variables or socket options or eBPF code
- Contributions:
  - Consideration: transport protocols should provide a set of basic functions which can be tuned, combined and dynamically extended to support new use cases on a per-connection basis
  - System design:
    - Extensions to QUIC is broken down in a protocol plugin and can be dynamically attached to the existing implementation through protocol operations
    - Safe and scalable technique that enables the on-demand exchange of protocol plugins over QUIC connections
    - Prototype pquic which add to picoquic a virtual machine that allows executing the bytecode of protocol plugins in a platform independent manner while monitoring their behaviour
    - Several example plugins to demonstrate the benefits of PQUIC



## Local Plugin Insertion

A PQUIC implementation can be extended by dynamically loading one or more protocol plugins.

- A PQUIC implementation provides an API to protocol plugins. These functions are called *protocol operations*
- On-the-fly protocol plugin insertion
  - plugin = pluglet + manifest
- Isolation between connections and between plugins



### Pluglet Runtime Environment (PRE)

Requirements:

- Plugins should run regardless of the underlying hardware and OS
- Environment should keep each pluglet under control

Relaxed version of eBPF with additional verification:

- Check simple properties of bytecode
- Monitor the correct operation of pluglets by injecting specific instructions when their bytecode is JITed (use a register to check if memory is within the allowed bounds)



### Protocol Operations

