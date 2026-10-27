<!-- covers: CMakeLists.txt, .github/workflows/ -->
# Branching and releases

Nyar follows the same development cycle as [Nodens](https://github.com/eldritch-codex/nodens/blob/main/docs/BRANCHING.md): one trunk, `main`, and every change integrates into it through a pull request. A release is an annotated tag on `main`, and a CI job publishes it. The reasoning is in the Nodens page; this page lists what applies to Nyar.

## Branch roles

| Branch | Purpose | Lifetime | Ends by |
|---|---|---|---|
| `main` | The trunk and the only published lineage. Every commit on it builds. | Permanent | Never |
| `feature/<slug>` | One change, cut from `main`. | Hours to a few days | Squashed into `main` through a pull request |
| `hotfix/<slug>` | A fix for a tag that already shipped, cut from that tag. | Hours | Squashed into `main`, then a patch tag on `main` |
| `release/<version>` | A released line that must stay stable while `main` moves on. | One release cycle | Deleted after the last patch tag |

There is no `dev` branch. Keep a `release/<version>` branch only while two versions must be supported at the same time.

## Rules

1. `main` always builds and always passes its checks.
2. Every change enters `main` through a pull request and lands as one squashed commit. Commit and pull request titles follow [Committing](COMMITTING.md).
3. Tags are annotated, named `vMAJOR.MINOR.PATCH`, and created on `main` only.
4. While the major version is 0, a breaking change bumps the minor version.
5. The tag name and the CMake project version must agree, because the release job refuses to publish when they do not (`project(Nyar VERSION ...)` in `CMakeLists.txt`).
6. Nyar fetches Nodens at a release tag (`GIT_TAG` in `CMakeLists.txt`). Moving to a newer Nodens is a change like any other: a branch, a pull request and a changelog line.

## A change

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
- confirm CI is green on `main`

```bash
git tag -a v0.1.0 -m "Nyar 0.1.0"
git push origin v0.1.0
```

The tag push builds the tag, checks the version against `CMakeLists.txt`, uploads a source archive and creates the GitHub Release, titled with the bare version. The job fails when the changelog section is missing.

## What CI checks, and when

| Event | Jobs | What it proves |
|---|---|---|
| Pull request into `main` | `format`, `build` for Debug and Release | the change compiles in both configurations and the C++ files it touches are formatted |
| Push to `main` | `format`, `build` | the trunk stays releasable |
| Tag `v*` | `build`, `release` | the tagged commit builds and the release is published from it |

The jobs run inside the shared development image (`ghcr.io/eldritch-codex/arch-dev:main`), so CI uses the same compiler and CMake as the dev container. The `format` job runs `clang-format` on the C++ files that the change touches, so a file is held to the style when it is edited.
