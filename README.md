# 编译
1. 编译ORB_SLAM2_DENSE库 (opencv3)
cd catkin_ws/src/ORB_SLAM2_DENSE/ORB_SLAM2_DENSE
./build.sh

2. 编译并安装 cv_bridge
cd catkin_ws
catkin_make -DCATKIN_WHITELIST_PACKAGES=cv_bridge
catkin_make install

3. 替换cv_bridgeConfig.cmake以使cvbridge调用opencv4
cd catkin_ws
cp cv_bridgeConfig.cmake install/share/cv_bridge/cmake/cv_bridgeConfig.cmake

4. 编译mask_cnn
cd catkin_ws
rm devel build -rf         #防止mask_cnn链接cv_bridge中的opencv3
source install/setup.bash  #添加cv_bridge环境变量，代替安装在系统的cv_bridge
catkin_make -DCATKIN_WHITELIST_PACKAGES=mask_cnn

5. 编译orb_slam_dense ros节点 (opencv3)
catkin_make -DCATKIN_WHITELIST_PACKAGES=orb_slam_dense

# 运行
cd catkin_ws
source devel/setup.bash

roslaunch orb_slam_dense rgbd1.launch                              #启动orb_slam
roslaunch mask_cnn mask_cnn.launch image:=/camera/rgb/image_color  #启动mask_cnn
rosbag play rgbd_dataset_freiburg1_room.bag -r 0.1                 #播放rosbag




