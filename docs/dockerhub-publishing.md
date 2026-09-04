# Docker Hub publishing

The fork publishes the AIO image to Docker Hub as:

```text
docker.io/aqqtran/openlist:latest-aio
```

Every successful main branch publish also creates a run-unique rollback tag:

```text
docker.io/aqqtran/openlist:commit-<full git SHA>-run-<run ID>-<attempt>
```

The run ID and attempt keep reruns from reusing a previously published rollback tag.

The `Docker Hub (AIO)` workflow validates the root `Dockerfile` for Linux amd64 and arm64 on pull requests, pushes to `main`, and manual dispatches. Each platform builds on its native GitHub-hosted runner (`ubuntu-latest` for amd64 and `ubuntu-24.04-arm` for arm64), which avoids QEMU emulation. Pull requests and manual dispatches from other branches build without Docker Hub login or pushes. On a `main` push or manual dispatch of `main`, each runner pushes its platform image by digest; a final job combines both digests and publishes the two tags above.

The build uses `BASE_IMAGE_TAG=aio` and leaves the Dockerfile's other build arguments at their existing defaults. Docker Hub authentication uses the `DOCKERHUB_TOKEN` Actions secret with the `aqqtran` username. The workflow keeps the source and revision OCI labels and emits BuildKit provenance for published images.

The upstream beta and release Docker workflows keep their original behavior in `OpenListTeam/OpenList`; all of their Docker build and publishing jobs are skipped in this fork.
