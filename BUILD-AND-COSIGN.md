# Build-your-own hardened image & co-sign

For customers who want to wrap our **prebuilt binary** in their own hardened
base image and apply their own signature.

Each product release attaches a `…-linux-amd64.tar.gz` (see this repo's
**Releases**) containing:
- the static linux/amd64 binary (CGO disabled — runs on scratch/distroless/your base)
- the runtime assets (HTML/JS/CSS, or templates/static)
- a reference `Dockerfile` (parameterised `BASE_IMAGE`)
- `SHA256SUMS` and `BUILD-INFO.txt` (source commit + exact build flags)

## Build with your hardened base
```bash
tar xzf cyber-advisory-v1.0.1-linux-amd64.tar.gz
shasum -a 256 -c SHA256SUMS            # verify integrity

docker build \
  --build-arg BASE_IMAGE=<your-hardened-base> \
  -t your-registry/cyber-advisory:v1.0.1 .
docker push your-registry/cyber-advisory:v1.0.1
```

## Co-sign (Cosign / Sigstore)
```bash
cosign sign --key <your-key> your-registry/cyber-advisory:v1.0.1
# or keyless:  cosign sign your-registry/cyber-advisory:v1.0.1
```

The binary is identical to the one in our published `export-containers/<app>`
image for the same version tag, so signatures over either are equivalent.
