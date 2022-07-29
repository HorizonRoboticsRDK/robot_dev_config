# Changelog for TogetherROS

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
 