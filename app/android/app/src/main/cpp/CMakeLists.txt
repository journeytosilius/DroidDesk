cmake_minimum_required(VERSION 3.22.1)

project("droiddesk_compositor")

# Add the native JNI bridge source
add_library(
    droiddesk_compositor
    SHARED
    compositor_jni.cpp
)

# Android 16 devices may use a 16 KiB memory page size. Keep every LOAD
# segment compatible even when this project is built with an older NDK.
target_link_options(
    droiddesk_compositor
    PRIVATE
    "-Wl,-z,max-page-size=16384"
    "-Wl,-z,common-page-size=16384"
)

# Search for standard Android libraries
find_library(log-lib log)
find_library(android-lib android)

# Link the libraries
target_link_libraries(
    droiddesk_compositor
    ${log-lib}
    ${android-lib}
)
