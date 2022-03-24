# A recipe for building the gvisor runsc sandbox.  Output is a single file.

FROM node:14-buster as runsc-builder

RUN \
  apt update && apt install -y git

# We'll need bazel to build gvisor.
RUN \
  npm install @bazel/bazelisk

# Fetch fork of gvisor supporting an --unprivileged flag.
# See https://github.com/google/gvisor/issues/4371
RUN \
git clone https://github.com/gristlabs/gvisor && \
  cd gvisor && \
  git checkout gvisor-default-docker-ct-with-updates && \
  cd ..

# Build gvisor.  This won't be fast, sorry.
RUN \
  cd gvisor && \
  /node_modules/.bin/bazel build runsc

# Move the single binary produced to /usr/bin/runsc.  Its location is a bit unpredictable.
RUN \
  cp `find /root/.cache/bazel/_bazel_root/ -iname "runsc" -type f | head` /usr/bin/runsc && \
  ls -l /usr/bin/runsc && \
  /usr/bin/runsc help

RUN \
  strip -s /usr/bin/runsc


# All we need is the binary, so drop everything else.
FROM scratch
COPY --from=runsc-builder /usr/bin/runsc runsc
