# Changelog for TogetheROS

v2.2.0 (2024-04-11)
------------------

功能变更：

- 基于TROS Foxy 2.1.3版本，适配Ubuntu 22.04系统和ROS2 Humble。
- TROS的安装路径由**`/opt/tros`**变更为**`/opt/tros/humble`**，和ROS2的安装路径层级和命名保持一致。
- 不再提供`tros-ros-base`安装包（包含rclcpp、rclpy、ros2cli等ROS2基础功能包），使用标准的ROS2发行包，安装TROS Humble时自动安装依赖的ROS2 Humble。
- 使用ROS2 fastdds的零拷贝通信功能，涉及到数据采集、图像编解码、算法示例等使用到图像数据的模块。
- 零拷贝通信使用的QoS的Reliability由`RMW_QOS_POLICY_RELIABILITY_RELIABLE`（rclcpp::QoS()）变更为`RMW_QOS_POLICY_RELIABILITY_BEST_EFFORT`（rclcpp::SensorDataQoS()），避免使用零拷贝时潜在的稳定性风险。
- 重构`hobot_dnn`，使用更底层的板端推理框架`libdnn`，不再使用`easydnn`。
- `hobot_audio`升级语音算法SDK，使用更底层的板端推理框架`libdnn`，不再使用`easydnn`。
- `hobot_trigger`适配ROS2 Humble版本rosbag2。

新增功能：
- `robot_dev_config`新增bloom编译和打包的脚本，用于ARM平台编译和打包TROS。
- `hobot_mipi_cam` node新增frame_ts_type配置项，支持realtime（用于计算通信延迟）和sensor（默认，用于传感器的时间戳同步）配置参数。
- 新增`hobot_shm` node，用于配置ROS2零拷贝环境。

问题修复：
- 修复编译器升级引入的兼容性问题。
- 修复板端编译部分ROS2 pkg存在的路径依赖问题。


v2.1.3 (2024-03-11)
------------------

功能变更：

- jpeg压缩格式图片使用的数据类型由`sensor_msgs::msg::Image`变更为标准的`sensor_msgs::msg::CompressedImage`，支持使用foxglove和ros2 rqt等工具查看TROS发布的jpeg格式图片。涉及到hobot_websocket, hobot_codec, hobot_image_publisher, hobot_usb_cam模块。
- 统一使用jpeg/mjpeg配置项指定发布/订阅jpeg压缩格式图片，删除jpeg-compressed/mjpeg-compressed配置项，涉及到hobot_codec和hobot_usb_cam模块。
- 引入表示TROS发行版的环境变量TROS_DISTRO，执行`source /opt/tros/setup.bash`/`source /opt/tros/local_setup.bash`命令后，环境变量`TROS_DISTRO`的值为空。hobot_codec, hobot_audio, hobot_mipi_cam, hobot_usb_cam等模块使用的配置文件路径由`/opt/tros/lib`变更为`/opt/tros/${TROS_DISTRO}/lib`。


v2.1.2 (2024-01-19)
------------------

新增功能：

- 重构hobot_usb_cam，支持更多format配置和转码。
- `hobot_audio`更新语音SDK，同时支持2mic和4mic麦克风板;增加micphone_name配置设备ID号。

问题修复：

- `hobot_rgbd_cam` node修复发送数据消息step字段设置错误问题。
- `hobot_tts`更新音频播放函数调用，解决新版本系统播放失败问题。
- `hobot_llm`删除config设备树文件，以及更新README，新版本系统可通过命令工具设置ION内存大小。


v2.1.1RC1 (2023-11-03)
------------------

新增功能：

- RDK Ultra平台支持`orb_slam3`和`vio`算法。

问题修复：

- `hobot_codec`优化类型检查逻辑，修复`in_format`为`h265`时检查失败的问题，同时提升类型的可扩展性。
- `hand_lmk_detection`修复RDK Ultra平台打包模型文件错误的问题。


v2.1.1 (2023-10-18)
------------------

新增功能：

- 新增`hobot_chatbot` node，调用智能语音、大语言模型、文本转语音模块，实现板端语音聊天功能。

问题修复：

- 文本转语音`hobot_tts` node，修复某些字符导致应用退出问题。


v2.1.0 (2023-09-14)
------------------

功能变更：

- `tros-ros-base`更新到最新ROS2 foxy源码，兼容最新ROS2 foxy软件包。
- 使用ROS2 foxy软件包只`source /opt/tros/setup.bash`即可，不再需要使用脚本建立软链接。

新增功能：

- 文本转语音`hobot_tts` node新增参数指定播放音频设备。
- 新增大语言模型`hobot_llm` node，可在端侧体验LLM。
- 图像编解码`hobot_codec` node配置参数`in_format`新增`jpeg-compressed`配置项，同时根据配置项选择订阅的话题数据类型。

问题修复：

- MIPI图像采集`hobot_mipi_cam` node修复发送RGB格式数据消息step字段设置错误问题。


v2.0.2 (2023-08-28)
------------------

功能变更：

- tros.b安装时配置的ROS2源（`/etc/apt/sources.list.d/ros2.list`）变更为清华镜像源，解决安装ROS2 package速度慢和失败的问题。

新增功能：

- 启动tros.b脚本配置环境时（`source /opt/tros/setup.bash`和`source /opt/tros/local_setup.bash`）新增权限检查的功能。如果当前账户不具有root权限将会自动进入切换到root账户的流程，解决因为权限不够导致的使用tros.b失败的问题。
- 智能语音算法`hobot_audio` node新增音频设备号参数配置功能，方便二次开发​。
- 事件触发`hobot_trigger` node新增通过std_msg话题给Trigger模块发放任务功能，规范Trigger配置方法。

问题修复：

- 修复图像加速处理`hobot_cv` node同时进行crop&resize处理图像时，处理失败的问题。
- 修复MIPI图像采集`hobot_mipi_cam` node启动时输出error log的问题。
- 修复数据可视化消息转换`hobot_visualization` node的launch启动文件配置无效的问题。


v2.0.1 (2023-06-10)
------------------

  1. rosbag2_storage_mcap, 新增ros2官方repo, rosbag数据播放记录支持mcap格式。
  2. hobot_trigger, 新增trigger基础应用示例,支持trigger时间触发与数据记录。
  3. hobot_visualization, 新增ai话题消息转为visualization话题信息功能。


v2.0.0 (2023-05-29)
------------------

  1. 代码托管平台更换为GitHub。
  2. hobot_dnn，修复X86平台hobot_dnn DDR模型推理失败问题，修复MobileNet_SSD后处理崩溃问题。
  3. hobot_audio，增加语音ASR识别结果输出。
  4. 更换应用打包方式为各个Node独立打包，新增打包脚本实现该功能。
  5. 更新应用launch启动脚本，引用依赖的Node Launch脚本，不再完整配置依赖的Node，只修改部分参数。
  6. webscoket更新web端地平线LOGO。
  7. hobot_codec和hobot_image_publisher修复X86平台编译问题。


v1.1.6 (2023-03-28)
------------------

  1. 新增X86平台支持。
  2. 优化源码编译流程，新增编译脚本build.sh，支持X3和X86平台源码编译。
  3. webserver使用的端口从80变更为8000，支持非root用户启动webserver服务。


v1.1.5 (2023-02-21)
------------------

  1. hobot_audio，新增麦克风类型配置功能，支持线形和环形麦克风类型；语音算法适配环形麦克风阵列。
  2. 修复TROS安装包中package路径依赖导致的使用TROS编译失败问题。
  3. hobot_sensors，优化mipi_cam图像发布时间戳，使用硬件时间戳并调整曝光时间，用于多传感器时间戳同步。
  4. hobot_image_publisher，配置文件新增h265编码测试视频；启动时输出配置参数；支持直接发布解码前JPEG格式的压缩图片。


v1.1.4 (2023-01-13)
------------------

  1. hobot_dnn，dnn_node_example删除未使用的GetNV12Tensor接口；dnn_node修复部分内置算法后处理中LoadConfig无返回值的问题。
  2. hobot_cv，新增hobot_cv_benchmark测评工具。
  3. hobot_sensors，新增mipi_cam自适应不同类型摄像头功能，完善错误log提示。
  4. hobot_image_publisher，新增MP4, H264和H265格式视频发布功能。
  5. mono2d_body_detection，修复在BPU压力大时，后处理中AI结果排序算法异常导致的无AI结果发布的问题。
  6. sysroot_docker，升级1.1.4版本的系统库和头文件，同时增加ffmpeg库相关文件。


v1.1.3 (2022-11-28)
------------------

  1. hobot_dnn，修复内置的算法前处理GetNV12PyramidFromNV12Img接口导致图片padding区域乱码问题。
  2. hobot_cv，新增图片padding功能；新增nv12格式图片处理功能。
  3. hobot_codec，优化输出日志，帮助异常情况下快速定位问题原因。
  4. hobot_sensor，支持imx219, imx477, ov5647型号mipi摄像头。


v1.1.2 (2022-10-29)
------------------

  1. hobot_dnn

    默认推理模式由同步模式变更为效率更高的异步模式。
    当模型文件中只包含一个模型时，可以不指定model_name模型名配置项。
    算法推理任务支持指定BPU核，默认使用负载均衡模式，满足算法部署性能调优需求。
    自适应支持多核模型推理。
    简化重构内置的算法输出解析方法，降低使用和理解难度。
    新增dnn_node_sample package示例，帮助用户参考部署算法模型。
    example示例使用板端安装在/app/model/basic/路径下的模型文件。

  2. hobot_msgs，roi感知消息Roi.msg和属性感知消息Attribute.msg中增加置信度信息，提升算法开发效率。
  3. hobot_cv，修复极端情况下内存状态异常导致的图片处理失败的问题，同时增加异常恢复机制；修复BPU加速处理图片对16字节宽度对齐限制的问题。
  4. hobot_sensor、hobot_websocket、hobot_sensors、hobot_image_publisher，优化log提示信息，帮助用户快速定位问题。
  5. hand_lmk_detection，修复算法推理压力大时crash的问题。
  6. hand_gesture_detection，修复内存泄漏问题。
  7. parking_perception，修复launch文件设置错误问题
  8. mono2d_trash_detection，新增2D垃圾检测算法示例，基于PaddlePaddle开源框架、地平线AI工具链和TogetheROS实现算法训练、模型转换和算法部署的全流程功能。
  9. tros_toolkit，简化TogetheROS源码编译流程。


v1.1.1 (2022-09-21)
------------------

  1. hobot_cv，优化log输出，出现异常时提示解决方法。
  2. hobot_image_publisherd，优化log输出，出现异常时提示解决方法。
  3. hobot_audio，智能语音算法示例升级语音算法SDK，优化声源定位DOA识别效果。
  4. audio_tracking，声源定位DOA应用示例，控制机器人转向声源方向。
  5. navigation2，源码集成ROS2的路径规划算法包，可用于二次开发。
  6. hobot_sensors，修复默认的MIPI相机标定文件路径错误的问题。

v1.1.0 (2022-09-09)
------------------

  1. hobot_dnn，example中算法前处理使用hobot_cv进行图片resize，解决不同输入图片分辨率影响算法效果的问题。
  2. hobot_cv，修复图片处理耗时不稳定的问题。
  3. hobot_websocket，支持3D检测算法数据序列化输出；支持停车区域环境感知算法数据的序列化和渲染。
  4. hobot_codec，增强程序鲁棒性，解决处理异常图片时程序崩溃的问题；支持通过rqt预览JPEG压缩格式图像；支持输出帧率控制，降低跨设备传输图片的网络带宽消耗。
  5. hobot_sensors，支持发布camera_info话题的相机标定数据；适配RGBD模组。
  6. hobot_msgs，属性感知消息Attribute.msg中属性数值value成员类型由int16变更为float32，可用于表示更丰富的属性信息。
  7. hobot_audio，智能语音算法示例完善README中算法输出的DOA数据说明。
  8. mono3d_indoor_detection，单目3D室内检测算法示例支持通过订阅图片进行算法推理；完善README中算法输出数据的说明。
  9. parking_perception，停车区域环境感知算法示例支持WEB端渲染算法结果功能。
  10. parking_search，新增自动泊车的应用示例。
  11. line_follower，新增小车巡线的应用示例，用CNN的方式实现巡线任务中引导线的位置感知。
  12. 交叉编译优化，拉取代码时自动下载sysroot_docker。

v1.0.5 (2022-08-26)
------------------

  1. hobot_dnn，example示例修复发布的ROS Msg中算法推理输出帧率数据错误的问题。
  2. parking_perception，新增用于停车场场景的算法示例，支持行人、汽车、停车地锁等目标检测，以及道路、车道线、车位区域等语义分割。
  3. hobot_image_publisher，新增本地图片发布工具，可用于算法测评。
  4. hobot_cv，新增neon加速高斯滤波和均值滤波功能。
  5. hobot_codec，修复发布的bgr8和rgb8格式图片消息中step参数错误导致的图片无法展示的问题。
  6. mono2d_body_detection，支持启动时选择使用MIPI/USB类型摄像头；修复发布的ROS Msg中算法推理输出帧率数据错误的问题。
  7. hand_lmk_detection，支持启动时选择使用MIPI/USB类型摄像头；修复rqt_graph工具展示的Node graph不连续的问题。
  8. hand_gesture_detection，支持启动时选择使用MIPI/USB类型摄像头；修复特定场景下发布的ROS Msg中算法推理输出帧率数据错误的问题。

v1.0.4 (2022-08-18)
------------------

  1. hobot_dnn，升级预测库，修复偶现算法推理输出异常的问题，以及加密芯片访问报错的问题
  3. audio_control，更新说明文档中的唤醒词说明
  4. hobot_hdmi，增加启动的launch文件，更新文档
  5. hobot_sensor，优化NV转RGB效率，更新RGBD驱动
  6. ORB SLAM3，新增v-slam功能
  7. lttng，新增trace工具lttng支持

v1.0.3 (2022-07-29)
------------------

  1. ros-perception,增加ROS2 vision_opencv适配
  3. hobot_audio，优化识别效果，更换配置默认唤醒词“精灵精灵”为“地平线你好”
  4. hobotcv，新增图片rotate，crop&resize&rotate接口
  5. hobot_sensor，优化NV转RGB效率，更新RGBD驱动

v1.0.2 (2022-07-17)
------------------

  1. mono2d_body_detection，人体关键点感知结果中增加置信度信息
  2. audio_control，增加语音控制小车运动功能
  3. hobot_audio，增加语音识别功能
  4. hobot_websocket，优化显示卡顿，解决偶现加载失败问题
  5. hobot_msgs，更新Point.msg，增加audio_msg

v1.0.1 (2022-06-23)
------------------
  1. HobotWebsocket，修复偶现内存泄漏问题
  2. HobotCodec，优化CPU占用
  3. 新增HobotHDMI功能，支持HDMI屏显展示
  4. mono2d_body_detection，更新性能统计方式，优化Log输出
  5. hand_lmk_detection，修复内存泄漏，优化推理性能
  6. hand_gesture_detection，解决画面中有多个人手的情况下部分人手无手势结果输出的问题，优化推理性能
  7. HobotDNN，优化异步推理性能，增加性能统计
  8. HobotCV，优化Log输出方式
  9. HobotSensor，修复rgbd sensor通过shared mem通信方式发布消息失败问题
 
