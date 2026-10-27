<!-- covers: CHANGELOG.md, docs/BRANCHING.md -->
# Committing

Nyar uses the commit conventions of [Nodens](https://github.com/eldritch-codex/nodens/blob/main/docs/COMMITTING.md): `type(scope): what the change does`, one thing per commit, explicit paths when staging, and a `!` before the colon for a breaking change. That page has the types and the reasoning. [Branching and releases](BRANCHING.md) covers where a change lives.

## Scopes

Name the area touched, and leave the scope out when the change spans the whole project.

| Scope | Area |
|---|---|
| `vulkan` | the Vulkan code in `src/` and `examples/` |
| `shaders` | the Slang shaders in `shaders/` |
| `cmake` | `CMakeLists.txt` and the build helpers |
| `devcontainer` | `.devcontainer/` |
| `zed` | the editor tasks in `.zed/` |
| `docs` | this folder and the README |

## Pull request titles

Pull requests are squash-merged, so the PR title becomes the commit on `main`. Write it in the commit format and check it before merging: GitHub appends the PR number.

## The changelog

[`CHANGELOG.md`](../CHANGELOG.md) is written by hand in the Keep a Changelog sections (`Added`, `Changed`, `Fixed`, ...). A `feat`, `fix` or breaking change adds a line under `Unreleased` in the same pull request. `docs`, `build`, `ci`, `style`, `chore` and most `refactor` commits need none.
