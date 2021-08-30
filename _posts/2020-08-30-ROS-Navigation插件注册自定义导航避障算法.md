---
title: ROS Navigation插件注册自定义导航避障算法
layout: post
post-image: "https://raw.githubusercontent.com/thedevslot/WhatATheme/master/assets/images/What%20is%20Jekyll%20and%20How%20to%20use%20it.png?token=AHMQUELVG36IDSA4SZEZ5P26Z64IW"
description: Jekyll is a static site generator. You give it text written in your favorite
  markup language and it uses layouts to create a static website.
tags:
- ROS
- 导航避障
- 仿真环境
---
# 前言
最近开组会的时候，导师催促我寻找创新点，着实让我头疼。因为说实话，我真的不想找什么创新点，我只想学习一些招聘简历上的技能类的东西，比如熟悉A*、Dijkstra和DWA导航避障算法，熟悉ROS,掌握C++、python和MATLAB。当然看起来没什么难度，其实我拒绝的不是创新，而是一上来就奔着创新而创新的念头，会让我的学习非常的不主动，我想把基础的东西学习完，然后再去创新改进。我以后是工作啊，就算发几篇文章，以后也不想搞学术，出了校门啥技能都不会，岂不是凉了，太没安全感了。但是人嘛总不能随意按照自己的意愿去做事，还是要毕业的不是。说到底还是因为自己菜QAQ，能力跟不上，没有那么多精力和时间，硕士三年一眨眼也就过去了。看周围很多同学在算法的创新，大多是基于MATLAB仿真，或者......，我不想那样，我想我的毕业论文，既然研究方向是机器人导航避障方向，就把ROS、C++学好,熟悉导航基本框架后，在框架的基础上加入自己的算法，然后去改进创新，仿真，在实际的机器人平台运行出自己的算法效果。这样可能对别人来说总觉得没必要，但是会让我有安全感。正好最近查阅文献在找创新点，如何在ROS中实现自己的算法时候，了解到 **ROS插件注册**这个概念，就又去搜了很多网站跟文献资料，在这正好梳理整理一下，方便以后自己查阅参考。 
# 1.ROS编写自定义全局路径规划——创客制造
[ROS与navigation教程-编写自定义全局路径规划](https://www.ncnynl.com/archives/201708/1887.html) 
在百度最先搜到这篇文章，是中文版的ROSwiki,本人英语不好，hhhhh,看完没太懂，只对插件注册有点印象，于是......
# 2.ROS的pluginlib的理解与实例——土豆西瓜大芝麻
[ROS的pluginlib的理解与实例](https://blog.csdn.net/jinking01/article/details/79414486)
看了这篇CSDN,然后按照博客主“2.5 Step by Step”编写一遍插件注册，熟悉了插件注册的流程。  
然后，又去看了[ROS与navigation教程-编写自定义全局路径规划](https://www.ncnynl.com/archives/201708/1887.html)文章，思路清晰了很多。
# 3.学完准备自己动手尝试
[为ROS navigation功能包添加自定义的全局路径规划器(Global Path Planner)](https://blog.csdn.net/bluewhalerobot/article/details/73771020?utm_source=blogxgwz4)-BWBOT
拷贝-修改！！！！！   
简单粗暴老哥稳niubility！  
按照这几个例程完成了插件注册。   
![在这里插入图片描述](https://csdn-img-blog.oss-cn-beijing.aliyuncs.com/9af867248b43445094327434637fd0a5.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpX3NodWk=,size_16,color_FFFFFF,t_70#pic_center) 


# 4.测试插件
[TurtleBot3安装](https://www.cnblogs.com/raina/p/12162435.html)
[运行例程](https://www.freesion.com/article/71381376096/)
想起当时自己下过TurtleBot3的仿真环境想着能不能试试，在这个仿真环境里试试。 
## 4.1查看TurtleBot3 move_base配置文件

```bash
roscd turtlebot3_navigation/
ls
see launch/
```
![在这里插入图片描述](https://csdn-img-blog.oss-cn-beijing.aliyuncs.com/56c70dd65dd84eb5bf8f5402b8a782b8.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpX3NodWk=,size_16,color_FFFFFF,t_70)  
在launch目录下打开终端输入  
```bash
sudo gedit move_base.launch  
```
可以看到 
```xml
<launch>
  <!-- Arguments -->
  <arg name="model" default="$(env TURTLEBOT3_MODEL)" doc="model type [burger, waffle, waffle_pi]"/>
  <arg name="cmd_vel_topic" default="/cmd_vel" />
  <arg name="odom_topic" default="odom" />
  <arg name="move_forward_only" default="false"/>

  <!-- move_base -->
  <node pkg="move_base" type="move_base" respawn="false" name="move_base" output="screen">
    <param name="base_local_planner" value="dwa_local_planner/DWAPlannerROS" />
    <rosparam file="$(find turtlebot3_navigation)/param/costmap_common_params_$(arg model).yaml" command="load" ns="global_costmap" />
    <rosparam file="$(find turtlebot3_navigation)/param/costmap_common_params_$(arg model).yaml" command="load" ns="local_costmap" />
    <rosparam file="$(find turtlebot3_navigation)/param/local_costmap_params.yaml" command="load" />
    <rosparam file="$(find turtlebot3_navigation)/param/global_costmap_params.yaml" command="load" />
    <rosparam file="$(find turtlebot3_navigation)/param/move_base_params.yaml" command="load" />
    <rosparam file="$(find turtlebot3_navigation)/param/dwa_local_planner_params_$(arg model).yaml" command="load" />
    <remap from="cmd_vel" to="$(arg cmd_vel_topic)"/>
    <remap from="odom" to="$(arg odom_topic)"/>
    <param name="DWAPlannerROS/min_vel_x" value="0.0" if="$(arg move_forward_only)" />
  </node>
</launch>
```
可以看到没有global_planner的参数，先不管，先运行一下。 
## 4.2运行仿真环境

```bash
roslaunch turtlebot3_gazebo turtlebot3_world.launch
roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=$HOME/map.yaml
```
如果没有地图看[运行例程](https://www.freesion.com/article/71381376096/) 
通过按键控制建图，保存。再运行仿真环境。

![在这里插入图片描述](https://csdn-img-blog.oss-cn-beijing.aliyuncs.com/7ff1476a690547f8afb0a2a70c90b6f3.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpX3NodWk=,size_16,color_FFFFFF,t_70) 

![在这里插入图片描述](https://csdn-img-blog.oss-cn-beijing.aliyuncs.com/7d9c2741967349acaabf1960fcb92bb1.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpX3NodWk=,size_16,color_FFFFFF,t_70) 

其中运行roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=$HOME/map.yaml时终端会显示很多信息 

![在这里插入图片描述](https://img-blog.csdnimg.cn/daf452f57b08472d98690771339ee280.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpX3NodWk=,size_16,color_FFFFFF,t_70) 

可以看到对于planner只有base_local_planner: dwa_local_planner ... 信息没有global_planner信息  
我们设置终点让其运动看一下效果。 
[原始效果](http://static.zybuluo.com/RockWang-/olplhcittruhulh1roh5ihk7/%E5%8E%9F%E5%A7%8B%E6%95%88%E6%9E%9C.mp4)

## 4.3修改global_planner

```xml
<launch>
  <!-- Arguments -->
  <arg name="model" default="$(env TURTLEBOT3_MODEL)" doc="model type [burger, waffle, waffle_pi]"/>
  <arg name="cmd_vel_topic" default="/cmd_vel" />
  <arg name="odom_topic" default="odom" />
  <arg name="move_forward_only" default="false"/>

  <!-- move_base -->
  <node pkg="move_base" type="move_base" respawn="false" name="move_base" output="screen">
    <param name="base_local_planner" value="dwa_local_planner/DWAPlannerROS" />
    <rosparam file="$(find turtlebot3_navigation)/param/costmap_common_params_$(arg model).yaml" command="load" ns="global_costmap" />
    <rosparam file="$(find turtlebot3_navigation)/param/costmap_common_params_$(arg model).yaml" command="load" ns="local_costmap" />
    <rosparam file="$(find turtlebot3_navigation)/param/local_costmap_params.yaml" command="load" />
    <rosparam file="$(find turtlebot3_navigation)/param/global_costmap_params.yaml" command="load" />
    <rosparam file="$(find turtlebot3_navigation)/param/move_base_params.yaml" command="load" />
    <rosparam file="$(find turtlebot3_navigation)/param/dwa_local_planner_params_$(arg model).yaml" command="load" />
    <remap from="cmd_vel" to="$(arg cmd_vel_topic)"/>
    <remap from="odom" to="$(arg odom_topic)"/>
    <param name="DWAPlannerROS/min_vel_x" value="0.0" if="$(arg move_forward_only)" />
  </node>
</launch>
```
在    
```
<param name="base_local_planner" value="dwa_local_planner/DWAPlannerROS" /> 
```
附近添加    
```
<param name="base_global_planner" value="global_planner/GlobalPlanner"/>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/460765afc07c4006b3a973e206755df8.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpX3NodWk=,size_16,color_FFFFFF,t_70#pic_center)  
可以看到global_planner信息加载进了，运行仿真看一下效果 
[global_planner效果](http://static.zybuluo.com/RockWang-/ybwp7ywpc2qr9gg47czn5fk6/global_planner.mp4) 

可以看出，路径有了些许不一样，可以查阅ROS Navigation包中 navfn/NavfnROS 与global_planner/GlobalPlanner区别 
[ROS: global_planner 整体解析](https://heyijia.blog.csdn.net/article/details/45030929)——白巧克力亦唯心 

## 4.4添加test_global_planner
同样在    
```
<param name="base_local_planner" value="dwa_local_planner/DWAPlannerROS" />
```
附近添加    
```
 <param name="base_global_planner" value="test_planner/testPlanner"/>
```
删除之前添加的 
```
<param name="base_global_planner" value="global_planner/GlobalPlanner"/>
```

```xml
<launch>
  <!-- Arguments -->
  <arg name="model" default="$(env TURTLEBOT3_MODEL)" doc="model type [burger, waffle, waffle_pi]"/>
  <arg name="cmd_vel_topic" default="/cmd_vel" />
  <arg name="odom_topic" default="odom" />
  <arg name="move_forward_only" default="false"/>

  <!-- move_base -->
  <node pkg="move_base" type="move_base" respawn="false" name="move_base" output="screen">
    <param name="base_local_planner" value="dwa_local_planner/DWAPlannerROS" />
 	<param name="base_global_planner" value="test_planner/testPlanner"/>
    <rosparam file="$(find turtlebot3_navigation)/param/costmap_common_params_$(arg model).yaml" command="load" ns="global_costmap" />
    <rosparam file="$(find turtlebot3_navigation)/param/costmap_common_params_$(arg model).yaml" command="load" ns="local_costmap" />
    <rosparam file="$(find turtlebot3_navigation)/param/local_costmap_params.yaml" command="load" />
    <rosparam file="$(find turtlebot3_navigation)/param/global_costmap_params.yaml" command="load" />
    <rosparam file="$(find turtlebot3_navigation)/param/move_base_params.yaml" command="load" />
    <rosparam file="$(find turtlebot3_navigation)/param/dwa_local_planner_params_$(arg model).yaml" command="load" />
    <remap from="cmd_vel" to="$(arg cmd_vel_topic)"/>
    <remap from="odom" to="$(arg odom_topic)"/>
    <param name="DWAPlannerROS/min_vel_x" value="0.0" if="$(arg move_forward_only)" />
  </node>
</launch> 
```
重新运行仿真环境 

```bash
roslaunch turtlebot3_gazebo turtlebot3_world.launch
roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=$HOME/map.yaml
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a991ae2ae5ec4a9fafd5a19b285fad1f.png)  
可以看到test_planner加载成功 
运行看仿真效果  
[test_planner效果](http://static.zybuluo.com/RockWang-/n9xdvxpeyli99fj8ipavy4z9/test_planner.mp4)  
可以看到全局路径变化明显为直线，这正是我们直接拷贝carrot_planner包源文件的规划算法内容（全局规划器最小实现例程内容）  
到此我们在TurtleBot3仿真环境中实现了自定义全局规划的注册和使用，下一步就可以进一步研究源码.cpp文件，学习如何编写自己的算法注册到ROS中进行仿真试验了。   
# 5.总结
插件注册步骤:直接copy的原文链接[ROS的pluginlib的理解与实例](https://blog.csdn.net/jinking01/article/details/79414486)------土豆西瓜大芝麻 

防止原文链接丢失 

1.在～/catkin_ws/src/目录下 

```bash
catkin_create_pkg plugin_test roscpp pluginlib 
```

2.在include/plugin_test文件夹下新建文件polygon_base.h, 将下述代码拷贝进去, 申明我们的基类. 

```cpp
    #ifndef PLUGINLIB_TUTORIALS__POLYGON_BASE_H_
    #define PLUGINLIB_TUTORIALS__POLYGON_BASE_H_
 
    namespace polygon_base
    {
      class RegularPolygon
      {
        public:
          virtual void initialize(double side_length) = 0;
          virtual double area() = 0;
          virtual ~RegularPolygon(){}
 
        protected:
          RegularPolygon(){}
      };
    };
    #endif 
``` 
3.在include/plugin_test文件夹下新建文件polygon_plugins.h, 将下述代码拷贝进去, 申明我们的插件: 

```cpp
    #ifndef PLUGINLIB_TUTORIALS__POLYGON_PLUGINS_H_
    #define PLUGINLIB_TUTORIALS__POLYGON_PLUGINS_H_
    #include <plugin_test/polygon_base.h>
    #include <cmath>
 
    namespace polygon_plugins
    {
      class Triangle : public polygon_base::RegularPolygon
      {
        public:
          Triangle(){}
 
          void initialize(double side_length)
          {
            side_length_ = side_length;
          }
 
          double area()
          {
            return 0.5 * side_length_ * getHeight();
          }
 
          double getHeight()
          {
            return sqrt((side_length_ * side_length_) - ((side_length_ / 2) * (side_length_ / 2)));
          }
 
        private:
          double side_length_;
      };
 
      class Square : public polygon_base::RegularPolygon
      {
        public:
          Square(){}
 
          void initialize(double side_length)
          {
            side_length_ = side_length;
          }
 
          double area()
          {
            return side_length_ * side_length_;
          }
 
        private:
          double side_length_;
 
      };
    };
    #endif
```

4.在src文件夹下创建文件polygon_plugins.cpp, 并拷贝下述代码进去, 注册我们的插件. 


```cpp
    #include <pluginlib/class_list_macros.h>
    #include <plugin_test/polygon_base.h>
    #include <plugin_test/polygon_plugins.h>
 
    PLUGINLIB_EXPORT_CLASS(polygon_plugins::Triangle, polygon_base::RegularPolygon)
    PLUGINLIB_EXPORT_CLASS(polygon_plugins::Square, polygon_base::RegularPolygon) 
```
5.在CMakeLists.txt文件中, 加入下述add_library申明. 值得注意的是, 在CMakeLists.txt文件中, 需要在include_directories中添加include目录, 否则我们前面写的两个头文件将会找不到. 

```cpp
    include_directories(
      ${catkin_INCLUDE_DIRS}
      include
    )
 
    ... ...
 
    add_library(polygon_plugins src/polygon_plugins.cpp) 
```
6.如前所述, 咱还需要编辑关于插件的信息内容, 在plugin_test主目录下, 创建一个polygon_plugins.xml文件, 复制下述内容进入: 

```cpp
    <library path="lib/libpolygon_plugins">
      <class type="polygon_plugins::Triangle" base_class_type="polygon_base::RegularPolygon">
        <description>This is a triangle plugin.</description>
      </class>
      <class type="polygon_plugins::Square" base_class_type="polygon_base::RegularPolygon">
        <description>This is a square plugin.</description>
      </class>
    </library> 
```
7.在同目录下的package.xml文件中export tag块中添加下述内容: 

```cpp
      <plugin_test plugin="${prefix}/polygon_plugins.xml" />
```
8.在命令行中运行下述指令, 对应的输出信息如下所示:
在～/catkin_ws进行 catkin_make重新编译
```cpp
    rospack plugins --attrib=plugin plugin_test
    plugin_test /home/silence/WorkSpace/catkin_ws/src/plugin_test/polygon_plugins.xml 
```
9.在src目录下, 新建polygon_loader.cpp文件, 用于测试, 复制下述内容: 

```cpp
    #include <pluginlib/class_loader.h>
    #include <plugin_test/polygon_base.h>
 
    int main(int argc, char** argv)
    {
      pluginlib::ClassLoader<polygon_base::RegularPolygon> poly_loader("plugin_test", "polygon_base::RegularPolygon");
 
      try
      {
        boost::shared_ptr<polygon_base::RegularPolygon> triangle = poly_loader.createInstance("polygon_plugins::Triangle");
        triangle->initialize(10.0);
 
        boost::shared_ptr<polygon_base::RegularPolygon> square = poly_loader.createInstance("polygon_plugins::Square");
        square->initialize(10.0);
 
        ROS_INFO("Triangle area: %.2f", triangle->area());
        ROS_INFO("Square area: %.2f", square->area());
      }
      catch(pluginlib::PluginlibException& ex)
      {
        ROS_ERROR("The plugin failed to load for some reason. Error: %s", ex.what());
      }
 
      return 0;
    }
```
10.并在CMakeLists.txt中加入下述申明, 然后 $ catkin_make. 

```cpp
    add_executable(polygon_loader src/polygon_loader.cpp)
    target_link_libraries(polygon_loader ${catkin_LIBRARIES}) 
```
在～/catkin_ws进行 catkin_make重新编译 
11.编译成功后, 运行节点, 应该会得到下述类似的输出. 

```cpp
    $ rosrun plugin_test polygon_loader
    [ INFO] [1477584281.637794959]: Triangle area: 43.30
    [ INFO] [1477584281.637923253]: Square area: 100.00 
```
进阶插件注册方法：直接从navigation包中拷贝carrot_planner文件夹下的所有内容到ROS源码工作目录,并修改所有"carrot"字符为自定义字符 
结合基本插件注册和global_planner插件注册步骤，配置文件xml和包含的头文件有所不同。  
# 参考文献 
1.[ROS与navigation教程-编写自定义全局路径规划———创客制造](https://www.ncnynl.com/archives/201708/1887.html)  
2.[ROS的pluginlib的理解与实例——土豆西瓜大芝麻](https://blog.csdn.net/jinking01/article/details/79414486)  
3.[为ROS navigation功能包添加自定义的全局路径规划器(Global Path Planner)——BWBOT](https://blog.csdn.net/bluewhalerobot/article/details/73771020?utm_source=blogxgwz4)  
4.[Ubuntu 18.04 + ROS Melodic + TurtleBot3仿真—— Raina_RLN](https://www.cnblogs.com/raina/p/12162435.html)  
5.[ROS: global_planner 整体解析——白巧克力亦唯心](https://heyijia.blog.csdn.net/article/details/45030929)  
6.[ROS TURTLEBOT3 仿真学习(二)(UBUNTU18.04+ROS MELODIC)——事莫难于必成](https://blog.csdn.net/qq_39531155/article/details/109030507)  
感谢以上每一位博主优秀的文章，才得以让我最终完整地完成，在这谢谢啦，抱拳了！  

