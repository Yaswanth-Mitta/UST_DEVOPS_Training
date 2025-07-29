# Stage 1: Build using GCC
FROM gcc:13 AS builder

WORKDIR /app
COPY main.cpp .
RUN g++ -static -o hello main.cpp

# Stage 2: Run in Alpine
FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/hello .
ENTRYPOINT ["./hello"]
