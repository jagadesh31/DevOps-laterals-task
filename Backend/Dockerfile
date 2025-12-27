FROM rust:1.70 AS builder

RUN apt-get update && apt-get install -y \
    libpq-dev \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app


COPY Cargo.toml Cargo.lock ./
RUN mkdir src && echo "fn main() { println!(\"build deps\"); }" > src/main.rs
RUN cargo build --release
RUN rm -rf src


COPY . .


RUN cargo build --release --bin login-app-backend

FROM debian:bullseye-slim

RUN apt-get update && apt-get install -y \
    libpq5 \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=builder /app/target/release/login-app-backend /app/backend

EXPOSE 8080

CMD ["./backend"]
