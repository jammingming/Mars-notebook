## Sonnerup 1998
目的：对矢量时间序列（最常用磁场 $\boldsymbol B(t)$）做特征分解，找到一组正交坐标系，使得矢量在中间方向方差最大，最小方向方差最小；用于确定间断面法向（磁重联电流片、弓激波、行星际间断）。
物理图像：间断面法向对应的磁场分量波动最小（法向磁场近似恒定），切向分量波动大。

1. 基础定义
设观测矢量样本集合 $\boldsymbol B_i = (B_{x,i},B_{y,i},B_{z,i}),\ i=1,\dots,N$
样本均值：
$$\langle \boldsymbol B\rangle = \frac1N\sum_{i=1}^N \boldsymbol B_i$$
偏差矢量：$\boldsymbol b_i = \boldsymbol B_i - \langle \boldsymbol B\rangle$

方差矩阵（实对称 3×3）
$$M = \langle \boldsymbol b \boldsymbol b^T \rangle =
\frac1N \sum_{i=1}^N
\begin{pmatrix}
b_{x,i}b_{x,i} & b_{x,i}b_{y,i} & b_{x,i}b_{z,i}\\
b_{y,i}b_{x,i} & b_{y,i}b_{y,i} & b_{y,i}b_{z,i}\\
b_{z,i}b_{x,i} & b_{z,i}b_{y,i} & b_{z,i}b_{z,i}
\end{pmatrix}$$
$M$ 是实对称矩阵，一定存在**3个实本征值+正交本征矢**。

2. 特征分解与 MVA 坐标系
求解本征方程：
$$M \boldsymbol e = \lambda \boldsymbol e$$
得到三个本征值，按从大到小排序：
$$\lambda_1 \ge \lambda_2 \ge \lambda_3$$
对应正交单位本征矢量 $\boldsymbol e_1,\boldsymbol e_2,\boldsymbol e_3$：
- $\boldsymbol e_1$：最大方差方向（max variance），波动最强
- $\boldsymbol e_2$：中间方差方向（intermediate）
- $\boldsymbol e_3$：最小方差方向（min variance） → 间断面法向 $\boldsymbol n$

MVA 核心假设：间断面是平面，法向磁场分量 $B_n=\boldsymbol B\cdot\boldsymbol n$ 几乎不变，因此法向方向方差最小。
MVA 坐标系 $(\boldsymbol e_1,\boldsymbol e_2,\boldsymbol e_3)$ 构成右手正交基。

3. 品质判据（Sonnerup 原文关键判据，用来判断 MVA 结果是否可信）
这是做 MVA 最容易忽略的部分，直接决定法向能不能用。

比值 $\lambda_2/\lambda_3$
- $\lambda_2/\lambda_3 \gg 1$：好。推荐阈值：$\boldsymbol{\lambda_2/\lambda_3 > 10}$（文献常用）
- 若 $\lambda_2/\lambda_3$ 接近 1：最大、最小方差方向区分不开，MVA 失效，法向不可信。

物理含义：最小方差方向必须显著比中间方向波动更小，才能可靠识别出面法向。
$\lambda_1/\lambda_2$：表征切向两个方向的波动差异，间断面一般不需要这个很大。

原文提醒：MVA 只适合平面间断；如果结构弯曲、时变、非一维，MVA 法向会有很大偏差。

4. 误差估计（Khrabrov & Sonnerup 1998 配套论文，MVA 必备）
本征矢角度误差（法向 $\boldsymbol e_3$ 的不确定度）：
$$\delta\theta \approx \sqrt{\frac{\lambda_3}{\lambda_2-\lambda_3}}$$
单位：弧度。$\lambda_2$ 越远大于 $\lambda_3$，角度误差越小。

Sonnerup & Scheible 章节还区分两种 MVA：
- 普通 MVA：对磁场矢量 $\boldsymbol B$，最常用；
- MVAB：就是上面这个磁场 MVA；
- 另外还有 MVA 对流速 $\boldsymbol V$（MVA-V），用于等离子体速度间断。

5. 重要原文注意事项（直接摘抄章节要点）
- 样本窗口选择非常关键：窗口必须刚好包含间断结构，不要混入过多前后背景；窗口太长会包含多个结构，破坏一维平面假设。
- 均值是窗口内样本均值，不要用全局背景场。
- 本征矢方向存在符号歧义：$\boldsymbol e_3$ 和 $-\boldsymbol e_3$ 都是合法最小方差方向，需要额外物理约束确定法向指向（比如利用通量、激波上下游）。
- 磁场噪声会抬高 $\lambda_3$，降低 $\lambda_2/\lambda_3$，劣化法向
