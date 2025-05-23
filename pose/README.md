# 基于YOLOv8-Pose的人体姿态分析系统

本系统使用YOLOv8-Pose模型实时检测视频中的人体姿态，提供跌倒检测报警和握拳计数功能。

## 功能特点

- 实时人体姿态检测
- 跌倒行为自动识别与报警
- 握拳动作识别与计数统计
- 支持多种报警方式（视觉、声音）
- 支持摄像头实时检测和视频文件处理
- 可配置的敏感度参数

## 环境要求

- Python 3.8+
- PyTorch
- Ultralytics YOLOv8
- OpenCV
- NumPy

## 系统架构

1. **输入模块**：支持摄像头实时输入或视频文件输入
2. **姿态检测模块**：使用YOLOv8-Pose检测人体关键点
3. **动作分析模块**：基于姿态数据判断动作类型（跌倒/握拳）
4. **报警/计数模块**：根据检测结果触发相应操作

## 使用方法

### 1. 跌倒检测功能

#### 实时摄像头检测

```bash
python pose/test_fall_detection.py --camera 0 --alarm
```

#### 处理视频文件

```bash
python pose/test_fall_detection.py --video path/to/video.mp4 --output results
```

#### 调整敏感度

```bash
python pose/test_fall_detection.py --camera 0 --threshold 0.7
```

### 2. 握拳计数功能

#### 实时摄像头检测

```bash
python pose/test_fist_counter.py --camera 0
```

#### 处理视频文件

```bash
python pose/test_fist_counter.py --video path/to/video.mp4 --output results
```

#### 调整敏感度

```bash
python pose/test_fist_counter.py --camera 0 --threshold 0.12
```

## 参数说明

### 跌倒检测参数

- `--source`：输入源，可以是摄像头索引或视频文件路径
- `--output`：结果保存目录，默认为 'pose/results'
- `--alarm`：启用声音报警功能
- `--threshold`：跌倒检测阈值，范围0-1，值越小越敏感，默认0.6
- `--show`：实时显示检测结果，默认开启
- `--save`：保存检测结果视频，默认开启

### 握拳计数参数

- `--camera`：使用摄像头（指定索引，如0表示默认摄像头）
- `--video`：使用视频文件
- `--output`：结果保存目录，默认为 'pose/results'
- `--threshold`：握拳检测阈值，范围0-1，值越小越敏感，默认0.15
- `--no-save`：不保存检测结果视频
- `--reset-key`：重置计数器的键，默认为 'r'

## 检测算法说明

### 跌倒检测算法

系统使用以下几种方法判断跌倒：

1. **姿态角度分析**：计算关键身体部位（头部、躯干）与地面的角度
2. **身体比例变化**：检测人体边界框宽高比的急剧变化
3. **关键点高度变化**：检测人体关键点（如头部）高度的突然下降
4. **运动速度分析**：检测异常的快速垂直运动

### 握拳检测算法

由于YOLOv8-Pose模型不提供手指关键点，系统使用以下方法近似判断握拳动作：

1. **手臂角度分析**：计算手臂弯曲角度
2. **前臂长度比例**：检测手腕到肘部的距离与手臂长度的比例变化
3. **状态转换跟踪**：通过状态机避免重复计数和误判

## 注意事项

1. 确保安装了所有必要的依赖
2. 检测效果受光线条件、摄像头角度等因素影响
3. 系统设计用于辅助监测，不应完全替代人工监督
4. 中文显示问题：程序使用了编码声明和合适的字体处理，确保中文正确显示
5. 在低配置设备上可能需要调整分辨率以获得更流畅的效果 