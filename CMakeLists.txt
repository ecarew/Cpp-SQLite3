# CMakeLists.txt for SQLite3
#
#

cmake_minimum_required(VERSION 3.8)

project(SQLite3
    LANGUAGES C
    VERSION 3.36.0
)

add_definitions(-DSQLITE_ENABLE_RTREE)
add_definitions(-DSQLITE_ENABLE_FTS4)
add_definitions(-DSQLITE_ENABLE_FTS5)
add_definitions(-DSQLITE_ENABLE_JSON1)
add_definitions(-DSQLITE_ENABLE_RBU)
add_definitions(-DSQLITE_ENABLE_STAT4)

## Comment this line out for threaded variant ( Using single right now because threads gave me issues )
#add_definitions(-DSQLITE_THREADSAFE=0)

set(Sources
    src/shell.c
    src/sqlite3.c
)

set(Headers
    src/sqlite3.h
    src/sqlite3ext.h
)

set(StaticLibraries
)

set(SharedLibraries
    dl
    pthread
)

add_library(${PROJECT_NAME} SHARED ${Sources} ${Headers})

target_link_libraries(${PROJECT_NAME} ${SharedLibraries} ${StaticLibraries})

set_target_properties(${PROJECT_NAME} PROPERTIES
    FOLDER Libraries
    LINKER_LANGUAGE C
    SOVERSION ${PROJECT_VERSION_MAJOR}
    VERSION ${PROJECT_VERSION}
    LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib
)

install(TARGETS ${PROJECT_NAME}
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib/static
)

install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_BINARY_DIR}/lib)



target_include_directories(${PROJECT_NAME} PUBLIC src)
