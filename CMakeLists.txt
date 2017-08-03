cmake_minimum_required(VERSION 2.6)


include(CheckCCompilerFlag)

project(mini18n)

set (mini18n_VERSION_MAJOR 2)
set (mini18n_VERSION_MINOR 1)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules/")

set(mini18n_SOURCES
    src/mini18n.c
    src/mini18n-multi.c
    src/mini18n_pv_data.c
    src/mini18n_pv_file_po.c
    src/mini18n_pv_file_yts.c
    src/mini18n_pv_file_csv.c
    src/mini18n_pv_hash.c
    src/mini18n_pv_list.c)

if(WIN32)
    set(mini18n_SOURCES ${mini18n_SOURCES} mini18n_pv_conv_windows.c)
    add_definitions(-DHAVE_UTF16=1)
endif()

include(CheckFunctionExists)
check_function_exists(wcsdup HAVE_WCSDUP)
if (HAVE_WCSDUP)
    add_definitions(-DHAVE_WCSDUP=1)
endif ()

try_compile(HAVE_WCS_POINTERS ${CMAKE_BINARY_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/cmake/check_wcs_functions.c)

add_definitions("-Wall")
add_definitions(-DBUILDING_DLL=1)
add_library(mini18n STATIC ${mini18n_SOURCES})



