
graph TD
    %% 子图定义：使用方括号 [] 包裹标题，避免特殊字符报错
    subgraph FastLoop [Fast Loop - UI and Inference]
        A[RGB-D Camera] -->|Raw Image| B(YOLOv8 Tracking)
        B -->|Masks and Boxes| C[Visual Overlay]
        C -->|Topic image_painted| D[RTAB-Map / Rviz2]
    end
    
    subgraph SlowLoop [Slow Loop - 3D Calculation]
        %% 移除了标签里的括号，这是最容易报错的地方
        B -.->|Shared Data with Lock| E{Defect Analyzer}
        E -->|Depth Extraction| F[3D Projection]
        F -->|TF Lookup| G[World Coordinates]
        G -->|Spatial Hash| H[Defect Manager]
    end
    
    H -->|Filter and Merge| I[Final Report.txt]
