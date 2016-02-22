cmake_minimum_required(VERSION 3.2.2)
project(VAL)

set(VAL_VERSION_MAJOR 1)
set(VAL_VERSION_MINOR 0)

#configure_file(
#  "${PROJECT_SOURCE_DIR}/include/val/VAL_Config.h.in"
#  "${PROJECT_SOURCE_DIR}/include/val/VAL_Config.h"
#)

include_directories("${PROJECT_SOURCE_DIR}/include")
include_directories("${PROJECT_SOURCE_DIR}/src")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -Wextra -pedantic -Werror")

# libval instructions
add_custom_command(
  OUTPUT src/val/lex.yy.cc src/val/pddl+.cpp
  COMMAND flex++ -o src/val/lex.yy.cc src/pddl+.lex && bison src/pddl+.yacc -o src/val/pddl+.cpp
  DEPENDS src/pddl+.lex src/pddl+.yacc)
#source_group("val_files" REGULAR_EXPRESSION "src/val/.*\\.cpp")
add_library(val SHARED
  src/val/pddl+.cpp 
  src/val/Action.cpp
  src/val/Analysis.cpp
  src/val/CausalGraph.cpp
  src/val/DebugWriteController.cpp
  src/val/DYNA.cpp
  src/val/dynaMain.cpp
  src/val/DYNATranslator.cpp
  src/val/Environment.cpp
  src/val/Evaluator.cpp
  src/val/Events.cpp
  src/val/FastEnvironment.cpp
  src/val/FuncAnalysis.cpp
  src/val/FuncExp.cpp
  src/val/graphconstruct.cpp
  src/val/HowAnalyser.cpp
  src/val/HowWhatWhenMain.cpp
  src/val/instantiationConstraint.cpp
  src/val/instantiation.cpp
  src/val/instantiationIP.cpp
  src/val/instantiationMain.cpp
  src/val/InstPropLinker.cpp
  src/val/LaTeXSupport.cpp
  src/val/LPGP.cpp
  src/val/LPGPTranslator.cpp
  src/val/Ownership.cpp
  src/val/parse.cpp
  src/val/PartialPlan.cpp
  src/val/pddlplus.cpp
  src/val/PinguPlanGenerator.cpp
  src/val/PingusTranslator.cpp
  src/val/Plan.cpp
  src/val/PlanRec.cpp
  src/val/Polynomial.cpp
  src/val/PrettyPrinter.cpp
  src/val/Proposition.cpp
  src/val/ptree.cpp
  src/val/random.cpp
  src/val/Relax.cpp
  src/val/RelaxTranslator.cpp
  src/val/RepairAdvice.cpp
  src/val/RobustAnalyse.cpp
  src/val/SASActions.cpp
  src/val/SearchSpace.cpp
  src/val/SimpleEval.cpp
  src/val/State.cpp
  src/val/TIM.cpp
  src/val/TIMMain.cpp
  src/val/TimSupport.cpp
  src/val/ToFnMain.cpp
  src/val/ToFunction.cpp
  src/val/TrajectoryConstraints.cpp
  src/val/typecheck.cpp
  src/val/TypedAnalyser.cpp
  src/val/TypedAnalysis.cpp
  src/val/TypeStrip.cpp
  src/val/TypeStripWC.cpp
  src/val/Utils.cpp
  src/val/Validator.cpp)
