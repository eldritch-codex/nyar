<!-- covers: CMakeLists.txt, .github/workflows/ -->
# Branching and releases

Nyar follows the same development cycle as [Nodens](https://github.com/eldritch-codex/nodens/blob/main/docs/BRANCHING.md): one trunk, `main`. A change integrates into it either as a direct commit or through a short pull request. A release is an annotated tag on `main`, and a CI job publishes it. The reasoning is in the Nodens page; this page lists what applies to Nyar.

## Branch roles

| Branch | Purpose | Lifetime | Ends by |
|---|---|---|---|
| `main` | The trunk and the only published lineage. Every commit on it builds. | Permanent | Never |
| `feature/<slug>` | One change, cut from `main`. | Hours to a few days | Squashed into `main` through a pull request |
| `hotfix/<slug>` | A fix for a tag that already shipped, cut from that tag. | Hours | Squashed into `main`, then a patch tag on `main` |
| `release/<version>` | A released line that must stay stable while `main` moves on. | One release cycle | Deleted after the last patch tag |

There is no `dev` branch. Keep a `release/<version>` branch only while two versions must be supported at the same time.

## Rules

1. `main` is kept green. A failing `main` is fixed before any other change, and a tag is created only on a commit whose CI run succeeded.
2. A change takes one of two routes, by the type of the commit (see [Which route](#which-route)). A small change is committed directly to `main`; a risky one goes through a pull request and lands as one squashed commit. Commit and pull request titles follow [Committing](COMMITTING.md).
3. Tags are annotated, named `vMAJOR.MINOR.PATCH`, and created on `main` only.
4. While the major version is 0, a breaking change bumps the minor version.
5. The tag name and the CMake project version must agree, because the release job refuses to publish when they do not (`project(Nyar VERSION ...)` in `CMakeLists.txt`).
6. Nyar fetches Nodens at a release tag (`GIT_TAG` in `CMakeLists.txt`). Moving to a newer Nodens changes the build, so it takes the pull request route and adds a changelog line.

## Which route

The type of the commit decides the route. When a change mixes types, it takes the riskier route.

| Route | Commit types | Why |
|---|---|---|
| Direct to `main` | `docs`, `style`, `chore`, `build` for tooling and the development environment (`.devcontainer/`, `.zed/`, `scripts/`), release preparation | They do not change what Nyar compiles to, or they are cheap to undo |
| Pull request | `feat`, `fix`, `refactor` and `perf` (C++ or shaders), `build` that changes `CMakeLists.txt` or the Nodens version, and `ci` | A mistake breaks the build, or breaks CI itself, so CI runs before the change lands |

Before a direct commit that touches C++, shaders or the build, run `bash scripts/check.sh` in the dev container. It checks the formatting of the C++ files changed from `origin/main`, then configures and builds Debug in `build/check`, so your own build tree is left alone:

```bash
devcontainer exec --workspace-folder . --config .devcontainer/devcontainer.json bash scripts/check.sh
```

A direct commit gets no CI run before it lands, only the push run after it, so a red push run is fixed first (rule 1).

## A small change

```bash
git switch main
git pull --ff-only
# edit; for C++ or build changes, run scripts/check.sh
git add path/to/file
git commit
git push
```

## A change by pull request

```bash
git switch main
git pull --ff-only
git switch -c feature/texture-uploads
# work, commit
git push -u origin feature/texture-uploads
gh pr create --base main --fill
```

Merge it with "Squash and merge", then delete the branch.

## A release

Nyar has no release yet. For the first one, on `main`:

- bump `project(Nyar VERSION ...)` in `CMakeLists.txt`
- give the version a dated section in `CHANGELOG.md`, moved out of `Unreleased`; the release job publishes that section as the release notes
- replace the development-status block in `README.md`
- commit it directly as `chore(release): prepare X.Y.Z`, push, and confirm the CI run on that commit succeeded (`gh run list --branch main --commit "$(git rev-parse HEAD)"`)

```bash
git tag -a v0.1.0 -m "Nyar 0.1.0"
git push origin v0.1.0
```

The tag push builds the tag, checks the version against `CMakeLists.txt`, uploads a source archive and creates the GitHub Release, titled with the bare version. The job fails when the changelog section is missing.

## What CI checks, and when

| Event | Jobs | What it proves |
|---|---|---|
| Pull request into `main` | `format`, `build` for Debug and Release | the change compiles in both configurations and the C++ files it touches are formatted |
| Push to `main` | `format`, `build` | the trunk stays releasable; for a direct commit this is its only CI run, after it lands |
| Tag `v*` | `build`, `release` | the tagged commit builds and the release is published from it |

The jobs run inside the shared development image (`ghcr.io/eldritch-codex/arch-dev:main`), so CI uses the same compiler and CMake as the dev container. The `format` job runs `clang-format` on the C++ files that the change touches, so a file is held to the style when it is edited.
