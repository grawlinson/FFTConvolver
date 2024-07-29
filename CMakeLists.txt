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
    # Test executable
    set(Test_SOURCES
        test/Test.cpp
    )

    add_executable(Test ${Test_SOURCES})
    target_link_libraries(Test PRIVATE FFTConvolver)
endif()

