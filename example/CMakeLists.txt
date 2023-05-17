cmake_minimum_required(VERSION 3.1.3)
project(TestCMakeWarnEverything C CXX)

set(W_LARGER_THAN_VAL 8192)
set(W_STACK_USAGE_VAL 8192)
set(W_FRAME_LARGER_THAN_VAL 8192)
set(W_ALLOC_SIZE_LARGE_THAN_VAL 8192)
set(W_ALLOCA_LARGER_THAN_VAL 8192)
set(W_WVLA_LARGER_THAN_VAL 8192)
# set(W_ABI_VAL 2)

include(CheckCCompilerFlag)
include(CheckCXXCompilerFlag)


if (DEFINED F_ABI_VERSION)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fabi-version=${F_ABI_VERSION}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fabi-version=${F_ABI_VERSION}")
endif()

if (DEFINED F_ABI_COMPAT_VERSION)
  set(RESULT_CACHE_ID_CXX_ACV "${CMAKE_CXX_COMPILER_ID}_CXX_HAS_WARNING_ACV")
  check_cxx_compiler_flag("-fabi-compat-version=2" ${RESULT_CACHE_ID_CXX_ACV})
  if(${RESULT_CACHE_ID_CXX_ACV})
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fabi-compat-version=${F_ABI_COMPAT_VERSION}")
  else()
    unset(F_ABI_COMPAT_VERSION)
  endif()
endif()

set(RESULT_CACHE_ID_CXX_SFA "${CMAKE_CXX_COMPILER_ID}_CXX_HAS_WARNING_SFA")
set(RESULT_CACHE_ID_C_SFA "${CMAKE_C_COMPILER_ID}_C_HAS_WARNING_SFA")

check_cxx_compiler_flag("-fstrict-flex-arrays=1" ${RESULT_CACHE_ID_CXX_SFA})
check_c_compiler_flag("-fstrict-flex-arrays=1" ${RESULT_CACHE_ID_C_SFA})
if(${RESULT_CACHE_ID_CXX_SFA})
  set(F_STRICT_FLEX_ARRAYS 1)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fstrict-flex-arrays=1")
endif()
if(${RESULT_CACHE_ID_C_SFA})
  set(F_STRICT_FLEX_ARRAYS 1)
  set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -fstrict-flex-arrays=1")
endif()

include(../cmake/warn-everything.cmake)

warn_everything(WARN_EVERYTHING_CXX WARN_EVERYTHING_C)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${WARN_EVERYTHING_C}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${WARN_EVERYTHING_CXX}")

add_library(c_test_lib STATIC test.c)
add_library(cxx_test_lib STATIC test.cc)
