# mpv macbuild

Build instructions for creating a macOS `libmpv` supporting both Apple Silicon and Intel.

## Requirements

- macOS with Xcode command line tools installed
- Homebrew
- Python 3
- Git

## Setup

1. Clone the repository:

```bash
git clone https://github.com/user1/mpv-macbuild.git
cd mpv-macbuild
```

2. Install dependencies:

```bash
brew install python3 pkg-config meson ninja yasm libarchive libsndfile libjpeg-turbo libpng libvpx libx264 libx265 libopus libfreetype
```

## Build libmpv

Create separate build directories for Apple Silicon and Intel to produce universal libraries.

```bash
mkdir -p build-arm64 build-x86_64
```

### Build Apple Silicon

```bash
cd build-arm64
export CFLAGS='-arch arm64'
export CXXFLAGS='-arch arm64'
export LDFLAGS='-arch arm64'
meson setup --buildtype=release --prefix=/usr/local ../mpv
ninja
ninja install
cd ..
```

### Build Intel

```bash
cd build-x86_64
export CFLAGS='-arch x86_64'
export CXXFLAGS='-arch x86_64'
export LDFLAGS='-arch x86_64'
meson setup --buildtype=release --prefix=/usr/local ../mpv
ninja
ninja install
cd ..
```

### Create a universal libmpv

```bash
mkdir -p universal/lib
lipo -create build-arm64/usr/local/lib/libmpv.dylib build-x86_64/usr/local/lib/libmpv.dylib -output universal/lib/libmpv.dylib
```

## Notes

- Adjust dependency list as needed for your mpv configuration.
- Use `meson configure` to enable or disable specific mpv options.
- The universal library can be packaged with headers from the installed build.
