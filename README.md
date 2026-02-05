```mermaid
graph TD
    %% 子图定义：快进程 - 负责UI显示与推理，低延迟
    subgraph FastLoop [快进程 - UI 与视觉推理]
        A[RGB-D 深度相机] -->|原始图像| B(YOLOv8 追踪)
        B -->|掩码与检测框| C[可视化绘制层]
        C -->|话题 /image_painted| D[RTAB-Map / Rviz2]
    end
    
    %% 子图定义：慢进程 - 负责繁重的3D运算与去重
    subgraph SlowLoop [慢进程 - 3D 空间计算]
        %% 虚线表示跨线程通信
        B -.->|带锁共享数据| E{缺陷分析器}
        E -->|深度提取| F[3D 投影 / 针孔模型]
        F -->|TF 坐标查询| G[世界坐标系转换]
        G -->|空间哈希匹配| H[缺陷管理器]
    end
    
    %% 最终输出
    H -->|过滤与合并| I[最终报告]
    
    %% 样式调整（可选，用于增强可读性）
    style FastLoop fill:#e1f5fe,stroke:#01579b
    style SlowLoop fill:#fff3e0,stroke:#e65100
    style E fill:#ffccbc,stroke:#bf360c
    style H fill:#c8e6c9,stroke:#2e7d32
```


flowchart LR
    Camera[RGB-D 相机] -->|RGB/Depth| FastLoop[fast_processing_loop\nYOLO分割 + 纹理绘制]
    FastLoop -->|/camera/camera/color/image_painted| RTAB[RTAB-Map / RViz2]
    SlowLoop -->|DefectManager| Report[final_report.txt]
