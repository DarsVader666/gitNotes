# ROS使用指南
## 1 基本操作
### 1.1基本初始化

**创建工作空间并初始化**

```
mkdir -p 自定义空间名称/src
cd 自定义空间名称
catkin_make
```


**进入src创建ros包并添加依赖**
```
cd src
catkin_create_pkg 自定义ROS包名 roscpp rospy std_msgs
```
std_msgs为标准信息库

### 1.2 CMakelists.txt编辑配置
文件CMakeLists.txt是CMake构建系统的输入，用于构建软件包。任何兼容CMake的软件包都包含一个或多个CMakeLists.txt文件，这些文件描述了如何构建代码以及将代码安装到何处。

```cmake
​cmake_minimum_required(VERSION 3.0.2) #所需 cmake 版本
project(demo01_hello_vscode) #包名称，会被 ${PROJECT_NAME} 的方式调用

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
#设置构建所需要的软件包
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
)


# 运行时依赖
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES demo01_hello_vscode
#  CATKIN_DEPENDS roscpp rospy std_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# 添加头文件路径，当前程序包的头文件路径位于其他文件路径之前
include_directories(
# include
  ${catkin_INCLUDE_DIRS}
)

## Declare a C++ library
# 声明 C++ 库
# add_library(${PROJECT_NAME}
#   src/${PROJECT_NAME}/demo01_hello_vscode.cpp
# )

## Add cmake target dependencies of the library
## as an example, code may need to be generated before libraries
## either from message generation or dynamic reconfigure
# 添加库的 cmake 目标依赖
# add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Declare a C++ executable
## With catkin_make all packages are built within a single CMake context
## The recommended prefix ensures that target names across packages don't collide
# 声明 C++ 可执行文件
add_executable(Hello_VSCode src/Hello_VSCode.cpp)

## Rename C++ executable without prefix
## The above recommended prefix causes long target names, the following renames the
## target back to the shorter version for ease of user use
## e.g. "rosrun someones_pkg node" instead of "rosrun someones_pkg someones_pkg_node"
#重命名c++可执行文件
# set_target_properties(${PROJECT_NAME}_node PROPERTIES OUTPUT_NAME node PREFIX "")

## Add cmake target dependencies of the executable
## same as for the library above
#添加可执行文件的 cmake 目标依赖
add_dependencies(Hello_VSCode ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
#指定库、可执行文件的链接库
target_link_libraries(Hello_VSCode
  ${catkin_LIBRARIES}
)

#############
## Install ##
## 安装 ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
#设置用于安装的可执行脚本
catkin_install_python(PROGRAMS
  scripts/Hi.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

```

简略版（关键点）:

```cmake
add_executable(源文件名
  src/源文件名.cpp
)
target_link_libraries(源文件名
  ${catkin_LIBRARIES}
)
```
### 1.3 package.xml编辑配置
```xml
​<?xml version="1.0"?>
<!-- 格式: 以前是 1，推荐使用格式 2 -->
<package format="2">
  <!-- 包名 -->
  <name>demo01_hello_vscode</name>
  <!-- 版本 -->
  <version>0.0.0</version>
  <!-- 描述信息 -->
  <description>The demo01_hello_vscode package</description>

  <!-- One maintainer tag required, multiple allowed, one person per tag -->
  <!-- Example:  -->
  <!-- <maintainer email="jane.doe@example.com">Jane Doe</maintainer> -->
  <!-- 维护人员 -->
  <maintainer email="xuzuo@todo.todo">xuzuo</maintainer>


  <!-- One license tag required, multiple allowed, one license per tag -->
  <!-- Commonly used license strings: -->
  <!--   BSD, MIT, Boost Software License, GPLv2, GPLv3, LGPLv2.1, LGPLv3 -->
  <!-- 许可证信息，ROS核心组件默认 BSD -->
  <license>TODO</license>


 
  <!-- 依赖的构建工具，这是必须的 -->
  <buildtool_depend>catkin</buildtool_depend>

  <!-- 指定构建此软件包所需的软件包 -->
  <build_depend>roscpp</build_depend>
  <build_depend>rospy</build_depend>
  <build_depend>std_msgs</build_depend>

  <!-- 指定根据这个包构建库所需要的包 -->
  <build_export_depend>roscpp</build_export_depend>
  <build_export_depend>rospy</build_export_depend>
  <build_export_depend>std_msgs</build_export_depend>

  <!-- 运行该程序包中的代码所需的程序包 -->  
  <exec_depend>roscpp</exec_depend>
  <exec_depend>rospy</exec_depend>
  <exec_depend>std_msgs</exec_depend>


  <!-- The export tag contains other, unspecified, tags -->
  <export>
    <!-- Other tools can request additional information be placed here -->

  </export>
</package>
```
### 1.4 编译文件

```
cd 自定义空间名称
catkin_make
```
生成build, devel文件夹...
### 1.5 运行文件
**启动命令行1：**
```
roscore
```
**再启动命令行2**
```
cd 工作空间
source ./devel/setup.bash
rosrun 包名 C++节点
```
可将`source`环节添加进`.bashrc`
```
echo "source ~/工作空间/devel/setup.bash" >> ~/.bashrc
```

## 2 launch文件编写
1. 选定功能包右击，添加launch文件夹
2. 选定launch文件夹右击，添加launch文件
3. 编辑launch文件内容
```
<launch>
    <node pkg="helloworld" type="demo_hello" name="hello" output="screen" />
    <node pkg="turtlesim" type="turtlesim_node" name="t1"/>
    <node pkg="turtlesim" type="turtle_teleop_key" name="key1" />
</launch>
```
* node->包含的某个节点
* pkg->功能包
* type->被运行的节点文件
* name->为节点命名
* output->设置日志的输出目标

4. 运行launch文件
   `roslaunch 包名 launch文件名`

## 3 ROS文件系统相关命令
1. 增

    `$ catkin_create_pkg 自定义包名 依赖包` -> 创建新的ROS功能包

    `$ sudo apt install xxx` -> 安装ROS功能包

2. 删

    `$sudo apt purge xxx` -> 删除某个功能包

3. 查

    `$ rospack list` -> 列出所有功能包

    `$ rospack find 包名` -> 查找某个功能包是否存在，如果存在返回安装路径

    `$ roscd 包名` -> 进入某个功能包

    `$ rosls 包名` -> 列出某个包下的文件

    `$ apt search xxx` -> 搜索某个功能包、
4. 改
   `$ rosed 包名 文件名` -> 修改功能包文件\
   需要安装 vim
5. 执行\
   5.1 roscore

   roscre是ROS的系统先决条件节点和程序的集合，必须运行人roscore才能使ROS节点进行通信。
   
   roscore将启动：
   * ros master
   * ros 参数服务器
   * rosout 日志节点
   
   用法：
   ```
   roscore
   ```
   或（指定端口号）
   ```
   roscore -p xxxx
   ```
   5.2 rosrun
   `$ rosrun 包名 可执行文件名` -> 运行指定的ROS节点

   比如：`$ rosrun turtlesim turtlesim_node`
   
   5.3 roslaunch
   `$ roslaunch 包名 launch文件名` -> 执行某个包下的launch文件
   
   5.4 `$ rqt_graph`或`$ rosrun rqt_graph rqt_graph`，可以看到节点关系的网络拓扑图