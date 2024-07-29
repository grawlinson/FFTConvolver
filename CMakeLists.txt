cmake_minimum_required(VERSION 3.12)

project(FFTConvolver CXX)

option(FFTCONVOLVER_USE_SSE "Enable SSE optimizations" OFF)
option(FFTCONVOLVER_BUILD_TESTS "Build tests" OFF)

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
