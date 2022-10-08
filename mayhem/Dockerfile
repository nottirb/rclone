# Build in a stock Go builder container
FROM fuzzers/go-fuzz:1.2.0 as builder

ADD . /rclone
WORKDIR /rclone

RUN cd fs/fspath && GO_ENABLED=0 go-fuzz-build -libfuzzer -o ../fspath.a .
RUN cd fs && clang -fsanitize=fuzzer fspath.a -o fspath.libfuzzer

FROM ubuntu:20.04
COPY --from=builder /rclone/fs/fspath.libfuzzer /

# CMD ["./fspath.libfuzzer"]