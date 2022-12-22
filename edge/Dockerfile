ARG VERSION="main"
ARG REVISION="ead36d7dfbf85e8626cfdc8d01e9188808865d03"

FROM ghcr.io/westonsteimel/rust-static-builder:stable as builder

ARG VERSION
ARG REVISION

RUN git clone --branch "${VERSION}" https://github.com/rustls/rustls-ffi /build/rustls-ffi \
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

