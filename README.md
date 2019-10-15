原程序为 https://github.com/YaoZhiwen/lsd_slam_catkin_16.04 ，安装方法见[原README.md](./原README.md)

#### 无法编译通过，修改的地方如下：

1. 因无法正常生成ros消息的头文件，我在另一个package里生成了之后复制到`~/catkin_ws/src/lsd_slam/lsd_slam_viewer/src`，这两个文件为`keyframeMsg.h、keyframeGraphMsg.h`。

2. 原程序总是有#include错误，如`#include "lsd_slam_viewer/KeyFrameDisplay.h"`，我把所有的`lsd_slam_viewer/`都删掉了。

3. OpenCV路径不对，我根据系统中的路径修改`~/catkin_ws/src/lsd_slam/lsd_slam_core/CMakeLists.txt`文件为：

4. ```cmake
   set(OpenCV_INCLUDE_DIRS /opt/ros/kinetic/include/opencv-3.3.1-dev)
   set(OpenCV_LIBS /opt/ros/kinetic/lib/x86_64-linux-gnu/libopencv_core3.so.3.3.1 /opt/ros/kinetic/lib/x86_64-linux-gnu/libopencv_imgproc3.so.3.3.1 /opt/ros/kinetic/lib/x86_64-linux-gnu/libopencv_highgui3.so.3.3.1 /opt/ros/kinetic/lib/x86_64-linux-gnu/libopencv_calib3d3.so.3.3.1)
   set(OpenCV_DIR /opt/ros/kinetic/share/OpenCV-3.3.1-dev)
   ```

5. 编译完后，运行报错`QObject::startTimer: Timers cannot be started from another thread`，将`/home/liuly/catkin_ws/src/lsd_slam/lsd_slam_core/src/util/settings.h`中的`bool displayDepthMap = true;`改为`bool displayDepthMap = false;`。

#### bag测试

```sh
roscore
rosrun lsd_slam_core live_slam image:=/image_raw camera_info:=/camera_info
rosbag play /mnt/hgfs/dataset/LSD_room.bag
rosrun lsd_slam_viewer viewer
```

#### D435测试

```sh
roslaunch realsense2_camera rs_camera.launch
rosrun lsd_slam_core live_slam image:=/camera/color/image_raw camera_info:=/camera/cocolor/camera_info
rosrun lsd_slam_viewer viewer
```

