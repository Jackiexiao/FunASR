
file(GLOB files1 "*.cpp")
set(files ${files1})

message("files: "${files})

add_library(funasr SHARED ${files})

if(WIN32)
    set(EXTRA_LIBS pthread yaml-cpp csrc kaldi-decoder fst glog gflags)
    if(CMAKE_CL_64)
        target_link_directories(funasr PUBLIC ${CMAKE_SOURCE_DIR}/win/lib/x64)
    else()
        target_link_directories(funasr PUBLIC ${CMAKE_SOURCE_DIR}/win/lib/x86)
    endif()
    target_include_directories(funasr PUBLIC ${CMAKE_SOURCE_DIR}/win/include )
    
    target_compile_definitions(funasr PUBLIC -D_FUNASR_API_EXPORT)
else()
    set(EXTRA_LIBS pthread yaml-cpp csrc kaldi-decoder fst glog gflags avutil avcodec avformat swresample)
    include_directories(${ONNXRUNTIME_DIR}/include)
    include_directories(${FFMPEG_DIR}/include)
endif()

#message("CXX_FLAGS "${CMAKE_CXX_FLAGS})
include_directories(${CMAKE_SOURCE_DIR}/include)
include_directories(${CMAKE_SOURCE_DIR}/third_party)
target_link_libraries(funasr PUBLIC onnxruntime ${EXTRA_LIBS})
