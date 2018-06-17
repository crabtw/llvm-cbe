if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
  project(llvm-cbe)
  cmake_minimum_required(VERSION 3.4.3)

  find_program(LLVM_CONFIG_PATH "llvm-config" DOC "Path to llvm-config binary")
  if(NOT LLVM_CONFIG_PATH)
    message(FATAL_ERROR "llvm-config not found: specify LLVM_CONFIG_PATH")
  endif()

  execute_process(COMMAND "${LLVM_CONFIG_PATH}"
                          "--cmakedir"
                  RESULT_VARIABLE HAD_ERROR
                  OUTPUT_VARIABLE LLVM_CMAKE_PATH
                  OUTPUT_STRIP_TRAILING_WHITESPACE)
  if(HAD_ERROR)
    message(FATAL_ERROR "llvm-config failed with status ${HAD_ERROR}")
  endif()

  list(APPEND CMAKE_MODULE_PATH "${LLVM_CMAKE_PATH}")

  include(LLVMConfig)
  include(HandleLLVMOptions)
  include(AddLLVM)

  include_directories(${LLVM_INCLUDE_DIRS})
  link_directories(${LLVM_LIBRARY_DIRS})
endif()

add_subdirectory(lib)
add_subdirectory(tools)
