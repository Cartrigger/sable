# Build container versions pinned on 04-18-26
FROM messense/cargo-zigbuild@sha256:4c18634a3d9c7775683b0f1e313584770d32216623a1273a104b634fa294bf4c

ARG RUST_VERSION
RUN rustup default ${RUST_VERSION}
RUN rustup target add x86_64-apple-darwin
RUN rustup target add aarch64-apple-darwin
RUN rustup target add x86_64-unknown-linux-gnu
RUN rustup target add aarch64-unknown-linux-gnu
RUN rustup target add x86_64-unknown-freebsd
RUN rustup target add aarch64-linux-android
# used for AArch64 FreeBSD
#RUN rustup component add rust-src

# Based on code from https://github.com/user01134/sable
# Allow builds from the Android NDK
RUN apt-get update && apt-get install -y wget unzip \
 && wget https://dl.google.com/android/repository/android-ndk-r26d-linux.zip \
 && unzip android-ndk-r26d-linux.zip -d /opt/ \
 && rm android-ndk-r26d-linux.zip
ENV ANDROID_NDK_HOME=/opt/android-ndk-r26d
ENV PATH=$ANDROID_NDK_HOME/toolchains/llvm/prebuilt/linux-x86_64/bin:$PATH
ENV CC_aarch64_linux_android=aarch64-linux-android21-clang
ENV CXX_aarch64_linux_android=aarch64-linux-android21-clang++

# Help rustc find NDK system libs when linking for aarch64-android
ENV ANDROID_API=21
ENV ANDROID_SYSROOT=$ANDROID_NDK_HOME/toolchains/llvm/prebuilt/linux-x86_64/sysroot
ENV ANDROID_LIBDIR=$ANDROID_SYSROOT/usr/lib/aarch64-linux-android/$ANDROID_API
ENV CARGO_TARGET_AARCH64_LINUX_ANDROID_RUSTFLAGS=-Lnative=$ANDROID_LIBDIR

WORKDIR /