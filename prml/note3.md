
PRML第二章（3）
---
### 二项分布

假设$x$服从01分布，即：$x(1)=\mu$
将$x$重复可得到二项分布：$Bern(x,\mu)=\mu^{x}{(1-\mu)}^{1-x}$
目标：用训练样本估计$\mu$
1. 频率派做点估计，求到$\mu$值：$p(t|\hat\mu)$
2. 贝叶斯估计，得到$\mu$的分布：$p(t|D)=\int p(t|\theta)p(\theta|D)d \theta$

对于$p(D|\mu)=\prod_{n=1}^{N} \mu^{x_n}(1-\mu)^{1-x_n}$
对分布取log得到：$\log p(D|\mu)=\sum_{n=1}^{N}(x_n\log\mu + (1-x_n)\log(1-\mu))$
将其对$\mu$求导取极值可得到：$\hat\mu=\frac{m}{N}$

由贝叶斯公式我们可以知道：$p(\mu|D) \propto p(D|\mu)p(\mu)=\mu^{m}(1-\mu)^{n}\mu^{a-1}(1-\mu)^{b-1}$

二项分布的共轭先验是Beta分布：$p(\mu|a,b)=\frac{1}{Z}\mu^{a-1}(1-\mu)^{b-1}$
其中归一项$Z$为Beta函数的积分值：$Z=Beta(a,b)=\int_{0}^{\infty}\mu^{a-1}(1-\mu)^{b-1}d\mu=\frac{\Gamma(a)\Gamma(b)}{\Gamma(a+b)} $
Gamma函数为：$\Gamma(x)=\int_{0}^{\infty}\mu^{x-1}e^{-\mu}d\mu$
Gamma函数是级乘在实数上的泛化：$\Gamma(x+1)=x\Gamma(x)$

结论：Beta先验+二项分布数据-->Beta后验
    $Beta(a,b)$ + $Bern(x)$-->$Beta(a+m,b+n)$
    
另外：$E_\theta(\theta)=E_D[E_\theta[\theta|D]]$说明数据量足够大时，频率派=贝叶斯派

## 多元分布

0、1向量编码，如骰子的概率可表示为：$x=(0,0,1,0,0,0)$，参数为：$\mu=(\mu_1,\mu_2,\mu_3,\mu_4,\mu_5,\mu_6)$，其中$\sum_{i=1}^{6}\mu_i=1$

多项分布，又叫多元伯努力分布或多元分类分布。
$p(D|\mu)=\prod_{n=1}^{N}\prod_{k=1}^{K}\mu_k^{x_{nk}}=\prod_{k=1}^{K}\mu_k^{\sum x_{nk}}=\prod_{k=1}^{K}\mu_k^{m_k}$

对其取对数：$\ln p(D|\mu)=\sum_k m_k\ln\mu_k$，且有约束：$\sum_k m_k=N$
根据拉格朗日乘子法得到：$L=\ln p(D|\mu)+\lambda(\sum_k m_k - N)$，对L取极值，得到点估计值：$\mu_k^{ml}=\frac{m_k}{N}$

狄利克雷分布：$Dir(\mu|\alpha)=\frac{1}{Z}\prod_{k=1}^{K}\mu_k^{\alpha_k-1}$
归一化因子：$Z=\frac{\Gamma(\alpha_0)}{\prod_{k=1}^{K}\Gamma(\alpha_k)}$，其中$\alpha_0=\sum\alpha_k$

### 充分统计量

通过数据产生的可以得到与数据有相同效果的统计量，是否具有唯一性？
即：$S(D)==>p(\theta|D)=p(\theta|S(D))$
