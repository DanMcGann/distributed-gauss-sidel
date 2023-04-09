###########################################################################
# Find GTSAM
find_package(GTSAM REQUIRED)

###########################################################################
# Find Boost
find_package(Boost COMPONENTS serialization filesystem system program_options thread REQUIRED)

###########################################################################
# Distributed Mapper
file(GLOB srcs "src/*.cpp" "src/*.h")
add_library(distributed-mapper ${srcs})
target_link_libraries(distributed-mapper gtsam)
target_include_directories(distributed-mapper PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/src")

###########################################################################
# Add executable
add_executable(runDistributedMapper scripts/runDistributedMapper.cpp)
target_link_libraries(runDistributedMapper distributed-mapper ${Boost_LIBRARIES} gtsam)

###########################################################################
# Tests
option(BUILD_DGS_TESTS "If True builds tests for DGS" OFF)
if(${BUILD_DGS_TESTS})
    set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH}" "${GTSAM_DIR}/../GTSAMCMakeTools")
    include(GtsamBuildTypes)
    include(GtsamTesting)
    add_subdirectory(tests)
endif()



