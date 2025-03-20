# 项目设计原因

本项目源于学校实验的要求，虽然实验内容并不复杂，但网络上缺少现成的、直接可用的项目代码。为此，我分别利用 DeepSeek 和 GPT 对代码进行了优化，以便大家对比和学习。

另外，由于网络上难以获取高清的棋盘格图，而相机标定与单目测距对棋盘格图的清晰度要求较高，我使用 Photoshop 绘制了一份简单且高清的棋盘格图，供大家在实验中使用。


# 相机标定与单目测距工具

本项目提供了一款用于相机标定和单目测距的工具，基于棋盘格标定法和 OpenCV 库实现。程序能够自动检测棋盘格角点，利用 EXIF 数据提取焦距信息（如果存在），进而计算相机矩阵、畸变参数以及相机到棋盘格的距离。

## 功能特点

- ***提供了高清无损的棋盘格图片***
- **棋盘格角点检测**：自动检测并精细化处理棋盘格内角点。
- **相机标定**：使用 OpenCV 对相机进行标定，计算相机矩阵和畸变系数。
- **单目距离测量**：根据标定结果计算相机与棋盘格之间的距离。
- **EXIF 数据提取**：如果图像中包含 EXIF 信息，则自动提取焦距，提升标定精度。
- **高分辨率图像优化**：对高分辨率图像进行缩放处理，加快角点检测速度后再映射回原图尺寸。

## 依赖环境

- Python 3.x
- [OpenCV](https://opencv.org/) （cv2）
- [NumPy](https://numpy.org/)
- [Pillow](https://python-pillow.org/)

### 安装方法

可以使用 pip 安装所需的 Python 库：

```bash
pip install opencv-python numpy Pillow
```

# 使用说明

本教程将指导你如何使用本项目进行相机标定与单目测距。请按照以下步骤操作：

## 1. 下载与配置

（1） **下载项目代码**  
通过 Git 克隆项目仓库或直接下载 ZIP 文件，并解压到本地。例如，使用 Git 命令：

```bash
git clone https://github.com/Ter1ko/CalibraDepth.git
cd MonoCalibPro
```

（2） **查找并确认相机参数**

在使用本程序之前，请先查找你所使用摄影设备的参数：

 - **相机传感器尺寸（sensor_size_mm）**：查找你的相机或手机的传感器宽度和高度（单位：毫米）。
 - **拍摄镜头焦距（focal_length_mm）**：查找你的镜头的物理焦距（单位：毫米）。
   
将以上参数填入程序中预设的位置（位于 calibration_gpt.py 或 calibration_dp.py 文件顶部的用户可调参数部分）。

 ## 2.准备棋盘格标定板
 
 （1）**使用提供的棋盘格图**
 
本项目中已提供一份由 Photoshop 制作的高清棋盘格图（支持 JPG、PNG、TIF 格式）。

 - 如果你使用 A4 纸打印该棋盘格，请确保打印质量良好。
 - 若你不使用 A4 纸或采用其他棋盘格，请根据实际尺寸修改程序中的棋盘格参数（如 chessboard_size 和 square_size_m）。

（2）**打印棋盘格**

将棋盘格图按原比例打印出来，确保打印后的棋盘格图案清晰无失真，以便角点检测时效果更佳。

 ## 3.拍摄校准图片
为了获得准确的标定结果，建议拍摄多张校准图片，具体要求如下：

（1）**拍摄数量**

建议拍摄 10 到 25 张校准图片，至少需要 5 张有效图片。更多的图片可以提高标定的准确性。

（2） **拍摄方式**

 - 多角度拍摄：从不同角度和方向拍摄棋盘格，保证棋盘格在图像中处于不同位置和姿态。
 - 不同距离：拍摄时保持相机与棋盘格之间有一定的距离变化，以便后续计算距离时获得更稳定的结果。
 - 充足光线：确保拍摄时光线充足，避免过曝或欠曝，保证棋盘格细节清晰可见。
 - 固定焦点：尽量保持相机处于固定焦点状态，以减少拍摄模糊。

（3） **将图片保存到指定文件夹**
   
拍摄完成后，将所有校准图片统一放置于项目根目录下的 images 文件夹中，并确保图片格式与程序中设置的 image_extension 一致。

 ## 4.运行程序：

1.确认所有参数配置正确，校准图片已放入 images 文件夹中。

2.使用 Python 运行脚本：

```bash
python calibration_gpt.py
python calibration_dp.py
```

3.程序将自动读取images 文件夹中的所有图像并校准图片，进行角点检测、相机标定和距离计算，并在终端输出标定结果、重投影误差及相机到棋盘格的距离信息。



## 重要参数说明
 
- **chessboard_size**：棋盘格内角点数量（行×列），用于角点检测。
- **square_size_m**：棋盘格每个正方形的实际边长（单位：米）。
- **sensor_size_mm**：相机传感器尺寸（宽、高，单位：毫米），需根据具体设备进行设置。
- **focal_length_mm**：默认焦距（单位：毫米），当 EXIF 中无焦距信息时使用。
- **image_extension**：校准图像的文件格式，所有图像需存放于 images 文件夹内。

## 注意事项
 - **参数调整**：
如果你使用非 A4 打印或其他规格的棋盘格，请务必修改相应参数，如棋盘格角点数量 (chessboard_size) 和每个棋盘格正方形的实际边长 (square_size_m)，以匹配实际打印尺寸。

  修改相机传感器参数时，请根据实际设备信息进行更新。 

  EXIF 数据中包含正确的焦距信息，程序将自动优先使用；否则使用默认值。

 - **图像质量**：
请确保拍摄的校准图片中棋盘格图案清晰、角点明显。图像模糊或角点不清会直接影响标定精度。

 - **环境光线**：
选择光线均匀的环境拍摄，避免过强或过弱的光线干扰角点检测。


# 贡献
  欢迎各位开发者提出改进意见或贡献代码！

# 鸣谢
- 感谢 OpenCV 团队提供了强大的计算机视觉库。
- 感谢所有为本项目贡献代码和建议的开发者。
