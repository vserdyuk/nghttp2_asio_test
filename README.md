# nghttp2_asio_test

This is an example project using nghttp2_asio library with patches for:
- building dynamic nghttp2_asio with MSVC
- building static nghttp2_asio (checked on macOS and Linux)
- installing nghttp2_asio via vcpkg

## How to build

### Clone

```
git clone https://github.com/vserdyuk/nghttp2_asio_test.git
cd nghttp2_asio_test
git submodule update --init
```

### Init vcpkg

```
cd vcpkg
bootstrap-vcpkg.bat
```
or (macOS/Linux)
```
./bootstrap-vcpkg.sh
```

More info on vcpkg is on the [official page](https://github.com/microsoft/vcpkg).

### Install nghttp2[asio]

```
vcpkg install nghttp2[asio]:x64-windows
```

or

```
.\vcpkg install nghttp2[asio]:x64-osx
```

or

```
.\vcpkg install nghttp2[asio]:x64-linux
```

### Build

From the project's root directory

```
cmake -B out/build -S .
cmake --build out/build
```

## Upstreaming

1. nghttp2: https://github.com/nghttp2/nghttp2/pull/1615
2. getopt-win32 (buggy _BEGIN_EXTERN_C/_END_EXTERN_C macros): TODO
3. vcpkg: if 1 and 2 accepted
