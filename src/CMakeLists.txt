set(mini18n_SOURCES
    mini18n.c
    mini18n-multi.c
    mini18n_pv_data.c
    mini18n_pv_file_po.c
    mini18n_pv_file_yts.c
    mini18n_pv_file_csv.c
    mini18n_pv_hash.c
    mini18n_pv_list.c)

add_definitions(-DBUILDING_DLL=1)

option(MINI18N_LOG "Enable the logging feature of mini18n" OFF)
if (MINI18N_LOG)
    add_definitions(-DMINI18N_LOG=1)
endif ()

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

add_library(mini18n STATIC ${mini18n_SOURCES})
