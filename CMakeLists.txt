cmake_minimum_required(VERSION 3.1.2)
project(bzip2 C)

include(GNUInstallDirs)

if(MSVC OR MSVC90 OR MSVC10)
    set(MSVC ON)
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()

set(BZ2_NAMESPACE bzip2)
set(BZ2_CONFIG ${PROJECT_NAME}Config)

option(BZ2_BUILD_EXE OFF)

add_library(${PROJECT_NAME} blocksort.c
                            bzlib.c
                            compress.c
                            crctable.c
                            decompress.c
                            huffman.c
                            randtable.c
                            bzlib.h
                            bzlib_private.h)
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER bzlib.h)
set_target_properties(${PROJECT_NAME} PROPERTIES LIBRARY_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/bin/Release)
set_target_properties(${PROJECT_NAME} PROPERTIES LIBRARY_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/bin/Debug)
set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/bin/Release)
set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/bin/Debug)
set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/lib/Release)
set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/lib/Debug)
set_target_properties(${PROJECT_NAME} PROPERTIES PDB_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/bin/Release)
set_target_properties(${PROJECT_NAME} PROPERTIES PDB_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/bin/Debug)
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_PDB_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/bin/Release)
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_PDB_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/bin/Debug)
set_target_properties(${PROJECT_NAME} PROPERTIES OUTPUT_NAME ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_NAME ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_NAME ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES LIBRARY_OUTPUT_NAME ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES PDB_NAME ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_PDB_NAME ${PROJECT_NAME})

if(BZ2_BUILD_EXE)
    add_executable(${CMAKE_PROJECT_NAME} bzip2.c)
    target_link_libraries(${CMAKE_PROJECT_NAME} ${PROJECT_NAME})
    target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
endif()

install(TARGETS ${PROJECT_NAME} EXPORT bzip2
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_BINDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/bzip2)

if(BZ2_BUILD_EXE)
    install(TARGETS ${CMAKE_PROJECT_NAME}
            EXPORT ${BZ2_CONFIG}
            RUNTIME DESTINATION bin
            LIBRARY DESTINATION lib
            ARCHIVE DESTINATION lib)
endif()

install(EXPORT bzip2 NAMESPACE ${BZ2_NAMESPACE}:: DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME} FILE ${BZ2_CONFIG}.cmake)

if(MSVC)
	install(FILES ${CMAKE_BINARY_DIR}/bin/Debug/${PROJECT_NAME}.pdb DESTINATION ${CMAKE_INSTALL_LIBDIR}/Debug CONFIGURATIONS Debug)
endif(MSVC)
