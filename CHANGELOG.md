# Changelog

All notable changes to Nyar are recorded here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html). Nyar has no release yet.

## [Unreleased]

### Changed

- Nyar fetches the Nodens release tag `v0.2.0`, up from the `dev` branch of Nodens, which no longer exists. It follows Nodens's renamed types: `VulkanBackend` and `IWindow::GetBackend()` replace `VulkanContext` and `GetGraphicsContext()`, and `FApplicationConfig` replaces `FApplicationSpecification`. Nyar no longer calls `find_package(glfw3)`, which clashed with the GLFW target that Nodens fetches.
- The Zed tasks and debug scenarios run in the dev container through the Dev Containers CLI (`@devcontainers/cli`), and the editor files `.zed/tasks.json`, `.zed/debug.json` and `.zed/settings.json` are tracked. The dev container mounts the project at its host path, so build outputs and debugger source paths match the host.
- Nyar follows the development cycle of Nodens: one trunk, `main`, where every change is squashed in through a pull request, a CI workflow that checks formatting and builds Debug and Release, and tag-driven releases. See `docs/BRANCHING.md` and `docs/COMMITTING.md`.
