# [AI Model Efficiency Toolkit (AIMET)](https://github.com/quic/aimet)
> AIMET is a library that provides advanced model quantization and compression techniques for trained neural network models. It provides features that have been proven to improve run-time performance of deep learning neural network models with lower compute and memory requirements and minimal impact to task accuracy.

# [nuPlan](https://www.nuscenes.org/nuplan#code)
> nuPlan is the world's first large-scale planning benchmark for autonomous driving

## [nuplan-devkit](https://github.com/motional/nuplan-devkit)
### environment setup
基本参考[这个页面](https://nuplan-devkit.readthedocs.io/en/latest/installation.html)就可以  
conda安装后需要编辑PATH  
```
export PATH=$PATH:/home/wu/miniconda3/bin
source ~/.profile
```

安装nuplan-devkit用的是Option B  
```
pip install -e .
```  

好像是因为数据太大[下载页面](https://www.nuscenes.org/nuplan#code)的数据集无法下载，[这个网页](https://motional-nuplan.s3.ap-northeast-1.amazonaws.com/index.html)的就可以  
下载完用PSCP传输到开发机或者虚拟机  
```
unzip nuplan-v1.1_mini.zip -d nuplan-v1.1_mini
```  

这两个文件夹需要自己生成    
```
mkdir ~/nuplan
mkdir ~/nuplan/dataset
mkdir ~/nuplan/exp
```

### jupyter notebook
需要用`conda`安装`jupyter notebook`并启动，只是用pip的话之前用`environment.yml`构建的环境不起作用  

先启动环境（虚拟机）  
```
conda activate nuplan 
```
启动jupyter（虚拟机）  
```
jupyter notebook --no-browser --port=8080
```
建立ssh（本地）
```
ssh -L 8080:localhost:8080 kangming.wu@192.168.15.20
```
本地浏览器打开（本地）
```
http://localhost:8080/tree?token=c4a135f3863ec0aaebf5cea44857d36882d8d2ab8230c1a2
```
参考：https://stackoverflow.com/questions/69244218/how-to-run-a-jupyter-notebook-through-a-remote-server-on-local-machine  

### dataset
文件名意义：```<log_date>_<vehicle_number>_<snippet_start>_<snippet_end>.db```  

`.db`文件可以用`DB Browser for SQLite`打开，但因为很多都是BLOB文件，所以还是需要用devkit自带的库来读取数据  

```python
class NuPlanDB(DB)
```
读取`.db`文件后得到的类  

```python
from nuplan.database.tests.test_utils_nuplan_db import get_test_nuplan_db

db = get_test_nuplan_db()
print(type(db.session))
print(db.session)
print(type(db.name))
print(db.name)
print(type(db.data_root))
print(db.data_root)
print(type(db.table_root))
print(db.table_root)
print(type(db.table_names))
print(db.table_names)
print(type(db.tables['category']))
print(db.tables['category'].all())
```

- gpkg文件  
  layer类似ps的layer，上层会遮挡下层  

# concept 用语集
### ego vehicle / agent vehicles 
ego vehicle：自车  
agent vehicle：搭载了agent驾驶的汽车  
### open-loop and close-loop
open-loop: 从输入到输出是一连串的，按照设定好的顺序执行，没有反馈。例如空调遥控、洗衣机、马路信号灯    
![image](https://github.com/user-attachments/assets/38c4f5c1-ad6e-452c-93d7-34c01f9dafa4)

close-loop：输出后有反馈来调节输入。例如空调、冰箱、暖炉    
![image](https://github.com/user-attachments/assets/5818211d-b62d-4f88-881b-862054bb778a)

具体来说，close-loop里，自车以及其他车驾驶轨迹偏离原来记录的数据  

### planner
用于决定车辆行进路线与控制车辆的程序  
```python
        behavior_trajectory = self.get_behavior_trajectory(environment)
        return self.get_motion_trajectory(behavior_trajectory)
```
- Behavior Planner  
  选择高层驾驶行为，不生成具体的轨迹或转向命令，而是输出行为意图（maneuver）
  - 应该换道、超车还是跟车？
  - 是否通过交叉口？等待红灯还是继续？
  - 应该加速或减速至多少？
- Motion Planner  
  也称为轨迹生成器或本地规划器（Trajectory Planner / Local Planner）  
  接收高层行为指令，生成一条可执行时空轨迹，包含：  
  - 空间路径（x, y）
  - 时间戳 t
  - 速度 v, 加速度 a, 方向变化 δ
### Trajectory
在路径基础上附加了时间、速度、加速度等动态参数的序列，是一段完整的时空参考  

### Navigator
为车辆从当前位置规划到目标位置的一条高层路径（navigation route）  
### navigation route
从起点到终点的几何级别路径，是给定的空间坐标序列，不包含时间信息  

### perception delay（感知延迟） 
从车辆采集环境数据（如摄像头、雷达、LiDAR）到系统作出可用输出（如目标检测、物体状态估计）之间的时间间隔  
### imitation learning
> supervised learning approach in which - in the context of autonomous driving - the behavior of an expert human driver is used as a target signal to supervise the model.

### agent
> AI programs that can make decisions and take actions on their own. One example is self-driving cars. These cars use autonomous agents to make decisions about when to turn, stop, or go based on the information they gather from sensors.

### Raster data 
> pixelated (or gridded) data where each pixel is associated with a specific geographical location

### heading
车子朝向

### Lidar
> determining ranges by targeting an object or a surface with a laser and measuring the time for the reflected light to return to the receiver. 

### point cloud
三维空间里点的集合  

### Topology 拓扑
![image](https://github.com/user-attachments/assets/cb748bae-8522-4802-9404-88d4db8dc232)

### Yaw / Roll / Pitch
Yaw：绕物体的垂直轴（通常是 Z 轴）旋转  
例：汽车左右转弯  

Roll：物体的纵轴（通常是 X 轴）旋转  
例：飞机左右倾斜，导致一侧机翼上升、另一侧下降的动作  

Pitch：绕物体的横轴（通常是 Y 轴）旋转  
例：飞机机头上仰或下俯的动作  

https://howthingsfly.si.edu/flight-dynamics/roll-pitch-and-yaw

### steering angle
车轮中心线与汽车中心线形成的夹角  

### throttle 节气门
制车辆加速的电子信号。自动驾驶系统通过计算所需的加速度，发送相应的"throttle"值（通常在0到1之间）  

### TTC(Time to Collision) 碰撞时间
车辆在当前速度和行驶方向下，与前方障碍物或车辆发生碰撞所需时间  
TTC = 相对距离 ÷ 相对速度  

### lateral acceleration 横向加速度
`a = v² / r`  
为横向加速度，v 为车辆速度，r 为转弯半径。该加速度的单位通常为米每二次方秒（m/s²），在工程实践中也常用重力加速度倍数（g）表示，1g 约等于 9.8 m/s²  

- 车辆操控性评估：高性能车辆在转弯时能承受更大的横向加速度，体现出更强的过弯能力
- 驾驶辅助系统的触发条件：如电子稳定程序（ESP）会根据横向加速度的变化，判断车辆是否出现侧滑，从而进行干预
- 乘坐舒适性分析：过大的横向加速度会导致乘客不适，设计时需在操控性和舒适性之间取得平衡

### curvature 曲率
曲率是曲线偏离直线的量（程度），或是曲面偏离平面的量（程度）  
$$
\kappa = \frac{1}{r}
$$
$\kappa$：曲率（curvature）：表示曲线在某一点的弯曲程度  
$r$：曲率半径（radius of curvature）：即该点处曲线的圆的半径  

对于平面曲线  
$$
\kappa = \frac{|y''|}{\left(1 + (y')^2\right)^{3/2}}
$$
$y'$：$\frac{dy}{dx}$：曲线在该点的斜率  
$y''$：$\frac{d^2y}{dx^2}$：曲线在该点的二阶导数  

### IMU（Inertial Measurement Unit，惯性测量单元）
测量组成：IMU 通常包含三轴加速度计（Accelerometer）和三轴陀螺仪（Gyroscope），有时也集成磁力计（Magnetometer）  
输出内容：它提供车辆在 X、Y、Z 三个方向上的线性加速度 和 旋转速率（角速度），用于估算车辆的运动状态  

## Metrics
### ADE
Average Displacement Error（平均偏移误差）  
planner轨迹与人类驾驶轨迹，各个时间点下的平均欧式距离  

### min_ade
在输出的多条轨迹里，误差最小的那条轨迹的ade  

### FDE（Final Displacement Error）
预测轨迹在指定预测时长结束时，与真实轨迹末端的偏差

### min_fde
在输出的多条轨迹里，误差最小的那条轨迹的fde  

### pred_ade
预测轨迹的ade  

### pred_fde
预测轨迹的fde  

### cls_loss
classification loss 分类损失
这里指预测哪一条轨迹是“最优/目标”轨迹

### reg_loss
模型预测轨迹与真实轨迹之间的回归损失（平滑L1）  
