ARG VERSION="main"
ARG REVISION="12a86f0afadd75ceb16a4abc562e2ef007822592"

FROM docker.io/rust:alpine AS builder

ARG VERSION
ARG REVISION

RUN apk update && apk add --no-cache \
    git \
    ca-certificates-bundle \
    build-base \
    busybox \
    musl-utils \
    musl-dev \
    openssl-dev>3 \
    libssh2-dev \
    libgit2-dev \
    curl-dev \
    zlib-dev

RUN cargo install \
    cargo-auditable \
    cargo-c

RUN git clone --branch "${VERSION}" https://github.com/rustls/rustls-ffi /build/rustls-ffi \
    && cd /build/rustls-ffi \
    && git reset --hard "${REVISION}" \
    && cargo auditable capi build --release \
    && cargo auditable capi install --prefix=/usr --destdir "/build/artifacts/rustls-ffi/"

FROM scratch

ARG VERSION
ARG REVISION
ENV RUSTLS_FFI_VERSION="${VERSION}" \
    RUSTLS_FFI_REVISION="${REVISION}"

COPY --from=builder /build/artifacts/rustls-ffi/ /usr/local/rustls-ffi/

LABEL org.opencontainers.image.title="rustls-ffi" \
    org.opencontainers.image.description="rustls-ffi container build" \
    org.opencontainers.image.revision="${REVISION}" \
    org.opencontainers.image.version="${VERSION}"

