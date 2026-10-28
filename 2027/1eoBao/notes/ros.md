## ROS笔记
# rosrun
- rosrun ros_pkg ros_node _arg1=param --argc argv的使用 参数服务器全局参数设置
  
# rosinit
- \param options 节点启动选项，被封装进了ros::init_options  

1.ros::init_options::AnonymousName：在节点名后加随机后缀，允许启动多个同名节点。

# publisher
- Publisher advertise(const std::string& topic, uint32_t queue_size, bool latch = false)  
  latch=false:不保存最后发布消息 true：保存
    作用：发布低频不变消息如地图

