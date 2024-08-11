# update minimum cmake version when oldest ubuntu LTS drops support
cmake_minimum_required(VERSION 3.16)

# require a minimum of C++ 11, but let's aim higher
if(NOT DEFINED CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

project(FFTConvolver LANGUAGES CXX)

# options
option(AUDIOFFT_BUILD_TESTS "Build tests for AudioFFT" OFF)
option(AUDIOFFT_INTEL_IPP "Enable usage of Intel IPP library" OFF)
option(AUDIOFFT_APPLE_ACCELERATE "Enable usage of Apple Accelerate library (Apple only)" OFF)
option(AUDIOFFT_FFTW3 "Enable usage of FFTW3 library" OFF)
option(FFTCONVOLVER_USE_SSE "Enable SSE optimizations" OFF)
option(FFTCONVOLVER_BUILD_TESTS "Build tests for FFTConvolver" OFF)

# AudioFFT static library
set(AudioFFT_SOURCES
  src/AudioFFT.cpp
  include/FFTConvolver/AudioFFT.h
)
add_library(AudioFFT STATIC ${AudioFFT_SOURCES})
target_include_directories(AudioFFT PUBLIC include)

# FFTConvolver static library
set(FFTConvolver_SOURCES
  src/FFTConvolver.cpp
  include/FFTConvolver/FFTConvolver.h

  src/TwoStageFFTConvolver.cpp
  include/FFTConvolver/TwoStageFFTConvolver.h

  src/Utilities.cpp
  include/FFTConvolver/Utilities.h
)

add_library(FFTConvolver STATIC ${FFTConvolver_SOURCES})
target_link_libraries(FFTConvolver PRIVATE AudioFFT)
target_include_directories(FFTConvolver PUBLIC include)

if(FFTCONVOLVER_USE_SSE)
    target_compile_definitions(FFTConvolver PRIVATE -DFFTCONVOLVER_USE_SSE)
endif()

if(FFTCONVOLVER_BUILD_TESTS)
    enable_testing()
    set(Test_FFT_SOURCES
        test/test_fftconvolver.cpp
    )
    add_executable(test_fftconvolver ${Test_FFT_SOURCES})
    target_link_libraries(test_fftconvolver PRIVATE FFTConvolver)
    add_test(NAME Test_FFTConvolver COMMAND test_fftconvolver)

    set(Test_AFFT_SOURCES
        test/test_audiofft.cpp
    )
    add_executable(test_audiofft ${Test_AFFT_SOURCES})
    target_link_libraries(test_audiofft PRIVATE AudioFFT)
    add_test(NAME Test_AudioFFT COMMAND test_audiofft)
endif()
