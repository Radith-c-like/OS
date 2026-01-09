cmake_minimum_required(VERSION 3.10)
project(NumberLibrary VERSION 1.0.0 LANGUAGES CXX)

# Настройка стандарта C++
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Создание статической библиотеки
add_library(Number STATIC
    Number.cpp
    Number.h
)

# Настройка свойств библиотеки
target_include_directories(Number
    PUBLIC 
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:include>
)

# Если нужна поддержка экспорта/импорта в Windows
if(WIN32)
    target_compile_definitions(Number PRIVATE NUMBER_EXPORTS)
endif()

# Установка библиотеки
install(TARGETS Number
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

install(FILES Number.h DESTINATION include)