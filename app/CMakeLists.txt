# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fpic")
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fpic -std=c++11")


set(SRC_CODE_PATH src/main/cpp)

set(OPEN_CV_ROOT /Users/zhangcheng/OpenSourceHub/OpenCV-android-sdk )
include_directories(${OPEN_CV_ROOT}/sdk/native/jni/include)

add_library( lib_opencv STATIC IMPORTED )
set_target_properties(lib_opencv
                    PROPERTIES IMPORTED_LOCATION
                    /Users/zhangcheng/DeepLearning/OpenSrc/ncnn_android_sample/app/src/main/jniLibs/${ANDROID_ABI}/libopencv_java3.so)

#ncnn
include_directories(${SRC_CODE_PATH}/include)
add_library( libncnn STATIC IMPORTED )
set_target_properties(
            libncnn
            PROPERTIES IMPORTED_LOCATION
            /Users/zhangcheng/DeepLearning/OpenSrc/ncnn_android_sample/app/src/main/jniLibs/${ANDROID_ABI}/libncnn.a
)


add_library( # Sets the name of the library.
             squeezencnn

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/squeezencnn_jni.cpp )

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
                       squeezencnn
                        libncnn
                       lib_opencv

                       z
                       jnigraphics
                       ${log-lib}
                       )