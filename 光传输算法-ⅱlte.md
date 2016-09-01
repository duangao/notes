# 光传输方程

积分器的任务： 计算LTE的数值解。

LTE求解的困难： GI渲染算法中给定点处的radiance与场景中所有物体的geometry、BSDF等性质都有关系； 而local Illumination中只与局部性质有关。



## 推导1： 应用能量守恒