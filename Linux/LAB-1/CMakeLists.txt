cmake_minimum_required(VERSION 3.10)
project(VectorLibrary VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)


option(GENERATE_EXPORT_HEADER "Generate export header for shared library" ON)

add_library(Vector SHARED
    Vector.cpp
    Vector.h
)


target_include_directories(Vector
    PUBLIC 
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:include>
)


if(WIN32)
    target_compile_definitions(Vector
        PRIVATE VECTOR_EXPORTS
        PUBLIC
            $<$<CONFIG:Debug>:_DEBUG>
    )
endif()


if(UNIX)
    set_target_properties(Vector PROPERTIES
        VERSION ${PROJECT_VERSION}
        SOVERSION 1
    )
endif()


install(TARGETS Vector
    EXPORT VectorTargets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(FILES Vector.h DESTINATION include)
install(EXPORT VectorTargets
    FILE VectorConfig.cmake
    NAMESPACE Vector::
    DESTINATION lib/cmake/Vector
)