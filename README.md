```mermaid
graph TD
    %% 子图定义
    subgraph FastLoop [快进程 - UI 与视觉推理]
        A[RGB-D 深度相机] -->|原始图像| B(YOLOv8 追踪/推理)
        B -->|掩码与检测框| C[可视化绘制层]
        C -->|话题 image_painted| D[RTAB-Map / Rviz2]
    end
    
    subgraph SlowLoop [慢进程 - 3D 空间计算]
        %% 虚线表示跨线程数据共享
        B -.->|带锁共享数据| E{缺陷分析器}
        E -->|深度提取| F[3D 投影]
        F -->|TF 坐标查询| G[世界坐标系]
        G -->|空间哈希| H[缺陷管理器]
    end
    
    H -->|过滤与合并| I[最终报告 (Final Report.txt)]
```
