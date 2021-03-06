project(camera_info_manager)
catkin_project(${PROJECT_NAME} INCLUDE_DIRS include
                               LIBRARIES camera_info_manager
)

find_package(catkin COMPONENTS camera_calibration_parsers image_transport roscpp)

include_directories(SYSTEM ${roscpp_INCLUDE_DIRS}
                           ${image_transport_INCLUDE_DIRS}
                           ${camera_calibration_parsers_INCLUDE_DIRS}
)
include_directories(include)

# add a library
add_library(${PROJECT_NAME} src/camera_info_manager.cpp)

install(TARGETS ${PROJECT_NAME}
        DESTINATION ${CATKIN_PROJECT_LIB_DESTINATION}
        COMPONENT main
)
install(DIRECTORY include/
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
)

# unit test uses gtest, but needs rostest to create a ROS environment
catkin_add_gtest(unit_test tests/unit_test.cpp)
target_link_libraries(unit_test ${PROJECT_NAME})

if (FALSE)
rosbuild_add_rostest(tests/unit_test.test)
endif()
