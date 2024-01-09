cmake_minimum_required(VERSION 3.1 FATAL_ERROR)
project(DBoW2)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 ")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3")

set(HDRS_DBOW2
  DBoW2/BowVector.h
  DBoW2/FORB.h 
  DBoW2/FClass.h       
  DBoW2/FeatureVector.h
  DBoW2/ScoringObject.h   
  DBoW2/TemplatedVocabulary.h)
set(SRCS_DBOW2
  DBoW2/BowVector.cpp
  DBoW2/FORB.cpp      
  DBoW2/FeatureVector.cpp
  DBoW2/ScoringObject.cpp)

set(HDRS_DUTILS
  DUtils/Random.h
  DUtils/Timestamp.h)
set(SRCS_DUTILS
  DUtils/Random.cpp
  DUtils/Timestamp.cpp)


find_package(OpenCV 4 QUIET)
if(NOT OpenCV_FOUND)
   find_package(OpenCV 3.0 QUIET)
   if(NOT OpenCV_FOUND)
      message(FATAL_ERROR "OpenCV > 3.0 not found.")
   endif()
endif()

set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

include_directories(${OpenCV_INCLUDE_DIRS})
add_library(DBoW2 SHARED ${SRCS_DBOW2} ${SRCS_DUTILS})
target_link_libraries(DBoW2 ${OpenCV_LIBS})

set_target_properties(DBoW2
    PROPERTIES PUBLIC_HEADER
    "include/DBoW2/BowVector.h;include/DBoW2/FORB.h;include/DBoW2/FClass.h;include/DBoW2/FeatureVector.h;include/DBoW2/ScoringObject.h;include/DBoW2/TemplatedVocabulary.h"
    )

install(TARGETS DBoW2
    EXPORT DBoW2Config
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/DBoW2
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    )

export(TARGETS
    DBoW2
    NAMESPACE DBoW2::
    FILE "${CMAKE_CURRENT_BINARY_DIR}/DBoW2Config.cmake"
)

install(EXPORT
    DBoW2Config
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/DBoW2/cmake"
    NAMESPACE DBoW2::
)
