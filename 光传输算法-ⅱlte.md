# 光传输方程

积分器的任务： 计算LTE的数值解。

LTE求解的困难： GI渲染算法中给定点处的radiance与场景中所有物体的geometry、BSDF等性质都有关系； 而local Illumination中只与局部性质有关。

待求解的方程：

$$L_o(p,w_o) = L_e(p,w_o) + \int f_r(p,w_o,w_i)L_i(p,w_i) |cos\theta_i| dw_i$$

## 推导1： 应用能量守恒, 统一入射radiance与出射radiance

在没有介质的情况下，p点的入射radiance等于p'点的出射radiance。

![
](/assets/pbrt16-1-09.jpg)

因此，

$$L(p,w_o) = L_e(p,w_o) + \int f_r(p,w_o,w_i)L(t(p,w_i),-w_i) |cos\theta_i| dw_i$$

这一步变换的意义是消除了原方程中$L_o、L_i$的区别，统一为$$L$$

## 推导2： 特例情况下LTE的解析解

这个例子一般用于debug使用。

考虑所有表面都是Lambert表面(brdf = c)，并且光发射的radiance是常量($$L_e$$)。

$$L(p,w_o) = L_e + \int c\  L(t(p,w_i),-w_i) |cos\theta_i| dw_i$$

