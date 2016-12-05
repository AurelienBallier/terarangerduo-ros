cmake_minimum_required(VERSION 2.8.3)
project(terarangerduo)

# search for everything we need to build the package
find_package(catkin REQUIRED COMPONENTS
  roscpp
  sensor_msgs
  std_msgs
  dynamic_reconfigure
)
# since we need boost search it as well
# find_package makes the ${..._INCLUDE_DIRS} ${..._LIBRARIES} variables we use later
find_package(Boost REQUIRED COMPONENTS system thread)

generate_dynamic_reconfigure_options(
  cfg/TerarangerDuo.cfg
)

# export the dependencis of this package for who ever depends on us
catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS roscpp sensor_msgs std_msgs dynamic_reconfigure
  DEPENDS boost
)

# tell catkin where to find the headers for this project
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
)

# make the executables
add_executable(${PROJECT_NAME} src/terarangerduo.cpp src/serial_port.cpp  )
add_dependencies(${PROJECT_NAME}  ${catkin_EXPORTED_TARGETS} )
add_dependencies(${PROJECT_NAME} ${PROJECT_NAME}_gencfg)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} ${Boost_LIBRARIES} )

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
