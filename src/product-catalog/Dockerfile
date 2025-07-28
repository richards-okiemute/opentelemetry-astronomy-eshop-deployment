FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app/

# Use Go build cache for dependencies
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build

# Copy
COPY go.mod go.sum ./

# Download
RUN go mod download

# Copy the rest of the source code
COPY . .

RUN go build -o product-catalog .

####################################

# trivy vuln. check on alpine: Total: 0 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 0, CRITICAL: 0)
FROM alpine AS release

RUN addgroup -S products-group && adduser -S products-user -G products-group

WORKDIR /usr/src/app/

COPY ./products/ ./products/
COPY --from=builder /usr/src/app/product-catalog/ ./

# Ensure correct ownership and permissions
RUN chown -R products-user:products-group /usr/src/app && chmod +x product-catalog

# Switch to non-root user
USER products-user

# Set environment variables
ENV PRODUCT_CATALOG_PORT=8088


ENTRYPOINT [ "./product-catalog" ]
