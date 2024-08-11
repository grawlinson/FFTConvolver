# FFTConvolver

FFTConvolver is a C++ library for highly efficient convolution of
audio data (e.g. for usage in real-time convolution reverbs etc.).

- Partitioned convolution algorithm (using uniform block sizes)
- Optional support for non-uniform block sizes (TwoStageFFTConvolver)
- No external dependencies (FFT already included)
- Optional optimization for SSE vector operations
- Optional FFT acceleration (Apple Accelerate, Intel IPP & FFTW3)
- Optional tests

## Building

Use [CMake][1] to generate the project, e.g.:

```sh
cmake -S . -B build -D CMAKE_BUILD_TYPE=Release
```

You can build the project by using the generated build files directly, or by
running the build tool through CMake:

```sh
cmake --build build
```

### Options

All options described below default to `OFF`.

| Option | Description |
| ------ | ----------- |
|`AUDIOFFT_BUILD_TESTS`|Build tests for AudioFFT|
|`AUDIOFFT_INTEL_IPP`|Enable usage of Intel IPP library (Linux+Windows)|
|`AUDIOFFT_APPLE_ACCELERATE`|Enable usage of Apple Accelerate library (Apple only)|
|`AUDIOFFT_FFTW3`|Enable usage of FFTW3 library|
|`FFTCONVOLVER_USE_SSE`|Enable SSE optimizations|
|`FFTCONVOLVER_BUILD_TESTS`|Build tests for FFTConvolver|

## Testing

```sh
cmake -S . -B build -D FFTCONVOLVER_BUILD_TESTS=ON -D AUDIOFFT_BUILD_TESTS=ON
cmake --build build
ctest --test-dir build
```

## License

This project utilizes the [MIT](COPYING.txt) license.

The FFT implementation is based on the great radix-4 routines by [Takuya Ooura][0].

[0]: https://www.kurims.kyoto-u.ac.jp/~ooura/fft.html
[1]: https://cmake.org
