# wf-worker:voice — slim voice-only worker image (neutral name).
# Base runtime (python stdlib + ffmpeg + git) + VieNeu Rust dist only.
# No Python VieNeu / torch / LLM payloads, so the image pulls far faster
# than the full worker image on cold Actions runners.
FROM python:3.11-slim-bookworm

ENV DEBIAN_FRONTEND=noninteractive \
    PYTHONUNBUFFERED=1 \
    LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    RUST_DIST=/tmp/rustdist

RUN apt-get update -qq \
 && apt-get install -y -qq --no-install-recommends \
      ffmpeg git ca-certificates curl \
 && rm -rf /var/lib/apt/lists/*

# VieNeu Rust distribution (public release asset in the workshop org).
ADD https://github.com/fruitcusp/test-actions-codespaces/releases/download/vieneu-rust-dist/vieneu-rust-dist.tar /tmp/vieneu-rust-dist.tar
RUN mkdir -p /tmp/rustdist \
 && tar -xf /tmp/vieneu-rust-dist.tar -C /tmp/rustdist \
 && (test -f /tmp/rustdist/vieneu-rs || (cp -a /tmp/rustdist/*/ /tmp/rustdist/ 2>/dev/null || true)) \
 && test -f /tmp/rustdist/vieneu-rs \
 && rm /tmp/vieneu-rust-dist.tar

WORKDIR /work
