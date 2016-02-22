cmake_minimum_required(VERSION 3.2.2)
project(VAL)

set(VAL_VERSION_MAJOR 1)
set(VAL_VERSION_MINOR 0)

configure_file(
  "${PROJECT_SOURCE_DIR}/include/val/VAL_Config.h.in"
  "${PROJECT_SOURCE_DIR}/include/val/VAL_Config.h"
)

include_directories("${PROJECT_SOURCE_DIR}/include")
include_directories("${PROJECT_SOURCE_DIR}/src")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -Wextra -pedantic -Werror")

# libval instructions
add_custom_command(
  OUTPUT src/val/lex.yy.cc src/val/pddl+.cpp
  COMMAND flex++ -o src/val/lex.yy.cc src/pddl+.lex && bison src/pddl+.yacc -o src/val/pddl+.cpp
  DEPENDS src/pddl+.lex src/pddl+.yacc)
#add_custom_command(
#  OUTPUT src/val/pddl+.cpp
#  COMMAND bison src/pddl+.yacc -o src/val/pddl+.cpp
#  DEPENDS src/pddl+.yacc)
source_group(val_files REGULAR_EXPRESSION src/val/*.cpp)
add_library(val SHARED src/val/pddl+.cpp src/val/lex.yy.cc ${val_files})
