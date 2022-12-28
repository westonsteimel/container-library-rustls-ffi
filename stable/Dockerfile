ARG VERSION="0.8.2"
ARG REVISION="0c8ce8508cbc54d9d00d53f3f77af0647824dfb3"

FROM docker.io/rust:alpine as builder

ARG VERSION
ARG REVISION

RUN git clone --branch "v${VERSION}" https://github.com/rustls/rustls-ffi /build/rustls-ffi \
    && cd /build/rustls-ffi \
    && git reset --hard "${REVISION}" \
    && cargo install cbindgen \
    && make \
    && make DESTDIR=/build/artifacts/rustls-ffi/ install

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

