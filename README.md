# FFTConvolver

FFTConvolver is a C++ library for highly efficient convolution of
audio data (e.g. for usage in real-time convolution reverbs etc.).

- Partitioned convolution algorithm (using uniform block sizes)
- Optional support for non-uniform block sizes (TwoStageFFTConvolver)
- No external dependencies (FFT already included)
- Optional optimization for SSE (enabled by defining `FFTCONVOLVER_USE_SSE`)
- Optional tests (enabled by defining `FFTCONVOLVER_BUILD_TESTS`)

## Building

Use [CMake][1] to generate the project, e.g.:

```sh
cmake -S . -B build -D CMAKE_BUILD_TYPE=Release

# Or enable SSE optimizations (OFF by default)
cmake -S . -B build -D CMAKE_BUILD_TYPE=Release -D FFTCONVOLVER_USE_SSE=ON
```

You can build the project by using the generated build files directly, or by
running the build tool through CMake:

```sh
cmake --build build
```

## Testing

To run the test-suite, pass `-D FFTCONVOLVER_BUILD_TESTS=ON` to the first
`cmake` invocation.

```sh
cmake -S . -B build -D FFTCONVOLVER_BUILD_TESTS=ON
cmake --build build
ctest --test-dir build
```

### License

This project utilizes the [MIT](COPYING.txt) license.

The FFT implementation is based on the great radix-4 routines by [Takuya Ooura][0].

[0]: https://www.kurims.kyoto-u.ac.jp/~ooura/fft.html
[1]: https://cmake.org
