# Reference runtime image — Policy Advisor (Cyber Policy) v1.0.10
# Wraps the prebuilt static linux/amd64 binary + assets. Static binary (CGO
# disabled) → runs on scratch/distroless or your own hardened base. Override
# BASE_IMAGE with your approved hardened base, then co-sign.
ARG BASE_IMAGE=gcr.io/distroless/static-debian12:nonroot
FROM ${BASE_IMAGE}
WORKDIR /app
COPY server /app/server
COPY templates /app/templates
COPY static /app/static
EXPOSE 3000
USER 65532:65532
ENTRYPOINT ["/app/server"]
