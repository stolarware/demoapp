# # Dockerfile
# FROM golang:1.21-alpine

# WORKDIR /app

# # Skopiuj pliki aplikacji
# COPY . .

# # Buduj aplikację
# RUN go build -o server main.go

# # Otwórz port
# EXPOSE 8080

# # Uruchom aplikację
# CMD ["./server"]



###########################################
# Build the manager binary
FROM registry.stolarware.pl/paas-tools/paas-go-toolset:1.24.1-3 AS builder
ARG TARGETOS
ARG TARGETARCH

WORKDIR /workspace
# Copy the Go Modules manifests
COPY go.mod go.mod
# COPY go.sum go.sum
# cache deps before building and copying source so that we don't need to re-download as much
# and so that source changes don't invalidate our downloaded layer

# copy certificates
COPY certificates/* /etc/pki/ca-trust/source/anchors/
ENV SSL_CERT_DIR=/etc/pki/ca-trust/source/anchors/

# add proxy
RUN go mod download

# Copy the go source
COPY src/main.go src/main.go
COPY web/templates web/templates
COPY web/static web/static

# Build
# the GOARCH has not a default value to allow the binary be built according to the host where the command
# was called. For example, if we call make docker-build in a local env which has the Apple Silicon M1 SO
# the docker BUILDPLATFORM arg will be linux/arm64 when for Apple x86 it will be linux/amd64. Therefore,
# by leaving it empty we can ensure that the container and binary shipped on it will have the same platform.
RUN CGO_ENABLED=0 GOOS=${TARGETOS:-linux} GOARCH=${TARGETARCH} go build -a -o server src/main.go

# Use distroless as minimal base image to package the manager binary
# Refer to https://github.com/GoogleContainerTools/distroless for more details
# FROM gcr.io/distroless/static:nonroot
FROM registry.stolarware.pl/ubi9-minimal:9.6-1751286687

# copy certificates
COPY certificates/* /etc/pki/ca-trust/source/anchors/
ENV SSL_CERT_DIR=/etc/pki/ca-trust/source/anchors/

# copy the manager binary
WORKDIR /
COPY --from=builder /workspace/server .
COPY --from=builder /workspace/web/templates web/templates
COPY --from=builder /workspace/web/static web/static
# COPY ./bin/manager .
# USER 65532:65532

ENTRYPOINT ["/server"]
