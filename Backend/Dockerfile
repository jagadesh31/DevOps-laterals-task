FROM rust:1.70 AS builder

RUN apt-get update && apt-get install -y \
    libpq-dev pkg-config && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY Cargo.toml Cargo.lock ./

RUN mkdir src && echo 'fn main(){println!("build deps");}' > src/main.rs
RUN cargo build --release
RUN rm -rf ./.cargo/registry ./.cargo/git src target

COPY . .
RUN cargo build --release --bin login-app-backend

RUN test -f target/release/login-app-backend && \
    echo "REAL BINARY BUILT: $(stat -c%s target/release/login-app-backend) bytes" && \
    ls -la target/release/login-app-backend

FROM debian:bullseye-slim

RUN apt-get update && apt-get install -y \
    libpq5 ca-certificates file && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=builder /app/target/release/login-app-backend /usr/local/bin/backend

RUN ls -la /usr/local/bin/backend && \
    file /usr/local/bin/backend && \
    stat -c "FINAL BINARY: %s bytes - $(file /usr/local/bin/backend)" /usr/local/bin/backend

EXPOSE 8080
ENTRYPOINT ["/usr/local/bin/backend"]
