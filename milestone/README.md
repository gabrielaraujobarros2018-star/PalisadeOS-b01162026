# PalisadeOS — b01152026 & b01162026

This repository documents two closely linked milestone builds in the PalisadeOS project.  
Together, they define the point where PalisadeOS transitions from a boot-capable system into a resilient, survivable operating system architecture.

---

## Build b01152026 — Foundational Release

**b01152026** is the most significant release in PalisadeOS history to date.

This build established PalisadeOS as a *real, bootable operating system* rather than a conceptual or experimental kernel. It introduced a unified, firmware-aware boot architecture and removed hardcoded assumptions at the lowest system level.

### Key Characteristics
- Full custom bootloader
- BIOS and UEFI support
- x86_64 and ARM64 readiness
- Deterministic kernel handoff
- Architecture-neutral boot configuration
- Verified kernel loading paths

This release permanently defined how PalisadeOS boots, hands off control, and initializes safely across different hardware environments.

---

## Build b01162026 — Survivability & Hardening

**b01162026** is the direct successor to b01152026 and represents the post-foundation hardening phase.

This build did not aim to add user-facing features. Instead, it focused on making the system *difficult to corrupt, difficult to brick, and difficult to misuse*.

### Key Characteristics
- Persistent user data isolation system
- System-above-system repair and patch authority
- Background process authority layer
- Integrity verification and checksum infrastructure
- Toolchain and header stabilization
- Failure-resilient system design

Where b01152026 made PalisadeOS real, b01162026 made it endure.

---

## Design Philosophy Shared by Both Builds

- Survivability over convenience
- Determinism over implicit behavior
- System authority over userland override
- Data preservation over system recovery shortcuts
- Clear separation of responsibility between boot, kernel, repair, and user space

---

## Project Status

PalisadeOS is not a hobby project or a prototype.  
It is designed for real hardware, real users, and daily-use scenarios, including flashing on desktops and Android devices.

These two builds together form the architectural baseline upon which all future PalisadeOS development is built.

---

# PalisadeOS Changelog
## Release Line: Post-b01152026 Development Phase
## Classification: Infrastructure, Survivability, System Authority

---

### Overview

This changelog documents the internal system evolution of PalisadeOS after the **b01152026** release.
This phase focuses on boot reliability, cross-platform compatibility, system resilience, and long-term
operability under hostile or failure-prone conditions.

This is not a feature-oriented phase.
This is a **system hardening phase**.

---

## [POST-b01152026] — Core System Evolution

---

### Bootloader Architecture

- Introduced a unified bootloader root under `/BootL_POS/bootloader`
- Bootloader now supports both legacy BIOS and modern UEFI paths
- Removed architecture-specific boot assumptions
- Enabled future ARM64 and x86_64 parity at boot time
- Established a deterministic boot flow regardless of firmware type

---

### BIOS Boot Path

- Implemented `stage1.asm` with real disk I/O logic
- Added CHS/LBA fallback handling for legacy disks
- Ensured safe relocation of stage2 into higher memory
- Implemented strict register sanitization before handoff
- Added signature verification hooks for later expansion

---

### BIOS Stage 2 Loader

- Implemented protected mode transition
- Added GDT initialization
- Introduced early memory probing
- Implemented kernel loading from disk sectors
- Added error halt routines for unrecoverable states
- Enabled debug output via BIOS interrupts

---

### BIOS Disk Interface

- Implemented `disk.asm` with raw INT 13h handling
- Added retry logic for disk read failures
- Implemented disk geometry detection
- Removed reliance on firmware defaults
- Ensured deterministic sector reads

---

### UEFI Boot Path

- Added native UEFI entrypoints for x64 and AA64
- Implemented `BOOTX64.EFI` and `BOOTAA64.EFI`
- Established clean separation from BIOS logic
- Enabled UEFI-compliant memory allocation
- Removed dependency on firmware framebuffer assumptions

---

### UEFI Framebuffer Initialization

- Implemented native GOP framebuffer detection
- Added resolution negotiation logic
- Enabled linear framebuffer mapping
- Removed fixed-resolution assumptions
- Added fallback handling for unsupported modes

---

### UEFI Memory Map Handling

- Implemented full memory map extraction
- Normalized memory descriptors across firmware vendors
- Removed unsafe memory region usage
- Ensured kernel receives authoritative memory state
- Enabled future NUMA awareness

---

### ELF Kernel Loading

- Implemented real ELF parsing logic
- Added segment validation checks
- Ensured alignment correctness
- Implemented relocation handling
- Prevented malformed kernel images from loading

---

### Kernel Verification

- Added kernel integrity validation hooks
- Implemented checksum verification logic
- Prepared infrastructure for signature enforcement
- Added fail-fast behavior on verification errors
- Prevented silent corruption boot scenarios

---

### Multiboot Compatibility

- Added Multiboot-compliant header
- Implemented Multiboot loader path
- Enabled compatibility with external tooling
- Preserved native boot path priority
- Avoided Multiboot lock-in

---

### Cross-Device Boot Configuration

- Introduced `boot.cfg` as a shared authority
- Implemented real parsing logic
- Enabled architecture-neutral configuration
- Removed compile-time boot flags
- Enabled runtime boot behavior selection

---

### Boot Configuration Capabilities

- Kernel path selection
- Video mode selection
- Debug flag toggling
- Safe-mode boot paths
- Fallback boot logic preparation

---

### Common Boot Code Layer

- Added shared config parser
- Added shared memory map normalization
- Added shared video detection logic
- Removed code duplication across boot paths
- Simplified future maintenance

---

### Linker Scripts

- Implemented `bios.ld`
- Implemented `uefi.ld`
- Ensured correct section placement
- Removed linker ambiguity
- Enabled deterministic binary layout

---

### System Handoff Interface

- Defined clean kernel handoff structures
- Implemented handoff headers
- Ensured ABI stability
- Removed undefined boot-time memory contracts
- Prepared kernel for multi-boot source inputs

---

### Persistent User Data System

- Introduced a system-independent user data keeper
- Guaranteed survival across:
  - Reboots
  - Updates
  - Repairs
  - Reflashes
- Enforced strict system access denial
- Enabled user-only data control

---

### User Data Isolation Model

- Physical directory separation
- Logical access barriers
- No system write permissions
- No automated cleanup routines
- No implicit migrations

---

### Data Survivability Guarantees

- System updates cannot modify user data
- Repair modules cannot overwrite user data
- Bootloader does not mount user data
- Kernel mounts are explicit and user-driven
- Failure scenarios preserve user ownership

---

### System Repair Module

- Introduced Patch Automation Core
- Designed as system-above-system authority
- Operates outside standard security boundaries
- Cannot be disabled by userland
- Cannot be overridden by apps

---

### Repair Capabilities

- Integrity verification
- Patch enforcement
- Recovery execution
- State reconciliation
- Controlled rollback support

---

### Repair Authority Model

- System security defers to repair core
- Kernel policies cannot block repair
- User permissions do not override repair
- Repair logic executes with absolute priority
- Failure of repair triggers escalation logic

---

### Background Process Authority

- Introduced `/procManager`
- Separated from application process control
- Designed for non-killable system tasks
- Used by repair system
- Prepared for future watchdog integration

---

### Process Control Model

- Background processes registered centrally
- Processes validated before execution
- Unauthorized termination prevented
- Priority enforced at kernel boundary
- Logging hooks prepared

---

### Integrity Infrastructure

- Added MD5 checksum generation
- Added SHA-256 checksum generation
- Added SHA-512 checksum generation
- Enabled multi-layer verification
- Prepared for reproducible builds

---

### Build Verification

- Checksums validate bootloader binaries
- Checksums validate kernel images
- Checksums validate system images
- Checksums enable tamper detection
- Checksums support forensic analysis

---

### Header Completion

- Added missing boot headers
- Added missing handoff definitions
- Added missing video structures
- Prevented ABI mismatches
- Prevented compile-time instability

---

### Toolchain Stability

- Reduced undefined behavior
- Removed implicit structure assumptions
- Stabilized cross-compiler outputs
- Reduced silent build failures
- Improved diagnostic clarity

---

### Documentation Updates

- Added reality-based README
- Explicitly stated non-hobby status
- Documented Android and desktop targets
- Declared daily-use readiness
- Clarified flashing expectations

---

### Licensing Clarifications

- Added LICENSE.md
- Declared asset copyright
- Separated code and asset licensing
- Prevented asset misuse
- Established legal clarity

---

### Project Positioning

- OS is production-oriented
- OS is not experimental
- OS is not a toy kernel
- OS is designed for real devices
- OS assumes real user data

---

### Architectural Direction

- Prioritized survivability over features
- Prioritized correctness over speed
- Prioritized authority separation
- Prioritized recovery over aesthetics
- Prioritized determinism over convenience

---

### Failure Handling Philosophy

- Fail fast at boot
- Fail loud during verification
- Preserve user data at all costs
- Recover system automatically
- Never silently corrupt state

---

### Security Direction (Preparatory)

- Secure Boot hooks defined
- Signature enforcement planned
- DTB support planned
- Panic recovery planned
- Boot metrics planned

---

### Developer Impact

- Reduced undefined kernel states
- Reduced debugging ambiguity
- Improved reproducibility
- Improved long-term maintainability
- Improved confidence in boot flow

---

### End of Post-b01152026 Phase

This phase established PalisadeOS as a **resilient system platform**.
Future phases will build features on top of a foundation that is
already difficult to corrupt, difficult to brick, and difficult to misuse.

---
