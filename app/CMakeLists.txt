# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.
cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

#添加库，分为两种，一种是需要编译为库的代码，一种是已经编译好的库文件
#需要编译为库的代码
add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp )

#添加avutil-56
add_library(avutil-56
            SHARED
            IMPORTED
            )

#添加swresample-3
add_library(swresample-3
            SHARED
            IMPORTED
            )

#添加avcodec-58
add_library(avcodec-58
            SHARED
            IMPORTED
            )

#avfilter-7
add_library(avfilter-7
            SHARED
            IMPORTED
            )

#swscale-5
add_library(swscale-5
            SHARED
            IMPORTED
            )

#avdevice-58
add_library(avdevice-58
            SHARED
            IMPORTED
            )

#avformat-58
add_library(avformat-58
            SHARED
            IMPORTED
            )

#postproc-55
add_library(postproc-55
            SHARED
            IMPORTED
            )


set_target_properties( avutil-56
                         PROPERTIES IMPORTED_LOCATION
                          ${CMAKE_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libavutil-56.so )

set_target_properties( swresample-3
                         PROPERTIES IMPORTED_LOCATION
                          ${CMAKE_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libswresample-3.so )

set_target_properties( avcodec-58
                         PROPERTIES IMPORTED_LOCATION
                          ${CMAKE_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libavcodec-58.so )

set_target_properties( avfilter-7
                         PROPERTIES IMPORTED_LOCATION
                          ${CMAKE_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libavfilter-7.so )

set_target_properties( swscale-5
                         PROPERTIES IMPORTED_LOCATION
                          ${CMAKE_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libswscale-5.so )


set_target_properties( avdevice-58
                         PROPERTIES IMPORTED_LOCATION
                          ${CMAKE_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libavdevice-58.so )


set_target_properties( avformat-58
                         PROPERTIES IMPORTED_LOCATION
                          ${CMAKE_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libavformat-58.so )

set_target_properties( postproc-55
                         PROPERTIES IMPORTED_LOCATION
                          ${CMAKE_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libpostproc-55.so )

#一般外面引入的库文件需要头文件
include_directories(
 ${CMAKE_SOURCE_DIR}/ffmpeg/
)

#关联引入库的文件和需要编译的native-lib
#target_include_directories(native-lib PRIVATE ${CMAKE_SOURCE_DIR}/ffmpeg/)

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.


#链接，把需要的so文件链接起来，这里native-lib需要链接ffmpeg的库文件和 log-lib
target_link_libraries( # Specifies the target library.
                       native-lib
                       swresample-3
                       avcodec-58
                       avfilter-7
                       swscale-5
                       avdevice-58
                       avformat-58
                       postproc-55
                       ${log-lib} )
