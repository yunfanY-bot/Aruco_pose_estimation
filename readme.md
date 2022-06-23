##文件描述
### calibration_checkerboard/
* 用于储存摄像头拍摄的棋盘格图像
* 用于摄像头校准

###  extract_frame.py
* 用于抽取视频帧并且转换成图片
* 用于摄像头校准

### pose_estimation.py
* 用于识别二维码姿态和方位
* 程序输出两个向量：角度坐标和位移坐标
* 需使用5*5 Aruco Marker
* 如更换摄像头则需先进行摄像头校准
* 需保证摄像机视野内只出现一个marker
* 需保持摄像头与marker垂直距离为10-20cm

### calibration.py
* 用于校准摄像机
* 将需要校准的摄像机置于棋盘格10-20cm，对准棋盘，并从各个角度距离录制成一段视频，然后使用extract\_frame.py提取图片，储存在calibration_checkerboard文件夹中
* input：棋盘格单元格边长、棋盘格格数（默认为7*10）
* output：相机内参，保存为calibration\_matrix.npy, distortion_coefficients.npy

### calibration\_matrix.npy,
* 相机内参数矩阵，用于计算marker坐标

### distortion\_coefficients.npy 
* 相机畸变系数， 用于计算marker坐标

##误差测量 (单位：cm)
> 参数：
> marker\_size = 1.2
> marker\_id = 0
> cam_distance = 16.5 

| 实际坐标 | 测量坐标 | 误差/信号波动 |
|----------|----------|------|  
|      (0, 0)    |    (0, 0)     |   Na   |
|      (-1, 0)    |     (-1.04, 0.02)     |   $\leq \pm$0.1  |
|    (-2 0)     |      (-2.05，0.04)   |    $\leq \pm$0.1  |
|     (-2.5, 0)     |      (-2.53, 0.03)     |  $\leq \pm$0.1   |
|     (-8, 0)     |      (-7.96, 0.05)     |  $\leq \pm$0.1   |
|      (0, 1)    |     (0, 1.04)     |   $\leq \pm$0.1  |
|    (0, 2)     |      (0, 2.05)   |    $\leq \pm$0.1  |
|     (0, 2.5)     |      (0, 2.54)     |  $\leq \pm$0.1   |
|     (0, 5.5)     |      (0, 5.58)     |  $\leq \pm$0.1   |

> *注： 以上测量结果为坐标中心对准摄像头视野中心的测量结果，改变坐标中心与视野中心相对位置可能会改变误差测量结果。

测量结果表示， 在摄像头高度固定为16cm时，可对marker实现毫米级定位。当marker处于摄像头视野边缘时，输出信号波动变大，误差变大。可使用对多个marker定位并取平均值的算法进一步提高精度。


##参考资料
* https://docs.opencv.org/4.x/d5/dae/tutorial_aruco_detection.html
* https://www.uco.es/investiga/grupos/ava/node/26
* https://fodi.github.io/arucosheetgen/