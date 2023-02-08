cmake_minimum_required(VERSION 3.8)
project(cpp_pubsub)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()


# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(OpenCV REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(image_transport)

include_directories(${OpenCV_INCLUDE_DIRS}) 

##下面是输出信息
message(STATUS "Opnecv ;ibrary status: ")
message(STATUS "> version: ${OpenCV_VERSION} ")
message(STATUS "libraries: ${OpenCV_LIBS} ")
message(STATUS "> include: ${OpenCV_INCLUDE_DIRS}  ")

# 将某个cpp文件添加为可执行文件并增加一个talker变量
add_executable(talker src/publisher_member_function.cpp)
# 将查找的这些包添加到这个可执行文件中
ament_target_dependencies(talker rclcpp std_msgs sensor_msgs cv_bridge OpenCV)
# 同上
add_executable(listener src/subscriber_member_function.cpp)
ament_target_dependencies(listener rclcpp std_msgs sensor_msgs cv_bridge OpenCV image_transport)   
#

# add the install(TARGETS…) section so ros2 run can find your executable
install(TARGETS
  talker
  listener
  DESTINATION lib/${PROJECT_NAME})
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

ament_package()
