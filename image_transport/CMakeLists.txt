project(image_transport)
catkin_project(${PROJECT_NAME} INCLUDE_DIRS include)

find_package(Boost)

find_package(ROS COMPONENTS message_filters rosconsole roscpp roslib)

find_library(pluginlib_poco poco_lite /opt/ros/fuerte/stacks/pluginlib/lib/)
find_library(pluginlib_boost boost_fs_wrapper /opt/ros/fuerte/stacks/pluginlib/lib/)
set(pluginlib_LIBRARIES ${pluginlib_poco} ${pluginlib_boost})
include_directories(/opt/ros/fuerte/stacks/pluginlib/include)
 

include_directories(SYSTEM ${pluginlib_INCLUDE_DIRS} ${roscpp_INCLUDE_DIRS})
include_directories(include)


# add a library
add_library(${PROJECT_NAME} src/publisher.cpp src/subscriber.cpp src/single_subscriber_publisher.cpp src/image_transport.cpp src/camera_common.cpp src/camera_publisher.cpp src/camera_subscriber.cpp src/manifest.cpp)
target_link_libraries(${PROJECT_NAME} ${BOOST_LIBRARIES}
                                      ${pluginlib_LIBRARIES}
)

install(TARGETS ${PROJECT_NAME}
        DESTINATION lib
        COMPONENT main
)

# add two execs
add_executable(republish src/republish.cpp)
target_link_libraries(republish ${PROJECT_NAME}
                                ${message_filters_LIBRARIES}
                                ${rosconsole_LIBRARIES}
                                ${roscpp_LIBRARIES}
                                ${roslib_LIBRARIES}
                                tinyxml
)

add_executable(list_transports src/list_transports.cpp)
target_link_libraries(list_transports ${PROJECT_NAME}
                                      ${rosconsole_LIBRARIES}
                                      ${roslib_LIBRARIES}
                                      tinyxml
)

install(TARGETS list_transports republish
        DESTINATION share/${PROJECT_NAME}/bin
)
