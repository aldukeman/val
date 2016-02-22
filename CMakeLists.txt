cmake_minimum_required(VERSION 3.2.2)
project(VAL)

set(VAL_VERSION_MAJOR 1)
set(VAL_VERSION_MINOR 0)

#configure_file(
#  "${PROJECT_SOURCE_DIR}/include/val/VAL_Config.h.in"
#  "${PROJECT_SOURCE_DIR}/include/val/VAL_Config.h"
#)

include_directories("${PROJECT_SOURCE_DIR}/include/val")
include_directories("${PROJECT_SOURCE_DIR}/src")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -pedantic -Werror")

# libval instructions
add_custom_command(
  OUTPUT src/val/lex.yy.cc src/val/pddl+.cpp
  COMMAND flex++ -o src/val/lex.yy.cc src/pddl+.lex && bison src/pddl+.yacc -o src/val/pddl+.cpp
  DEPENDS src/pddl+.lex src/pddl+.yacc)
#source_group("val_files" REGULAR_EXPRESSION "src/val/.*\\.cpp")
add_library(val SHARED
  src/val/pddl+.cpp 
  src/val/Action.cpp
  src/val/DebugWriteController.cpp
  src/val/Environment.cpp
  src/val/Events.cpp
  src/val/FuncExp.cpp
  src/val/LaTeXSupport.cpp
  src/val/Ownership.cpp
  src/val/Plan.cpp
  src/val/Polynomial.cpp
  src/val/PrettyPrinter.cpp
  src/val/Proposition.cpp
  src/val/ptree.cpp
  src/val/random.cpp
  src/val/RepairAdvice.cpp
  src/val/RobustAnalyse.cpp
  src/val/State.cpp
  src/val/TrajectoryConstraints.cpp
  src/val/typecheck.cpp
  src/val/Utils.cpp
  src/val/Validator.cpp)
