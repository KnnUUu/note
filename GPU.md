# general
### shader 着色器
既不是单纯属于游戏引擎的程序，也不是 GPU 硬件本身，而是运行在 GPU 上的一段小程序  
通常用 GLSL、HLSL、Metal Shading Language 或者引擎自带的 Shader Graph 等方式编写  

### host processor
Host一般指cpu  
device一般指gpu  

### kernal
a function/program that runs on the GPU in parallel across many threads.  

### atomic
不可分的操作  
具体为在该操作完成前其他操作无法插入  

### reduction operation（归约操作）
把一组数据通过某种运算规则，逐步缩减为一个结果的过程  
加法：[1,2,3,4]→10  
乘法：[2,3,4]→24  

### SPM（scratch-pad memory） 暂存存储器/手动管理缓存
一种由程序员显式控制的高速本地存储器  

### SPI（Shader Processor Input） 
GPU 前端硬件中的一个模块，负责 把外部输入数据分发给 shader cores  

### SQC, I-cache, K-cache
Shader Queue Controller： 管理/调度器，负责指令与数据请求  
I-cache：指令缓存，放着要执行的 shader 代码  
K-cache：存放常量数据，很多线程要用的统一参数就放这里  

### Shader Resource Table（SRT） 着色器资源表
存放着色器可访问的纹理、缓冲区、常量等资源的索引  

### Accumulator（ACC） 累加器
CPU 或 GPU 中用于存放中间计算结果的寄存器  

---  

# CUDA
warp：GPU执行thread的单位，1warp=32thread  
Streaming Multiprocessor：处理Block的硬件，block 分配给某个 SM，一个 block 内的多个 warp 被调度器执行  

---  

# AMD
### Streaming Wave Coalescer (SWC)
把多个来自不同 wave 的内存访问请求合并（coalesce），打包成更少、更高效的内存事务  

### LSB(Least Significant Bit) 最低有效位
一个二进制数中最右边的一位  
代表数值中权重最小的部分（也就是0或1）  

### SCC（Scalar Condition Code） 标量条件码
保存比较运算结果，用于后续的分支或条件执行  

### work-item
aka thread, aka lane  
lane主要在表述VALU操作时使用  

### wave
一组（32或64个）work-item组成  

### VGPR(Vector General-Purpose Register)
保存计算用数据寄存器  
每个thread占有4byte的空间，总共4byte x work-item里的thread数，私有互相之间无法访问  

### SIMD(Single Instruction Multiple Data)
用于计算单个wave的vector ALU单元  

### Workgroup
一组可以互相同步的wave的集合，通过LDS共享数据  

### WGP(Workgroup Processor)
处理Workgroup的单位，包含scalar跟vector ALU，LDS，scalar caches  

### LDS(Local Data Share)
分配给waves或者workgroups的内存

---  

# Render
### 光流（Optical Flow）
每个像素在相邻帧之间的移动方向和速度  
通常用一个二维向量场表示，每个像素对应一个矢量，表示水平方向和垂直方向的位移  

### 运动向量（Motion Vector）
图像块在连续帧之间的位移。它用来表示某个区域从前一帧到后一帧的运动情况  

与光流的区别
- 光流 (Optical Flow)：精细到每个像素的运动估计，通常是稠密的矢量场。  
- 运动向量 (Motion Vector)：通常是视频编码里使用的，以块为单位的运动估计，不一定覆盖所有像素。  

### 泛光/辉光效果
后期渲染特效，模拟强光源在摄像机镜头或人眼中产生的光晕  
提取亮部区域 → 模糊扩散 → 叠加回原图  

### Upshuffle/Downshuffle & UnSampling/DownSampling （上采样/下采样）
图像放大缩小  
