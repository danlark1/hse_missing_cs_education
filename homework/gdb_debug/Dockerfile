FROM ubuntu:18.04

RUN apt-get update

RUN apt-get install -y g++ gdb

COPY with_symbols /opt

COPY without_symbols /opt

COPY with_core /opt

COPY coredump /opt

COPY flaky /opt
