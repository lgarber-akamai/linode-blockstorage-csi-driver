FROM golang:1.18-alpine as builder
# from makefile
ARG REV

RUN mkdir -p /linode
WORKDIR /linode

COPY go.mod .
COPY go.sum .

COPY app/linode/main.go .
COPY pkg ./pkg

RUN go mod download

RUN CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-X main.vendorVersion='${REV}' -extldflags "-static"' -o /bin/linode-blockstorage-csi-driver /linode

FROM alpine
LABEL maintainers="Linode"
LABEL description="Linode CSI Driver"

COPY --from=builder /bin/linode-blockstorage-csi-driver /linode

RUN apk add --no-cache e2fsprogs findmnt blkid

ENTRYPOINT ["/linode"]
