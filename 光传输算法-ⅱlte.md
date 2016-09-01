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

$$\int |cos\theta_i| dw_i = 4\pi$$

故原积分为：
$$L = L_e + c\pi L$$

将L迭代代入($$c\pi = \rho_{hh}, 为Lambert反射的反射率$$)：

$$L = L_e + \rho_{hh} (L_e + \rho_{hh} ( L_e + ... = \Sigma_{i=1}^{\infty} L_e \rho_{hh}^i$$

其中$$\rho_{hh} < 1$$，故上述级数收敛于$$\frac{L_e}{1-\rho_{hh} }$$


## 推导3： LTE的"三点式"形式

定义p'处出射radiance为$$L(p',w_o) = L(p'->p)$$, 其中p更靠近相机film
定义p'处入射radiance $$L(p',w_i) = L(p''-p')$$,其中p'' 更靠近光源。
定义p'处的BSDF $$f(p',w_o,w_i) = f(p''->p'->p)$$

![](/assets/pbrt16-1-15.jpg)


下面将原积分从方向积分转换为表面上面积的积分，微元之间的雅可比行列式为$$|cos\theta '|/ r^2$$

将原方程中cos项以及上述雅可比行列式以及可见性检测合为一项：

$$G(p'' <-> p') = V(p'' <-> p') cos\theta |cos\theta ''| / (|p''-p'|^2)$$

代入后得到：

$$L(p'->p) = L_e(p'->p) + \int_A f(p'' -> p' -> p) L(p''->p) G(p'' <-> p) dA(p'')$$


## 推导4： 通过函数迭代，将LTE转换为路径上的积分