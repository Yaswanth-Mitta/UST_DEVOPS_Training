FROM ubuntu:latest

RUN apt-get update && \
    apt-get install -y g++ && \
    apt-get clean

WORKDIR /app

COPY main.cpp .

RUN g++ main.cpp -o app

CMD["./app"]