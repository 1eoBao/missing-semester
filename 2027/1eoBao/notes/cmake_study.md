## cmake学习笔记
# 宏观看cmake在ros系统中的作用
- 在 ROS 系统中，CMake 是最底层的核心构建工具。ROS 本身并没有重新发明一套编译系统，而是选择 CMake 作为所有 C/C++ 软件包的基础构建引擎，catkin本质上是在cmake之上进行封装和扩展，以便管理ros特有的依赖关系和包结构
# CMakeLists.txt常用命令
- 版本与项目
    
      # 必须放在 CMakeLists.txt 最顶部 
      cmake_minimum_required(VERSION 3.10)
      # 语言参数可选，常用 CXX（C++）、C（C语 言） 
      # 调用后自动设置 PROJECT_NAME 等变量 
      project(MyProject CXX) 

- 添加可执行文件与库
    
     
      # 将源文件编译生成可执行文件
      add_executable(MyApp main.cpp utils.cpp)
  
      # STATIC：静态库（.a / .lib），编译时直接嵌入可执行文件
      # SHARED：动态库（.so / .dll），运行时加载
      # 不指定类型时，由 BUILD_SHARED_LIBS 变量决定
      add_library(MyLib STATIC library.cpp)
- 链接库
        
        cmake
        # 可链接自己项目中的库目标，也可链接外部库
        target_link_libraries(MyApp PRIVATE MyLib)
- 设置变量
        
        # 普通变量，后续通过 ${变量名} 引用
        set(MY_APP_NAME "runoob-app")
        add_executable(${MY_APP_NAME} main.cpp)
        # 等价于 add_executable(runoob-app main.cpp)
                

        # 同时设置多个值（列表）
        set(SOURCES main.cpp utils.cpp helper.cpp)
        add_executable(MyApp ${SOURCES}) 
    注意：CMake 预定义/约定变量	CMAKE_CXX_STANDARD、CMAKE_BUILD_TYPE、PROJECT_SOURCE_DIR
- 为目标指定头文件路径：

            # 语法：add_custom_command(
            #          TARGET <目标>
            #          PRE_BUILD | PRE_LINK | POST_BUILD
            #          COMMAND <命令> [参数...]
            #          [COMMENT <说明>]
            #          [VERBATIM])
            # PRE_BUILD：在编译前执行
            # PRE_LINK：在链接前执行
            # POST_BUILD：在构建完成后执行
            add_custom_command(
            TARGET MyApp POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E echo "构建完成！"
            COMMENT "打印构建完成信息"
            )

- 条件语句：

            
            # 语法：if(<条件>) ... elseif(<条件>) ... else() ... endif()
            # CMake 支持的条件表达式包括：比较、逻辑运算、变量判断等
            if(CMAKE_BUILD_TYPE STREQUAL "Debug")
            # Debug 模式下启用调试信息
            message(STATUS "当前为 Debug 构建模式")
            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")
            else()
            # Release 模式下启用优化
            message(STATUS "当前为 Release 构建模式")
            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")
            endif()

- 