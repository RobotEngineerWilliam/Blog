---
title: ROS 指南
date: 2024-07-29 14:18:16
aim: 建立几篇博客说明ROS的功能实现
---


.py 文件编译：

- 文件开头需要加上以下注释，表明解释器

```python
#! /usr/bin/env python
```

- 文件加上可执行权限

```markup
chmod u+x ***
```

- launch运行
  
  ```xml
  <node name="random_map_sensing" pkg="mppi_panda_mobile" type="real_time_obstacle_publisher.py" output="screen">
      </node>
  ```
  
  
