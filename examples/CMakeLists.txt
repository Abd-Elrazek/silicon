cmake_minimum_required(VERSION 2.8)
SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH}
                      ${CMAKE_SOURCE_DIR}/../cmake)

set(CMAKE_CXX_COMPILER clang++) # Gcc fails to compile the blog example.

find_package( Iod )
find_package( Boost COMPONENTS system thread date_time program_options filesystem chrono)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/.. ${IOD_INCLUDE_DIR}
                    ${CMAKE_CURRENT_SOURCE_DIR}/../third_party/websocketpp
                    ${CMAKE_CURRENT_SOURCE_DIR}/../third_party/install/include
                    )

link_directories(${CMAKE_CURRENT_SOURCE_DIR}/../third_party/install/lib
                ${CMAKE_CURRENT_SOURCE_DIR}/../third_party/install/lib64
                $ENV{HOME}/local/lib)


set(WEBSOCKETS_LIBS
    ${Boost_LIBRARIES} pthread)

add_definitions(-std=c++1y  -ftemplate-depth=1024 -g)

set(MHD_LIBS gnutls gcrypt pthread microhttpd)

add_executable(simple simple.cc)
target_link_libraries(simple ${MHD_LIBS})

add_executable(blog blog.cc)
target_link_libraries(blog ${MHD_LIBS} sqlite3 nettle)

add_executable(blog_test blog_test.cc)
target_link_libraries(blog_test sqlite3 nettle ${MHD_LIBS} curl)

add_executable(ws_broadcast_server ws_broadcast_server.cc)
target_link_libraries(ws_broadcast_server ${WEBSOCKETS_LIBS})


add_executable(https https.cc)
target_link_libraries(https ${MHD_LIBS})

add_executable(file_streaming file_streaming.cc)
target_link_libraries(file_streaming ${MHD_LIBS})

add_executable(access_http_header_with_mhd access_http_header_with_mhd.cc)
target_link_libraries(access_http_header_with_mhd ${MHD_LIBS})

