<div align="center">
  <h1 align="center">WarmShower OS Linux Kernels</h1>
  <p align="center">Enhanced Linux kernels with improved performance, multiple CPU schedulers, and advanced optimizations.<br/>Built on top of the upstream CachyOS kernel fork.</p>
</div>

## Table of Contents

- [Overview](#overview)
- [Kernel Variants & Schedulers](#kernel-variants--schedulers)
- [Features](#features)
  - [Performance Optimizations](#performance-optimizations)
  - [CPU Enhancements](#cpu-enhancements)
  - [Networking](#networking)
  - [Filesystem & Memory](#filesystem--memory)
  - [Additional Features](#additional-features)
- [WarmShower OS Repositories](#warmshower-os-repositories)
  - [Quick Installation](#quick-installation)
  - [Manual Installation](#manual-installation)
  - [CPU Compatibility Check](#cpu-compatibility-check)
  - [Repository Configuration](#repository-configuration)
- [SCX Schedulers](#scx-schedulers)
- [Other Distributions](#other-distributions)
- [Support & Community](#support--community)
- [Contributors](#contributors)

## Overview

WarmShower OS provides a collection of enhanced Linux kernels designed for improved performance across different workloads. Kernels are built from the [CachyOS Linux](https://github.com/CachyOS/linux) fork (currently based on **Linux 7.1**), which merges topic branches on top of upstream stable releases.

The kernels feature multiple CPU schedulers, advanced compiler optimizations, and carefully selected patches to deliver better responsiveness and throughput.

> [!NOTE]
> All WarmShower OS kernels are available in multiple architecture optimizations: `x86-64`, `x86-64-v3`, `x86-64-v4`, and `znver4`.

## Kernel Variants & Schedulers

Each scheduler is optimized for different use cases. We recommend testing each one to find the best fit for your specific requirements.

### Available Schedulers
| Scheduler | Full Name | Package(s) | Best for... | Developer |
| :--- | :--- | :--- | :--- | :--- |
| **[BORE](https://github.com/firelzrd/bore-scheduler)** | **B**urst-**O**riented **R**esponse **E**nhancer | `linux-warmshower-bore`, `linux-warmshower-deckify`, `linux-warmshower-hardened`, `linux-warmshower-rt-bore` | Interactive workloads & gaming | [firelzrd](https://github.com/firelzrd) |
| **[EEVDF](https://lwn.net/Articles/927530/)** | **E**arliest **E**ligible **V**irtual **D**eadline **F**irst | `linux-warmshower`, `linux-warmshower-eevdf`, `linux-warmshower-lts`, `linux-warmshower-server` | General-purpose computing | Peter Zijlstra |
| **[BMQ](https://gitlab.com/alfredchen/linux-prjc)** | **B**it**M**ap **Q**ueue CPU Scheduler | `linux-warmshower-bmq` | Throughput-oriented workloads | [Alfred Chen](https://gitlab.com/alfredchen) |

### Specialized Variants

- **`linux-warmshower`** - Default kernel: EEVDF scheduler with Clang Thin LTO and AutoFDO
- **`linux-warmshower-rc`** - Release candidate kernel tracking the next upstream version
- **`linux-warmshower-hardened`** - Security-focused kernel with hardening patches and the BORE scheduler
- **`linux-warmshower-lts`** - Long Term Support version (currently 6.18) with EEVDF and Cachy Sauce
- **`linux-warmshower-rt-bore`** - Real-time kernel with BORE scheduler integration
- **`linux-warmshower-server`** - Server-optimized EEVDF configuration with lazy preemption
- **`linux-warmshower-deckify`** - Handheld gaming variant with BORE scheduler, Steam Deck support, and MSI Claw driver

### Compiler Variants

- **`linux-warmshower`** - Default optimized build using Clang Thin LTO with AutoFDO + Propeller profile-guided optimizations
- **Scheduler-specific packages** (`linux-warmshower-bore`, `linux-warmshower-bmq`, `linux-warmshower-eevdf`, etc.) - GCC-compiled builds without LTO, focused on scheduler choice over compiler optimizations

## Features

The WarmShower OS kernel fork merges topic branches into each release. The current **Linux 7.1** base integrates fifteen topic branches on top of upstream stable.

### Performance Optimizations

- **Advanced Compilation**: Highly customizable PKGBUILD with support for both GCC and Clang compilers
- **Link Time Optimization (LTO)**: Clang Thin LTO (v12) enabled by default on the main `linux-warmshower` package
- **Distributed ThinLTO**: Support for distributed Clang ThinLTO builds to speed up kernel compilation
- **Profile-Guided Optimization**: AutoFDO + Propeller profiling on the default kernel for optimal code generation
- **Kernel Control Flow Integrity (kCFI)**: Available when using LLVM for enhanced security
- **Timer Frequency Options**: Configurable between 100Hz, 250Hz, 300Hz, 500Hz, 600Hz, 750Hz, and 1000Hz (default: 1000Hz)
- **Architecture Optimizations**: Support for x86-64-v3, x86-64-v4, and AMD Zen4 specific builds
- **Compiler Optimizations**: Advanced GCC flags including `-fivopts` and `-fmodulo-sched`
- **PREEMPT_DYNAMIC**: Runtime-selectable preemption modes (full, lazy, voluntary, none)
- **vmalloc Optimizations**: Bulk page freeing in `vfree()`, improved `vrealloc()` shrink/grow paths, and optimized contiguous range freeing

### CPU Enhancements

- **Multiple Schedulers**: BORE, EEVDF, and BMQ schedulers for different workload optimization
- **[POC Selector](https://github.com/masahitoS/scx_cake) v2.6.2**: Piece-Of-Cake fast idle CPU selector inspired by scx_cake, reducing wakeup latency
- **Cluster-Aware Scheduling**: Improved load balancing for CPU clusters
- **Preempt-IPI Optimizations**: Lower-latency SMP/IPI paths
- **WarmShower OS Sauce**: Custom `CONFIG_CACHY` configuration with scheduler and system tweaks
- **AMD P-State Enhancements**: Preferred Core support and latest amd-pstate improvements from linux-next
- **Real-Time Support**: RT kernel builds available with BORE scheduler integration
- **Low-Latency Optimizations**: Patches for improved responsiveness and reduced jitter

### Networking

- **[BBR3 TCP](https://github.com/google/bbr)**: BBRv3 congestion control available as a separate module alongside BBRv1

### Filesystem & Memory

- **ZFS Support**: Built-in ZFS filesystem support with pre-compiled modules
- **NTFS Improvements**: Upstream NTFS driver fixes
- **MGLRU Enhancements**: Unified writeback reclaim statistics and improved dirty writeback reactivation
- **Transparent Hugepages**: `ADVISE` mode for shmem/tmpfs transparent hugepages
- **NVIDIA Integration**: Proprietary and open-source NVIDIA driver support
- **I/O Scheduler Improvements**: Enhanced BFQ and mq-deadline performance; [ADIOS](https://github.com/firelzrd/adios) v3.2.0 multi-queue I/O scheduler support
- **VRAM Cgroup (DMEM)**: Device memory controller for restricting GPU VRAM usage per cgroup

### Additional Features

#### Hardware Support
- **AMD ISP4**: AMD ISP 4 camera driver for supported platforms
- **Gaming Hardware**: Steam Deck patches, ROG Ally support, MSI Claw HID driver, EasySMX 05 Pro gamepad support
- **Apple Hardware**: T2 MacBook support with apple-bce driver in staging
- **ASUS Hardware**: Extended ASUS hardware compatibility patches
- **Lenovo Hardware**: WMI battery charge limiting, GPU/CPU tunable attributes
- **HP Hardware**: OMEN Slim and OMEN MAX laptop support
- **Samsung Hardware**: Galaxy Book 4/5 series audio fixes
- **Graphics**: HDR support enabled, AMDGPU min_powercap override, DRM fair GPU scheduling
- **Display**: HDMI VRR on AMD, ALLM and passive VRR connector properties

#### System Enhancements
- **Multimedia**: v4l2loopback modules included by default
- **Virtualization**: ACS Override support for VFIO/GPU passthrough
- **Upstream Integration**: Cherry-picked patches from Clear Linux and linux-next

## WarmShower OS Repositories

Our repositories contain optimized Arch Linux and WarmShower OS packages rebuilt with performance, stability, and security enhancements.

### Available Repository Tiers

| Repository | Target CPUs | Package Coverage | Optimization Level |
|------------|-------------|------------------|--------------------|
| `znver4` | AMD Zen 4 & Zen 5 | Full package set | Zen4-specific optimizations |
| `x86-64-v4` | Intel Xeon / Intel 11th+ | Full package set | AVX, AVX2, AVX512, SSE4.2, SSSE3 |
| `x86-64-v3` | 2012 or newer | Full package set | AVX, AVX2, SSE4.2, SSSE3 |
| `x86-64` | All x86-64 CPUs | Kernels only | Basic optimizations |

### Quick Installation

**Automated Setup (Recommended)**

Our installation script automatically detects your CPU and configures the optimal repositories:

```bash
# Download and extract the installer
curl -O https://repo.warmshower.ai/warmshower-repo.tar.xz
tar xvf warmshower-repo.tar.xz && cd warmshower-repo

# Run the automated installer
sudo ./warmshower-repo.sh
```

**What the script does:**
- Auto-detects your CPU architecture support
- Configures appropriate repository tiers
- Backs up your existing `pacman.conf`
- Sets up GPG keys and mirrors

### Manual Installation

**Step 1: Add WarmShower OS Signing Keys**

```bash
# Import and locally sign the WarmShower OS GPG key.
# Replace <FINGERPRINT> with the fingerprint from https://warmshower.ai/signing-key
sudo pacman-key --recv-keys <FINGERPRINT> --keyserver keyserver.ubuntu.com
sudo pacman-key --lsign-key <FINGERPRINT>
```

> [!NOTE]
> The signing key fingerprint will be published at warmshower.ai once the signing infrastructure (WS-001/WS-002) is complete.

**Step 2: Install Required Packages**

```bash
sudo pacman -U \
    'https://repo.warmshower.ai/x86_64/warmshower-keyring-<ver>-any.pkg.tar.zst' \
    'https://repo.warmshower.ai/x86_64/warmshower-mirrorlist-<ver>-any.pkg.tar.zst'
```

**Step 3: Update your system**

```bash
sudo pacman -Syu
```

### CPU Compatibility Check

```bash
/lib/ld-linux-x86-64.so.2 --help | grep supported
```

- ✅ **`x86-64-v4 (supported, searched)`** = CPU supports v4
- ❌ **`x86-64-v4`** (no parentheses) = CPU does NOT support v4

### Repository Configuration

Edit `/etc/pacman.conf` to add the appropriate repositories:

**For x86-64 (Basic) Support:**
```ini
[warmshower]
Include = /etc/pacman.d/warmshower-mirrorlist
```

**For x86-64-v3 Support:**
```ini
[warmshower-v3]
Include = /etc/pacman.d/warmshower-v3-mirrorlist
[warmshower-core-v3]
Include = /etc/pacman.d/warmshower-v3-mirrorlist
[warmshower-extra-v3]
Include = /etc/pacman.d/warmshower-v3-mirrorlist
[warmshower]
Include = /etc/pacman.d/warmshower-mirrorlist
```

**For x86-64-v4 Support:**
```ini
[warmshower-v4]
Include = /etc/pacman.d/warmshower-v4-mirrorlist
[warmshower-core-v4]
Include = /etc/pacman.d/warmshower-v4-mirrorlist
[warmshower-extra-v4]
Include = /etc/pacman.d/warmshower-v4-mirrorlist
[warmshower]
Include = /etc/pacman.d/warmshower-mirrorlist
```

**For AMD Zen4 CPUs:**
```ini
[warmshower-znver4]
Include = /etc/pacman.d/warmshower-v4-mirrorlist
[warmshower-core-znver4]
Include = /etc/pacman.d/warmshower-v4-mirrorlist
[warmshower-extra-znver4]
Include = /etc/pacman.d/warmshower-v4-mirrorlist
[warmshower]
Include = /etc/pacman.d/warmshower-mirrorlist
```

### Debug Packages

Debug symbols are available through debuginfod. Configure it by creating:

```bash
sudo mkdir -p /etc/debuginfod
echo "https://debuginfod.warmshower.ai" | sudo tee /etc/debuginfod/warmshower.urls
```

> [!NOTE]
> The WarmShower debuginfod service will be available at `debuginfod.warmshower.ai` once provisioned. Until then, the `warmshower-settings` package falls back to the Arch Linux public debuginfod server.

## SCX Schedulers

WarmShower OS supports the sched-ext (SCX) framework for userspace schedulers. For setup and configuration, see the [sched-ext documentation](https://github.com/sched-ext/scx).

> [!WARNING]
> SCX schedulers are **not compatible** with the `linux-warmshower-bmq` kernel variant.

## Other Distributions

WarmShower OS kernels are focused on Arch Linux and WarmShower OS. For community ports to other distributions, check the upstream CachyOS kernel repositories.

## Support & Community

- **GitHub Issues:** [github.com/Warm-shower/linux-warmshower/issues](https://github.com/Warm-shower/linux-warmshower/issues)
- **Website:** [warmshower.ai](https://warmshower.ai)
- **Forum:** Coming soon at warmshower.ai/forum

## Contributors

Special thanks to our key contributors:

- **[firelzrd](https://github.com/firelzrd/bore-scheduler)** - BORE Scheduler developer
- **[Arch Linux](https://archlinux.org)** - Foundation distribution
- **[CachyOS](https://github.com/CachyOS)** - Upstream kernel fork we build upon
- **[Linux Kernel Community](https://github.com/torvalds/linux)** - Upstream kernel development
- **All WarmShower OS contributors** - see [github.com/Warm-shower](https://github.com/Warm-shower)
