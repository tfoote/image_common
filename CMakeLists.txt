cmake_minimum_required(VERSION 2.8)
project(image_common)
find_package(catkin REQUIRED)
catkin_stack()
catkin_project(image_common
)

# process folders in the order of dependencies
add_subdirectory(camera_calibration_parsers)
add_subdirectory(image_transport)
add_subdirectory(camera_info_manager)
add_subdirectory(polled_camera)
