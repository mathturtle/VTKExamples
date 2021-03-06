project (${WIKI}Plotting)

if(NOT WikiExamples_BINARY_DIR)
find_package(VTK REQUIRED)
include(${VTK_USE_FILE})
endif()

set(KIT_LIBS ${VTK_LIBRARIES})

#
# Build all .cxx files in the directory
file(GLOB ALL_FILES *.cxx)

include(${WikiExamples_SOURCE_DIR}/CMake/RequiresVersion.cmake)

set(VERSION_MIN "7.0")
Requires_Version(BoxChart ${VERSION_MIN} ALL_FILES)
Requires_Version(StackedPlot ${VERSION_MIN} ALL_FILES)
Requires_Version(StackedBar ${VERSION_MIN} ALL_FILES)

foreach(SOURCE_FILE ${ALL_FILES})
  string(REPLACE ".cxx" "" TMP ${SOURCE_FILE})
  string(REPLACE ${CMAKE_CURRENT_SOURCE_DIR}/ "" EXAMPLE ${TMP})
  add_executable(${WIKI}${EXAMPLE} ${EXECUTABLE_FLAG} ${EXAMPLE}.cxx)
  target_link_libraries(${WIKI}${EXAMPLE} ${KIT_LIBS})
endforeach()

if (BUILD_TESTING)
# Testing
set(KIT Plotting)
set(NEEDS_ARGS
  HistogramBarChart
  LinePlot
)
set(DATA ${WikiExamples_SOURCE_DIR}/src/Testing/Data)

add_test(${KIT}-HistogramBarChart ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${KIT}CxxTests
  TestHistogramBarChart ${DATA}/Pileated.jpg)

add_test(${KIT}-LinePlot ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${KIT}CxxTests
  TestLinePlot -E 50)

include(${WikiExamples_SOURCE_DIR}/CMake/ExamplesTesting.cmake)
endif()
