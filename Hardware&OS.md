# Hardware
## 冯诺依曼模型 von Neumann architecture
- 电脑由运算器（arithmetic logic unit）、控制器（control unit）、存储器（memory）、输入设备、输出设备组成  
- 运算器集成在控制器里  
![image](https://github.com/KnnUUu/note/assets/44579350/a067ebc0-6a35-484e-a57f-a223a5e15e2e)
## memory
- 内存的地址是从 0 开始编号的，然后自增排列，最后一个地址为内存总字节数 - 1  
## CPU
- 32 位 CPU 一次可以计算 4 个字节
- 64 位 CPU 一次可以计算 8 个字节
- 寄存器（register）用于存储计算时的数据，比内存更快  
- 控制单元（control unit）用于控制CPU工作  
- 逻辑计算单元（Arithmetic/logic unit）负责计算  

- 参数
  - Socket Type  
    determines whether the CPU can be physically installed on the motherboard  
  - cores  
    number of cores affects multitasking and the ability to handle multiple processes simultaneously  
  - Threads  
    Each core can handle multiple threads  
  - Clock Speed  
    Measured in GHz(Gigahertz), this indicates the speed at which the CPU   
    Base Clock Speed: speed under normal conditions  
    Boost/Turbo Clock Speed: The maximum speed the CPU can achieve under load. Can cause over heat and shorter life span  
  - L1, L2, and L3 Cache  
    small amounts of very fast memory located on the CPU
  - TDP (Thermal Design Power)  
    Measured in watts, this indicates the amount of heat the CPU generates and how much cooling is required.  

# OS
- For PC: Windows / Max OS  
- For Smart Phone: IOS / Android    
- For Server: Linux  
与unix关系？  

## Virtual Machine (VM)
a compute resource that uses software instead of a physical computer to run programs and deploy apps. One or more virtual “guest” machines run on a physical “host” machine.  Each virtual machine runs its own operating system and functions separately from the other VMs, even when they are all running on the same host.   
Common VM: VMware, VirtualBox, Hyper-V  

## Docker
将应用以及运行环境一并打包部署，解决了在本地开发的软件无法在其他环境里运行的问题  
Containers are like lightweight, portable virtual machines. They package up an application and all its dependencies (like libraries, system tools, and settings) so that it can run consistently across different environments.  

- VPS(virtual private server): splits a server into multiple virtual private servers  
- ECS(Elastic Compute Service): Similar to VPS but user can resize base on computing resourse they need  
- 

### Concurrency computation 并发 & Parallel computation 并行  
concurrency ：再复数个任务之前快速切换执行  
Parallelism: 同时执行复数个任务  
![Concurrency_and_Parallel](https://github.com/KnnUUu/note/assets/44579350/bb5160cc-0a8f-40c7-a501-9d44887bbd85)
