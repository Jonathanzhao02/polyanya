# Build Stage
FROM --platform=linux/amd64 rustlang/rust:nightly as builder

ENV DEBIAN_FRONTEND=noninteractive
## Install build dependencies.
RUN apt-get update 
RUN apt-get install -y cmake clang
RUN cargo install cargo-fuzz

## Add source code to the build stage.
ADD . /polyanya/

WORKDIR /polyanya/fuzz/

RUN cargo +nightly fuzz build

FROM --platform=linux/amd64 rustlang/rust:nightly

## TODO: Change <Path in Builder Stage>

COPY --from=builder /polyanya/fuzz/target/x86_64-unknown-linux-gnu/release/vertex /

COPY --from=builder /polyanya/fuzz/target/x86_64-unknown-linux-gnu/release/polygon /

COPY --from=builder /polyanya/fuzz/target/x86_64-unknown-linux-gnu/release/path /