cmake_minimum_required(VERSION 3.20)
project(leap_clion C)

set(CMAKE_C_STANDARD 11)

add_executable(leap_clion
        main.c
        ExampleConnection.c
)

# LeapC headers
target_include_directories(leap_clion PRIVATE
        "/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/include"
)

# LeapC library folder
target_link_directories(leap_clion PRIVATE
        "/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/lib"
)

# Link LeapC + required macOS framework
target_link_libraries(leap_clion PRIVATE
        LeapC
        "-framework CoreFoundation"
)

# Runtime library search path so dyld can find libLeapC.dylib
set_target_properties(leap_clion PROPERTIES
        BUILD_RPATH "/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/lib"
)

