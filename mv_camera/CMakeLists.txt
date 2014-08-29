cmake_minimum_required(VERSION 2.8.3)
project(mv_camera)

find_package(
    catkin REQUIRED COMPONENTS
    tf roscpp camera_info_manager driver_base dynamic_reconfigure image_transport 
    nodelet message_generation
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES mv_camera_driver mv_camera_nodelet 
  CATKIN_DEPENDS tf roscpp camera_info_manager driver_base dynamic_reconfigure image_transport 
    nodelet message_runtime
  DEPENDS system_lib
)


## Generate services in the 'srv' folder
add_service_files(
  FILES
  PropertyMap.srv
)

## Generate added messages and services with any dependencies listed here
generate_messages()


include_directories(include)
include_directories(${catkin_INCLUDE_DIRS})
link_libraries(${catkin_LIBRARIES})

# common source files for both node and nodelet driver interfaces
set(DRIVER_SOURCES 
  src/driver_mv_camera.cpp
  src/dev_mv_camera.cpp
  src/features.cpp
  src/formats.cpp
)
add_executable(mv_camera_interface src/driver_interface_node.cpp)
add_library(mv_camera_driver ${DRIVER_SOURCES})

add_executable(mv_camera_node src/mv_camera_node.cpp )
target_link_libraries(mv_camera_node  mv_camera_driver)

add_library(mv_camera_nodelet src/mv_camera_nodelet.cpp)
target_link_libraries(mv_camera_nodelet mv_camera_driver)
