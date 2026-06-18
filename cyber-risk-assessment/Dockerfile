# Reference runtime image — Cyber Risk Assessment
# Wraps the prebuilt static linux/amd64 binary + assets. The binary is fully
# static (CGO disabled), so it runs on scratch/distroless or your own hardened
# base. Override BASE_IMAGE with your approved hardened base, then co-sign.
ARG BASE_IMAGE=gcr.io/distroless/static-debian12:nonroot
FROM ${BASE_IMAGE}
WORKDIR /app
COPY cyber-risk-assessment /app/cyber-risk-assessment
COPY index.html styles.css favicon.svg /app/
EXPOSE 8080
ENV HOST=0.0.0.0 PORT=8080
USER 65532:65532
ENTRYPOINT ["/app/cyber-risk-assessment"]
