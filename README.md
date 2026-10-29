<h1 align="center">Nyar</h1>

<h3 align="center">An experimental Vulkan real-time renderer for the Nodens framework.</h3>

<p align="center">
  <a href="https://github.com/eldritch-codex/nyar/actions/workflows/ci.yml"><img src="https://img.shields.io/github/actions/workflow/status/eldritch-codex/nyar/ci.yml?branch=main&amp;style=flat-square&amp;label=CI" alt="CI status"></a>
  <img src="https://img.shields.io/badge/Nodens-0.2.0-blue?style=flat-square" alt="Nodens 0.2.0">
  <img src="https://img.shields.io/badge/C%2B%2B-23-blue?style=flat-square" alt="C++23">
  <img src="https://img.shields.io/badge/CMake-3.30-blue?style=flat-square" alt="CMake 3.30">
  <img src="https://img.shields.io/badge/platform-Linux-blue?style=flat-square" alt="Platform: Linux">
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-yellow?style=flat-square" alt="License: MIT"></a>
</p>

<p align="center">
  <a href="https://github.com/eldritch-codex/nyar/milestone/1"><img src="https://img.shields.io/github/milestones/progress/eldritch-codex/nyar/1?style=flat-square" alt="Milestone 1 progress"></a>
  <a href="https://github.com/eldritch-codex/nyar/milestone/2"><img src="https://img.shields.io/github/milestones/progress/eldritch-codex/nyar/2?style=flat-square" alt="Milestone 2 progress"></a>
  <a href="https://github.com/eldritch-codex/nyar/milestone/3"><img src="https://img.shields.io/github/milestones/progress/eldritch-codex/nyar/3?style=flat-square" alt="Milestone 3 progress"></a>
</p>

<p align="center">
  <a href="https://github.com/eldritch-codex/nodens/blob/main/docs/guide/getting-started.md">Getting Started</a> ·
  <a href="docs/BRANCHING.md">Branching</a> ·
  <a href="https://github.com/eldritch-codex/nodens/blob/main/docs/guide/README.md">Nodens Guide</a> ·
  <a href="https://github.com/eldritch-codex/nodens">Nodens</a>
</p>

> **Status:** Nyar is in active, experimental development with no stable release. It is a learning ground for real-time rendering with Vulkan and modern C++20/23, and its APIs change freely.
>
> **Limitations:** Linux is the only validated platform, through the published development image.

## Introduction

Nyar is designed to work as a separate module for the [Nodens](https://github.com/eldritch-codex/nodens) framework. Its design and implementation draw from established principles in real-time rendering and modern engine architecture and tools (for example Slang shaders and dynamic rendering).

- **Immediate goal:** follow and complete the official [Khronos Vulkan "Drawing a triangle" tutorial](https://docs.vulkan.org/tutorial/latest/03_Drawing_a_triangle/00_Setup/00_Base_code.html) to establish a solid baseline.
- **Future direction:** incrementally refactor and morph the working tutorial code into a dedicated renderer API.

## Nodens version

Nyar fetches Nodens with `FetchContent`, pinned to a release tag by `GIT_TAG` in `CMakeLists.txt` (currently `v0.2.0`). To move to a newer Nodens, change the tag and rebuild. While Nodens is at 0.x, a release that changes the API bumps the minor version, so expect to port the renamed types.

## Development cycle

Nyar has one trunk, `main`. A change is a `feature/<slug>` branch, cut from `main` and squashed back through a pull request once CI passes; a release is an annotated `v*` tag on `main`. This is the cycle of Nodens. See [Branching and releases](docs/BRANCHING.md) and [Committing](docs/COMMITTING.md), and the [changelog](CHANGELOG.md) for what changed.

## Development environment

Nyar uses EldritchCodex's shared Linux graphics image through `.devcontainer/devcontainer.json`:

```text
ghcr.io/eldritch-codex/arch-dev:main
```

Open Nyar in Zed or Visual Studio Code, then reopen it in the Dev Container. NVIDIA hosts need a working host driver and NVIDIA Container Toolkit. Intel/AMD hosts use the `/dev/dri` configuration. See the [Nodens toolchain guide](https://github.com/eldritch-codex/nodens/blob/main/docs/guide/building-and-toolchain.md) for GPU setup details.

To keep Zed on the host, install the [Dev Containers CLI](https://github.com/devcontainers/cli) (`npm install -g @devcontainers/cli`). The `.zed` tasks and the debugger run in the container through it; start with the **Docker: Start** task. The tasks use `.devcontainer/devcontainer.json`, which mounts the project at its host path. See [Zed outside the container](https://github.com/eldritch-codex/nodens/blob/main/docs/guide/building-and-toolchain.md#zed-outside-the-container).

## Acknowledgements

In September 2026 I started experimenting with LLM coding agents (Claude Code, ZedAI and Pi) and even decided to develop my own: [Armitage](https://eldritchcodex.com/armitage/). I use them mostly to learn new stuff, but also they help with documentation, CI/CD and general project upkeep. Commits carry no co-author or AI attribution trailers, since I am responsible for every commit.

## Suggested reading
- Akenine-Möller, T., Haines, E., Hoffman, N., Pesce, A., Iwanicki, M., & Hillaire, S. (2019). **Real-time rendering** (Fourth edition). CRC Press.
- Castorina, M., & Sassone, G. (2023). **Mastering graphics programming with vulkan: Develop a modern rendering engine from first principles to state-of-the-art techniques**. Packt Publishing.
- Eisemann, E. (Ed.). (2012). **Real-time shadows**. CRC Press.
- Gregory, J. (2019). **Game engine architecture** (Third edition). CRC Press, Taylor & Francis Group.
- Lengyel, E. (2019). **Foundations of game engine development. Volume 2: Rendering**. Terathon Software LLC.
- Marrs, A., Shirley, P., & Wald, I. (Eds.). (2021). **Ray Tracing Gems II: Next Generation Real-Time Rendering with DXR, Vulkan, and OptiX**. Apress. https://doi.org/10.1007/978-1-4842-7185-8
- Pharr, M., Jakob, W., & Humphreys, G. (2017). **Physically based rendering: From theory to implementation** (Third edition). Morgan Kaufmann Publishers/Elsevier. https://www.pbr-book.org/
- **Vulkan Documentation: Vulkan Documentation Project**. (n.d.). Retrieved August 31, 2026, from https://docs.vulkan.org/spec/latest/index.html
- Woo, A. P., Pierre. (2019). **Shadow Algorithms Data Miner**. CRC PRESS.

## License

Copyright (c) 2026 EldritchCodex. The project and its entire history are licensed under the MIT License.
