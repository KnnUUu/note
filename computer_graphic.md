# 图形学 / computer graphic

参考：[Unity User Manual 2021.3 (LTS)](https://docs.unity.cn/cn/current/Manual/UnityManual.html)

## 渲染管线 / render pipelines

执行一系列操作来获取场景的内容，并将这些内容显示在屏幕上。
- 剔除
- 渲染
- 后期处理

![image](https://user-images.githubusercontent.com/44579350/233566910-6f3f45ef-ed33-46bc-8480-d208ab541a5c.png)

### Vertex Data 顶点数据
顶点坐标、纹理坐标、顶点法线和顶点颜色等顶点属性

### Vertex Shader 顶点着色器
坐标变换计算顶点在屏幕上坐标

### 曲面细分
外壳着色器(Hull Shader) + 镶嵌器(Tessellator) + 域着色器(Domain Shader）

### Rasterization 光栅化
通过3D上的投影计算出屏幕上每个点的颜色

## Normal Map 法线贴图
法线：垂直模型平面的线
通过改变表面法线来影响计算光照的方式，不会对网格的实际多边形性质产生影响

## Phong光照模型
通过计算并叠加三种光照，模拟出较为真实的着色
- Ambient 环境光
- Diffuse 漫反射光
- Specular 高光
