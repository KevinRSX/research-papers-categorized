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

Break down the protocol execution flow into **generic** subroutines. These specified procedures are called protocol operations.

Parameters are separated out.

Protocol operations are split into three anchors, each of which is a insertion point for a pluglet:

- replace: points to the actual implementation, enables a pluglet to override default behavior
- pre: attaches the plugin just before the protocol operation invocation
- post: just after



### Attaching Protocol Plugins

- A plugin consists of a combination of several pluglets
- Plugins require an interface with which they can operate on their connection

Pluglets are attahced to PREs. PREs' heap points to an area common to all pluglets of a plugin.

- PQUIC exposes some functions to PRE



## Exchanging Plugins

TLS 1.3 prevents middleboxes from modifying data exchanged over QUIC connections. Protocol plugins could therefore be exchanged over an existing QUIC connection.

The design enables independent developers to publish their own plugins for which the PQUIC peers' trust in their validity is established by independent plugin validators (PV).

- Directly offer to plugin developers an efficient mean that is logarithmic in the number of plugins published
- Make PQUIC peers able to formulate their safety requirements by combining the PVs they trust