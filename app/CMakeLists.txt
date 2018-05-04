# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.
#cmake最低版本
cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.



#设置需要编译的代码位置
set(distribution_DIR ${CMAKE_SOURCE_DIR}/../../../../libs)

#添加库，分为两种，一种是需要编译为库的代码，一种是已经编译好的库文件。 比如上面编译好的ffmpeg的库

#添加avutil-56

#已经编译好的库文件
add_library(avutil-56
            SHARED
            IMPORTED
            )
#已经编译好的库文件
set_target_properties( avutil-56
                         PROPERTIES IMPORTED_LOCATION
                         ../../../../libs/armeabi/libavutil-56.so )

#添加swresample-3

#已经编译好的库文件
add_library(swresample-3
            SHARED
            IMPORTED
            )
#已经编译好的库文件
set_target_properties( swresample-3
                         PROPERTIES IMPORTED_LOCATION
                         ../../../../libs/armeabi/swresample-3.so )

#添加avcodec-58

#已经编译好的库文件
add_library(avcodec-58
            SHARED
            IMPORTED
            )
#已经编译好的库文件
set_target_properties( avcodec-58
                         PROPERTIES IMPORTED_LOCATION
                         ../../../../libs/armeabi/avcodec-58.so )

#avfilter-7

#已经编译好的库文件
add_library(avfilter-7
            SHARED
            IMPORTED
            )
#已经编译好的库文件
set_target_properties( avfilter-7
                         PROPERTIES IMPORTED_LOCATION
                         ../../../../libs/armeabi/avfilter-7.so )

#swscale-5

#已经编译好的库文件
add_library(swscale-5
            SHARED
            IMPORTED
            )
#已经编译好的库文件
set_target_properties( swscale-5
                         PROPERTIES IMPORTED_LOCATION
                         ../../../../libs/armeabi/swscale-5.so )

#avdevice-58

#已经编译好的库文件
add_library(avdevice-58
            SHARED
            IMPORTED
            )
#已经编译好的库文件
set_target_properties( avdevice-58
                         PROPERTIES IMPORTED_LOCATION
                         ../../../../libs/armeabi/avdevice-58.so )

#avformat-58

#已经编译好的库文件
add_library(avformat-58
            SHARED
            IMPORTED
            )
#已经编译好的库文件
set_target_properties( avformat-58
                         PROPERTIES IMPORTED_LOCATION
                         ../../../../libs/armeabi/avformat-58.so )

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")



#需要编译为库的代码
add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp )

add_library( # Sets the name of the library.
             filtering_audio

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/ffmpeg_examples/filtering_audio.cpp )

#一般外面引入的库文件需要头文件

include_directories(libs/ffmpeg)

#关联引入库的文件和需要编译的native-lib
target_include_directories(native-lib PRIVATE libs/ffmpeg)

#关联引入库的文件和需要编译的filtering_audio
target_include_directories(filtering_audio PRIVATE libs/ffmpeg)


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


#链接，把需要的so文件链接起来，这里native-lib需要链接ffmpeg的库文件和 log-lib
target_link_libraries( native-lib
                        swresample-3 avcodec-58 avfilter-7 swscale-5 avdevice-58 avformat-58


                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )
