# 要点
## 快速测试
直接clone下来
```
git clone git@github.com:Mousikar/ws_swarm_robot_py.git
```
到有src文件夹的文件夹运行catkin_make

1. 框架说明：
多移动机器人Gazebo仿真平台软件包括
    （1）gazebo_swarm_robot_tb3
        多机器人仿真平台启动+多机器人运动驱动
    （2）turtlebot3_description
        turtlebot3仿真模型
    （3）gazebo_ros
        Gazebo平台启动包


2. 使用步骤
Step 1 打开多机器人Gazebo仿真平台
    roslaunch gazebo_swarm_robot_tb3 gazebo_swarm_robot_5.launch

Step 2 运行多移动机器人运动控制程序
    角度一致性实验
      rosrun gazebo_swarm_robot_tb3 main_angle
        示例代码功能为角度一致性

Step 3 停止小车
    rosrun gazebo_swarm_robot_tb3 gazebo_stop_robot


3. 其他
    (1)Ubuntu终端停止程序操作：按键“Ctrl+C”即可；若无法停止，可采取按键“Ctrl+Z”和按键“Ctrl+D”组合使用。

## C++函数使用
头文件是swarm_robot_control.cpp

\
初始化：std::vector<int> swarm_robot_id{1, 2, 3, 4, 5};

SwarmRobot swarm_robot(&nh, swarm_robot_id);

\
获得单个、多个机器人位姿：getRobotPose

swarm_robot.getRobotPose(i, current_robot_pose); 

swarm_robot.getRobotPose(current_robot_pose); \\输入、输出都是一个n*3的矩阵current_robot_pose

\
移动单个、多个机器人：moveRobot

移动单个机器人：swarm_robot.moveRobot(i, 0.0, w);

移动多个机器人：swarm_robot.getRobotPose(speed);  \\输入是一个n*2的矩阵speed

\
停止单个、所有机器人：stopRobot

停止第i个机器人：swarm_robot.stopRobot(i);

停止所有机器人：swarm_robot.stopRobot();

\
检查速度是否超过上下限：checkVel

w = swarm_robot.checkVel(w, MAX_W, MIN_W);

## Python函数使用
头文件是swarm_robot_control_new.py

\
初始化：

swarm_robot_id = [1, 2, 3, 4, 5, 6]

swarm_robot = SwarmRobot(swarm_robot_id)

\
获得单个、多个机器人位姿：getRobotPose

robot_pose = swarm_robot.get_robot_poses(i) 

current_robot_pose = swarm_robot.get_robot_poses()  # 输出是一个n*3的矩阵current_robot_pose

\
移动单个、多个机器人：moveRobot

移动单个机器人：swarm_robot.move_robot(i, 0.0, w)

移动多个机器人：swarm_robot.move_robots(speed) # 输入是一个n*2的矩阵speed

\
停止单个、所有机器人：stopRobot

停止第i个机器人：swarm_robot.stop_robot(i)

停止所有机器人：swarm_robot.stop_robots()

\
检查速度是否超过上下限：checkVel

w = swarm_robot.check_vel(w, MAX_W, MIN_W)

## 话题
可以打开rostopic list查看有哪些话题

控制小车移动的话题："/robot_{}/cmd_vel".format(i+1)

机器人的基座标系和世界坐标系：

"robot_{}/base_footprint"

"robot_{}/odom"

## launch文件
可以看one.launch那个文件

●是burger还是pi

●名字robot1等等

●初始位置和姿态

●世界地图（6个机器人那个launch加载了不同的环境）

●加载模型和状态发布

## 模型
常用的是turtlebot3_burger和turtlebot3_waffle_pi

到这里应该讲完了，相关的还有实验的相关话题，和仿真的主要区别就是世界坐标系不一样。

std::string base_marker = "base_marker";

其余的基本完全一致。

