# gpd_ws
这个程序是gpd(https://github.com/atenpas/gpd.git)的补充算法（只用中文写了）

关于gdp的一些小改动：

1）.grasp_detector.cpp为生成角度滤波，以保证生成的grasp是在一个特定的角度范围内：
((cosangle>0.77)&&(z.maxCoeff()<0.62))||((cosangle>0.98)&&(z.maxCoeff()>=0.62))    ) 
0.62是最浅抓取深度，比这个小，则角度随意，否则，只能从上到下

2）.cloud_camera.cpp中加入了平面检测，故把平面去除了

3)grasp_detection_node.cpp加入了通讯机制

其余的cpp是一些不痛不痒的改动


使用说明（记得source）：

1.UR10驱动：
	在calib_ws-master空间中
	1.roslaunch ur_modern_driver ur10_bringup.launch robot_ip:=192.168.1.100 [reverse_port:=REVERSE_PORT]
	2.roslaunch ur10_moveit_config ur10_moveit_planning_execution.launch
	3.roslaunch ur10_moveit_config moveit_rviz.launch config:=true

2.realsense驱动：
在calib_ws-master空间中（该包是杨师弟提供https://github.com/lixiny/calib_ws.git）：
	roslaunch realsense_driver.launch

3.tf发布：
rosrun tf static_transform_publisher 0.075 0.06 -0.02 3.141592653 -1.570796 -0.03 /ee_link /camera_joint 100

4.使用gdp：
在gpd空间中，roslaunch gdp tutorial1.launch

5.主程序（先讲USB驱动权限开放）：
python amazon_picking.py

有个重点要说明：

gpd生成的grasp的空间方向是用手抓坐标系（固定在每个candidate grasp上的）的x,y,z相对于摄像头的单位向量

故这三个向量如何用，即就是旋转矩阵的三个列向量！具体参见amazon_picking.py，具体正负号得实际情况调一下：

rot2_mat2    = np.array([[grasps.approach.x,grasps.axis.x,-grasps.binormal.x,0],[grasps.approach.y,grasps.axis.y,-grasps.binormal.y,0],[grasps.approach.z,grasps.axis.z,-grasps.binormal.z,0],[0,0,0,1]])




https://www.cnblogs.com/zhengjiang/p/10864512.html
