cmake_minimum_required(VERSION 3.10)

# Set the project name and version
project(Probe VERSION 1.0)

# Specify C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)


# Define include paths and library paths
include_directories(${CMAKE_SOURCE_DIR}/src)
link_directories(${CMAKE_SOURCE_DIR}/src/parser-ff)

# Add compiler flags
add_definitions(-Wall -ansi -DNDEBUG -DENDEBUG -O3)

add_subdirectory(src)

# Specify the executable name and sources
set(SOURCES
    src/util/bitarray.cxx
    src/util/global_options.cxx
    src/util/options.cxx
    src/util/global_structures.cxx
    src/inference/prop_util.cxx
    src/inference/landmarks.cxx
    src/inference/precedence.cxx
    src/inference/helpful.cxx
    src/pddl/pddl_fluent_set.cxx
    src/pddl/pddl_string_table.cxx
    src/pddl/PDDL.cxx
    src/pddl/state.cxx
    src/app/main-probe.cxx
    src/search/search.cxx
    src/search/node.cxx
    src/search/focus_bfs.cxx
    src/heuristic/h_add_plus.cxx
    src/heuristic/h_add.cxx
    src/heuristic/h1.cxx
    src/heuristic/h_cls.cxx
    src/heuristic/heuristics.cxx
    src/heuristic/h2.cxx
)

# Define the executable
add_executable(probe ${SOURCES})

# Link libraries
target_link_libraries(probe ff pddl opspddl)

# Optional: Set the static linking flag (if required)
set_target_properties(probe PROPERTIES LINK_FLAGS "-static")
