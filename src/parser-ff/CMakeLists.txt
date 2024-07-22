####### Bison and Flex

find_package(BISON REQUIRED)
find_package(FLEX REQUIRED)

set(BISON_OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR})
set(FLEX_OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR})

FLEX_TARGET(pddl_lexer lex-fct_pddl.l ${CMAKE_CURRENT_BINARY_DIR}/lex-fct_pddl.c
            DEFINES_FILE ${CMAKE_CURRENT_BINARY_DIR}/lex-fct_pddl.h
            COMPILE_FLAGS "-Pfct_pddl")
BISON_TARGET(pddl_parser scan-fct_pddl.y ${CMAKE_CURRENT_BINARY_DIR}/scan-fct_pddl.tab.c
             COMPILE_FLAGS "-pfct_pddl -d -v")
add_flex_bison_dependency(pddl_lexer pddl_parser)

FLEX_TARGET(ops_pddl_lexer lex-ops_pddl.l ${CMAKE_CURRENT_BINARY_DIR}/lex-ops_pddl.c
            DEFINES_FILE ${CMAKE_CURRENT_BINARY_DIR}/lex-ops_pddl.h
            COMPILE_FLAGS "-Pops_pddl")
BISON_TARGET(ops_pddl_parser scan-ops_pddl.y ${CMAKE_CURRENT_BINARY_DIR}/scan-ops_pddl.tab.c
             COMPILE_FLAGS "-pops_pddl -d -v")
add_flex_bison_dependency(ops_pddl_lexer ops_pddl_parser)


####### Files


set(LIB_SOURCES
    libff.c
    memory.c
    output.c
    parse.c
    expressions.c
    inst_pre.c
    inst_easy.c
    inst_hard.c
    inst_final.c
    relax.c
    search.c
    utility.c
)

add_library(ff STATIC
            ${LIB_SOURCES}
            )
target_compile_options(ff PRIVATE ${CMAKE_C_FLAGS})




add_library(pddl STATIC
            ${FLEX_pddl_lexer_OUTPUTS}
            ${BISON_pddl_parser_OUTPUTS}
            )
target_compile_options(pddl PRIVATE ${CMAKE_C_FLAGS})

target_include_directories(pddl
    PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}  # Include the directory with generated files
)





add_library(opspddl STATIC
            ${FLEX_ops_pddl_lexer_OUTPUTS}
            ${BISON_ops_pddl_parser_OUTPUTS}
            )
target_compile_options(opspddl PRIVATE ${CMAKE_C_FLAGS})

target_include_directories(opspddl
    PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}  # Include the directory with generated files
)

####### Dependencies

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# add_dependencies(ff pddl_parser ops_pddl_parser)

# add_executable(test_lib test_main.cxx)
# target_link_libraries(test_lib libff)

