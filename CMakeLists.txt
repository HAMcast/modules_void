cmake_minimum_required(VERSION 2.6)
project (hamcast_libvoid_module)

# Set up environment paths to cmake modules and libhamcast
set (CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR})

#set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

if (CMAKE_BUILD_TYPE STREQUAL Debug)
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++ -std=c++11 -DHC_ENABLE_LOGGING -g -Wextra -Wall -pedantic")
else ()
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++ -std=c++11 -O3")
endif (CMAKE_BUILD_TYPE STREQUAL Debug)

set (LIBVOIDMODULE_SRC  void.cpp)

# search for libs
find_package (LibHamcast REQUIRED)

include_directories (. ${HAMCAST_INCLUDE} )

add_library(libvoidmodule SHARED ${LIBVOIDMODULE_SRC})

target_link_libraries (libvoidmodule ${CMAKE_LD_LIBS} ${HAMCAST_LIBRARY})

# install lib
set (LIBVOIDMODULE_VERSION_MAJOR 0)
set (LIBVOIDMODULE_VERSION_MINOR 15)
set (LIBVOIDMODULE_VERSION_PATCH 1)
set(LIBRARY_VERSION ${LIBVOIDMODULE_VERSION_MAJOR}.${LIBVOIDMODULE_VERSION_MINOR}.${LIBVOIDMODULE_VERSION_PATCH})
set(LIBRARY_SOVERSION ${LIBVOIDMODULE_VERSION_MAJOR})
set_target_properties(libvoidmodule PROPERTIES
                        SOVERSION ${LIBRARY_SOVERSION}
                        VERSION ${LIBRARY_VERSION}
                        OUTPUT_NAME voidmodule)
