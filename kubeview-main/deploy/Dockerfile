# Stage 1
FROM golang:1.24-alpine AS build

WORKDIR /app

# Copy go module files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy source code and static assets
COPY server/ ./server/
COPY public/ ./public/

# Build the Go binary from server/
RUN CGO_ENABLED=0 GOOS=linux go build -o kubeview-app ./server

# Stage 2
FROM gcr.io/distroless/static-debian11

WORKDIR /app

COPY --from=build /app/kubeview-app ./kubeview-app
COPY --from=build /app/public ./public

EXPOSE 8000

ENTRYPOINT ["/app/kubeview-app"]
