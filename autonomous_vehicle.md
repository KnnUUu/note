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
### open-loop and close-loop
open-loop: 从输入到输出是一连串的，按照设定好的顺序执行，没有反馈。例如空调遥控、洗衣机、马路信号灯    
![image](https://github.com/user-attachments/assets/38c4f5c1-ad6e-452c-93d7-34c01f9dafa4)

close-loop：输出后有反馈来调节输入。例如空调、冰箱、暖炉    
![image](https://github.com/user-attachments/assets/5818211d-b62d-4f88-881b-862054bb778a)

### planner
用于决定车辆行进路线与控制车辆的程序  

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

### yaw
绕物体的垂直轴（通常是 Z 轴）旋转  
例：汽车左右转弯  

### Roll
物体的纵轴（通常是 X 轴）旋转  
例：飞机左右倾斜，导致一侧机翼上升、另一侧下降的动作  

### Pitch
绕物体的横轴（通常是 Y 轴）旋转  
例：飞机机头上仰或下俯的动作  