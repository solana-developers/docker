FROM ubuntu:20.04


ENV DEBIAN_FRONTEND=noninteractive


RUN apt-get update && \
    apt-get install -y libssl-dev curl bash git gnupg2 libudev-dev pkg-config zlib1g-dev llvm clang cmake make libprotobuf-dev protobuf-compiler


RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y


ENV PATH="/root/.cargo/bin:${PATH}"


RUN rustup component add rustfmt


RUN git clone https://github.com/solana-labs/solana.git
WORKDIR /solana


RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
    apt-get install -y nodejs && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install yarn


RUN cargo build --release


ENV PATH="/solana/target/release:${PATH}"


RUN cargo install --git https://github.com/project-serum/anchor avm --locked --force


RUN avm install latest && \
    avm use latest

RUN cargo install seahorse-lang