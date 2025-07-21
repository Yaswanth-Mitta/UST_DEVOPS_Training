FROM gcc:latest AS builder
WORKDIR /app
COPY main.cpp .
RUN g++ -o app main.cpp

FROM ubuntu:latest
WORKDIR /app
COPY --from=builder /app .
CMD ["./app"]


