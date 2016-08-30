# Russian Roulette

这是用于提高蒙特卡罗采样效率的方法，通过忽略贡献较小的采样，来使得更多的采样集中在更为“重要”的采样上。

该方法并不能减小方差， 甚至如果概率选取不当，会增加方差。

以direct light 求解来说明其原理：

$$L_o(p,\omega_o) = \int f_r(p,\omega_o,\omega_i) L_d(p,\omega_i) |cos\theta_i| d\omega_i $$

对应的monte carlo estimator 为：

$$F_N = \frac{1}{N} \Sigma_{i=1}^N f_r(p,\omega_o,\omega_i) L_d(p,\omega_i) |cos\theta_i| / p(\omega_i) $$

引入Russian Roulette后：

$$ F_{N}^{'} =  (F_N - qc) / (1-q) , \xi > q$$ 

$$     = c, otherwise$$

显然 $$E[F_N^{'}] = E[F_N]$$



