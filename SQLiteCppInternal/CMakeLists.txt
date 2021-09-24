
set(SQLITE_SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR})

# list of header files of the library
set(SQLITECPP_INC
        ${SQLITE_SRC_DIR}/include/SQLiteCpp/SQLiteCpp.h
        ${SQLITE_SRC_DIR}/include/SQLiteCpp/Assertion.h
        ${SQLITE_SRC_DIR}/include/SQLiteCpp/Backup.h
        ${SQLITE_SRC_DIR}/include/SQLiteCpp/Column.h
        ${SQLITE_SRC_DIR}/include/SQLiteCpp/Database.h
        ${SQLITE_SRC_DIR}/include/SQLiteCpp/Exception.h
        ${SQLITE_SRC_DIR}/include/SQLiteCpp/Statement.h
        ${SQLITE_SRC_DIR}/include/SQLiteCpp/Transaction.h
        ${SQLITE_SRC_DIR}/include/SQLiteCpp/VariadicBind.h
        ${SQLITE_SRC_DIR}/include/SQLiteCpp/ExecuteMany.h
        )

# list of sources files of the library
set(SQLITECPP_SRC
        ${SQLITE_SRC_DIR}/src/Backup.cpp
        ${SQLITE_SRC_DIR}/src/Column.cpp
        ${SQLITE_SRC_DIR}/src/Database.cpp
        ${SQLITE_SRC_DIR}/src/Exception.cpp
        ${SQLITE_SRC_DIR}/src/Statement.cpp
        ${SQLITE_SRC_DIR}/src/Transaction.cpp
        )

find_package (SQLite3 REQUIRED)
message(STATUS "Link to sqlite3 system library")

# add sources of the wrapper as a "SQLiteCpp" static library
add_library(SQLiteCpp ${SQLITECPP_SRC} ${SQLITECPP_INC})
target_include_directories(SQLiteCpp PUBLIC
        ${SQLITE_SRC_DIR}/include)

target_link_libraries(SQLiteCpp PUBLIC SQLite::SQLite3)
if(SQLite3_VERSION VERSION_LESS "3.19")
    set_target_properties(SQLiteCpp PROPERTIES COMPILE_FLAGS "-DSQLITECPP_HAS_MEM_STRUCT")
endif()

