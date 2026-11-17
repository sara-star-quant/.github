# Sara Star Quant

Software that publishes its claims, evidence and limits.

We work on four kinds of system: encrypted network links, reproducible numerical computation, offline-first device software and verification gates for automated code changes. Each project states its scope, and what it does not do beside what it does.

## At a glance

- **Activity:** Quantum-Go, oxfuse, KioskOps and presence are actively developed. presence-mcp and filesigner-sdk-android are not.
- **Maintenance:** one person maintains every project today. presence documents its continuity plan in its `GOVERNANCE.md`.
- **Review:** Quantum-Go and KioskOps have had no independent security audit, and no project is certified. Our evidence is our own tests, models, vectors and fuzzing, linked below. Fixed Quantum-Go security advisories are [published](https://github.com/sara-star-quant/Quantum-Go/security/advisories).
- **Licenses:** Apache-2.0, except KioskOps, which is source-available under BSL 1.1. Production use is allowed unless you offer a commercial product or service that competes with it, and each release converts to Apache-2.0 no later than January 1, 2032 ([exact terms](https://github.com/sara-star-quant/KioskOps-SDK-Android-Enterprise/blob/main/LICENSE)).

## Post-quantum network links

**[Quantum-Go](https://github.com/sara-star-quant/Quantum-Go)** is pre-1.0. The construction that combines its two key exchanges has no computational security proof, and its replay window is tested, not proven.

*Outcome.* It is meant to let two systems exchange long-lived data so that, by design, breaking X25519 with a future quantum computer is not enough to decrypt what an attacker recorded today. A hybrid handshake (an attacker must break both ML-KEM-1024 and X25519), complete after one round trip, authenticates the peers by pre-shared key or pinned server key and encrypts the traffic. It does not provide anonymity or hide traffic patterns.

*Architecture.* A Go library with an ordered TCP transport and a UDP datagram transport, plus `quantum-tunnel forward`, which carries TCP between two plain endpoints with no code change. The plain hop to each endpoint stays unprotected, so keep it on loopback. Documented fits, not yet field-tested: database and broker links between sites, bulk-data replication between clusters, and legacy industrial links. It does not help services that already use TLS 1.3 with hybrid post-quantum key exchange.

Evidence: the [wire specification](https://github.com/sara-star-quant/Quantum-Go/blob/main/docs/PROTOCOL.md), [conformance vectors](https://github.com/sara-star-quant/Quantum-Go/blob/main/testdata/conformance/README.md) that an independent implementation can check byte for byte, [symbolic ProVerif models and their limits](https://github.com/sara-star-quant/Quantum-Go/blob/main/docs/formal/README.md), and a [threat model](https://github.com/sara-star-quant/Quantum-Go/blob/main/docs/technical/THREAT_MODEL.md). [Scope and open work](https://github.com/sara-star-quant/Quantum-Go/blob/main/docs/SCOPE.md) are listed beside them, and a [one-page guide](https://github.com/sara-star-quant/Quantum-Go/blob/main/docs/usage/PILOT.md) goes from download to a working tunnel. Independent review of how the combiner binds keys and ciphertexts is the contribution we would value most.

## Reproducible numerics

**[oxfuse](https://github.com/sara-star-quant/oxfuse)** is experimental: a small float32 operator set and a limited ONNX subset. It is not meant for large models, training or speed-critical work, and its README names mature alternatives.

*Outcome.* On the CPU, a small numerical graph returns the same output bits on every tested platform (Linux and Windows on arm64 and x86_64, macOS on arm64), so a result can be reproduced and checked bit for bit. NaN sign and payload are excluded as hardware behavior. The optional GPU backend is measured per adapter and not claimed to be bit-exact.

*Architecture.* A compiler in Rust for small numerical graphs: a graph, written in JSON or imported from ONNX, is type-checked, optimized by passes that never change a result bit, and run on a reference or a SIMD CPU backend. A hash over all 2^32 inputs of `Sigmoid` and `exp` is checked in CI on five platforms. It suits engineers who test runtimes, compilers or numerical libraries, keep exact regression outputs, or study reproducibility.

Evidence: a [claims table with reproduction commands](https://github.com/sara-star-quant/oxfuse/blob/main/docs/claims.md) and a statement of [who it is for and who it is not](https://github.com/sara-star-quant/oxfuse/blob/main/docs/use-cases.md).

## Mobile and connected devices

**[KioskOps SDK for Android](https://github.com/sara-star-quant/KioskOps-SDK-Android-Enterprise)** lets a managed device fleet keep recording through outages. Events are encrypted at rest, validated, screened for personal data and hash-chained, so a server that holds a stored chain head can detect alteration, rollback and truncation after it; truncation before a trust-on-first-use point is a documented limit ([secure design](https://github.com/sara-star-quant/KioskOps-SDK-Android-Enterprise/blob/main/docs/SECURE_DESIGN.md)). Its presets map controls to compliance frameworks and are not a certification. Android today; iOS is planned.

**[filesigner-sdk-android](https://github.com/sara-star-quant/filesigner-sdk-android)** signs files offline with ECDSA P-256 keys that never leave the device's secure hardware.

## Verification gates for code changes

**[presence](https://github.com/sara-star-quant/presence)** checks what a coding assistant says about its own changes. It keeps project context for Claude Code across sessions, records what happened to each change (reverts, amends, closed pull requests), and warns when "done" is claimed without a test or a build since the change; stricter presets block the commit instead. It runs locally and makes no outbound calls by default. Its [signed receipts](https://github.com/sara-star-quant/presence/blob/main/docs/receipts.md) are experimental. **[presence-mcp](https://github.com/sara-star-quant/presence-mcp)** exposes the same context to other tools.

## What you can check

- **Claims and status.** A statement in our documentation points to a test, a measurement or a document, or says that it does not. "Pre-1.0", "unaudited" and "experimental" mean exactly that, and no control is described as a certification.
- **Releases.** Quantum-Go, oxfuse and KioskOps publish checksums or signed build provenance. A [script](https://github.com/sara-star-quant/Quantum-Go/blob/main/scripts/release-verify.sh) verifies each Quantum-Go release.
- **Disclosure.** Security reports go through GitHub's private vulnerability reporting, enabled on every repository listed here.

Further repositories hold applied research notes and open analytical models on public data. [All repositories](https://github.com/sara-star-quant?tab=repositories).
