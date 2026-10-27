## Song et al. 1994
Song, P., Russell, C. T., & Gary, S. P. 1994, JGR, 99, 6011（单星识别低频波模的传输比框架；MAVEN 上的实现见 Ruhunusiri et al. 2015, GRL, 42, 8917）

### 为什么需要传输比

多星编队可以靠静止系色散关系直接分辨波模；**单星有 Doppler 频移歧义**（不知道 $k$，无法把观测频率换算成等离子体静止系频率），所以只能退一步：==用磁场涨落与等离子体矩涨落之间的**幅度关系**（不是色散关系）来判模==。四类基本 LF 波模（fast / Alfvén / slow / mirror）在"横纵比、热压-磁压配分、速度-磁场耦合"上有彼此不同的特征值区间，这就是传输比法的物理基础。

### 前提条件：是 MHD 吗？

==不是"四个 MHD 模"，而是"**3 个 MHD 线性波模 + 1 个动理学不稳定性**"==：fast、Alfvén、slow 是理想 MHD 的线性本征模；**mirror 在理想 MHD 里根本不存在**（MHD 的压强是标量，$T_\perp=T_\parallel$，没有驱动它的自由能），它是动理学（Vlasov）给出的零频率增长模。

#### 三个 MHD 模的前提

均匀背景 $\boldsymbol{B}_0$、$\rho_0$、$p_0$ 下，理想 MHD 线性化给出三个根（$v_A=B_0/\sqrt{\mu_0\rho_0}$，$c_s=\sqrt{\gamma p_0/\rho_0}$，$\theta=\angle(\boldsymbol{k},\boldsymbol{B}_0)$）：

$$
\omega^2=k^2v_A^2\cos^2\theta \quad\text{(Alfvén)}
$$

$$
\frac{\omega^2}{k^2}=\frac{v_A^2+c_s^2}{2}\pm\frac{1}{2}
\sqrt{\left(v_A^2+c_s^2\right)^2-4v_A^2c_s^2\cos^2\theta}
\quad\text{(fast / slow)}
$$

满足以下条件时这"三模图景"才成立：

1. **低频**：$\omega\ll\Omega_{ci}$（离子回旋频率）。否则 Hall 项、电子惯性、动理学色散进来，"三模"会变成哨声波 / 动理学 Alfvén 波 / 离子回旋波；
2. **长波**：$k\rho_i\ll1$（通常还要求 $k\rho_e\ll1$）；
3. **单一流体 + 准中性**，且压强近似**各向同性**（或至少是 CGL 双绝热闭合）；
4. **非相对论**：$v_A,c_s\ll c$（可忽略位移电流）；
5. **线性小振幅**：$\delta B/B_0\ll1$ —— 传输比判据本身就是线性极化关系；
6. **局地均匀**：分析窗口内背景近似不变（卫星在飞，这条在磁鞘里只是"勉强"成立；128 s 窗口内背景已经在演化）。

三模的物理分工（也是判据表的由来）：

| 模 | 压缩性 | 磁场涨落 | 热压 vs 磁压 | 相速度 | 传播方向 |
|---|---|---|---|---|---|
| fast | 强压缩 | $\delta B_\parallel\neq0$ | 同相（$P_R>0$） | $v_f\ge\max(v_A,c_s)$，各向同性 | 任意角 |
| Alfvén | 不可压 | 纯横向 $\delta B_\parallel=0$ | 不参与 | $v_A\cos\theta$ | 沿 $\boldsymbol{B}_0$ |
| slow | 压缩 | $\delta B_\parallel\neq0$ | 反相（$P_R<0$），总压近似守恒 | 最慢，$\theta\to90^\circ$ 时 $v_s\to0$ | 近 $\boldsymbol{B}_0$ 的声学支 |

#### mirror 为什么不算 MHD 模

- 需要在等离子体静止系里**零相速度**（不传播、只增长，故称"不稳定性/结构"而非"波"）；理想 MHD 的色散关系里没有 $\omega=0$ 的压缩解；
- 需要**温度各向异性** $T_\perp>T_\parallel$，高 $\beta$。线性阈值（Hasegawa 1969；Hall 1979）约为

$$\beta_\perp\left(\frac{T_\perp}{T_\parallel}-1\right)>1$$

- 由**镜像力**驱动：磁压升高 → 粒子被"镜像"反射 → 垂直压强继续升高 → 正反馈。观测表现为**磁洞/镜像结构**：$\delta B$ 与 $\delta\rho$ 反相、总压近似守恒（所以 $P_R<0$，和 slow 模同号）；
- 在 CGL（双绝热）流体里能给出定性的流体图像，但**精确阈值与增长率必须动理学**；
- ==slow 与 mirror 的判据**完全相同**（$T_R<1$ 的压缩涨落、$P_R<0$ 的压力平衡），唯一分界是 $D_R$：slow 的相速度能与流速相比（$D_R\ge1$），mirror 在等离子体静止系里零相速度（$D_R\approx0$）==。

#### 这套判据额外要求的"隐式假设"

- **只能工作在 MHD/惯性区**：128 s 窗口 → $f\gtrsim8$ mHz；SWIA 4 s → 奈奎斯特 0.125 Hz。火星磁场弱（$B\sim10$ nT → $f_{ci}\approx0.15$ Hz），==观测频段上端已经很接近 $\Omega_{ci}$，MHD 近似在高端是勉强的==；
- **单星只有幅度关系**：测不到 $k$ 与 $\theta_{kB}$，所以"准平行/准垂直"只能靠 $T_R,C_R,P_R,D_R$ 间接判定；
- **把混合涨落当单一模式**：磁鞘里多支波模共存，方法只给出"主导涨落"的归属；
- **多离子问题**：火星磁鞘有 H⁺ + O⁺/O₂⁺ 拾取离子，严格说是多流体/双离子等离子体，$v_A$ 要用总质量密度，"单一流体 MHD"只是近似；
- **判据表没有 firehose 分支**（$T_\parallel>T_\perp$）：强火管不稳定区会被硬归进现有五类之一（个人理解，作者未讨论）；
- **"slow 模稀少"有两层原因**：① $D_R$ 判据只在内磁鞘/停滞区（$v_s\gtrsim V_0$）才把扰动判成 slow，外磁鞘的慢模会被归为 mirror；② $\beta\sim1$ 的无碰撞等离子体里慢模 Landau 阻尼强，难维持成相干波（两点都是个人理解，作者未讨论）。

### 四个传输比

$$
T_R=\frac{\delta B\cdot\delta B-\delta B_\parallel^2}{\delta B_\parallel^2}
=\frac{\delta B_\perp^2}{\delta B_\parallel^2}
$$

$$
C_R=\frac{\delta P_i^2/P_{i0}^2}{\delta B\cdot\delta B/B_0^2}
$$

$$
P_R=\frac{\delta P_i/P_{i0}}{\delta P_B/P_{B0}},
\qquad P_B=\frac{B^2}{2\mu_0}
$$

$$
D_R=\frac{\delta V\cdot\delta V/V_0^2}{\delta B\cdot\delta B/B_0^2}
$$

| 传输比 | 物理含义 | 主要区分什么 |
|---|---|---|
| $T_R$ | 横向磁场涨落 / 平行磁场涨落 | 横波（Alfvén）与压缩性涨落（mirror、fast） |
| $C_R$ | 热压涨落 / 场强涨落 | 声学性（慢模、快模的压缩部分）与纯磁扰动 |
| $P_R$ | 热压扰动 / 磁压扰动配分 | fast（同相，$P_R>0$）与 slow/mirror（反相，$P_R<0$） |
| $D_R$ | 速度扰动 / 磁场扰动 | 扰动的**相速度**与**流速**之比（量级），区分"真在传播"与"被流搬过去" |

### 判据表（Song et al. 1994, Table 1）

| 波模 | $T_R$ | $C_R$ | $P_R$ | $D_R$ |
|---|---|---|---|---|
| Alfvén + 准平行 slow | >1 | <1 | ⋯ | ⋯ |
| 准平行 fast | >1 | >1 | ⋯ | ⋯ |
| 准垂直 fast | <1 | ⋯ | >0 | ⋯ |
| 准垂直 slow | <1 | ⋯ | <0 | >1 |
| Mirror | <1 | ⋯ | <0 | <1 |

判据的逻辑链，念起来是：

1. 先看 $T_R$：横波（>1）还是压缩（<1）；
2. 压缩的再看 $P_R$：热压与磁压同相（fast，>0）还是反相、压力平衡（slow / mirror，<0）；
3. 反相的再看 $D_R$：$D_R\ge1$ 说明扰动自身相速度可与流速相比（准垂直 slow，真波）；$D_R\approx0$ 说明它在等离子体静止系里驻留（mirror，只是被流搬过去）；
4. Alfvén 与准平行 slow 传输比性质重合，**单星无法分开**，必须合并成一组。

### $D_R$ 到底在比什么：相速度 vs 流速

单星测到的是**航天器系频率**，它是两项叠加：

$$\omega_{sc}=\boldsymbol{k}\cdot\boldsymbol{V}_0+\omega_{\rm plasma}
=k\left(V_0\cos\varphi \pm v_{\rm ph}\right)$$

由于不知道 $k$，"波在等离子体里传播"与"结构被等离子体流搬过去"在单星上看不出区别——这正是 $D_R$ 要绕开的歧义。它不走色散关系，而走幅度关系：由感应方程，扰动的磁场与速度之比由**相速度**定标（几何因子视波模而定，量级上）

$$\frac{\delta v}{\delta B/B_0}\sim v_{\rm ph}
\quad\Longrightarrow\quad
D_R=\frac{(\delta v/V_0)^2}{(\delta B/B_0)^2}\sim\left(\frac{v_{\rm ph}}{V_0}\right)^2$$

**这一步怎么来的**：在等离子体静止系里取单色平面波 $e^{i(\boldsymbol{k}\cdot\boldsymbol{r}-\omega t)}$，理想 Ohm 定律 $\delta\boldsymbol{E}=-\delta\boldsymbol{v}\times\boldsymbol{B}_0$ 代入 Faraday 定律 $\partial_t\delta\boldsymbol{B}=-\nabla\times\delta\boldsymbol{E}$，得

$$\delta\boldsymbol{B}=\frac{1}{\omega}\left[\delta\boldsymbol{v}(\boldsymbol{k}\cdot\boldsymbol{B}_0)-\boldsymbol{B}_0(\boldsymbol{k}\cdot\delta\boldsymbol{v})\right]\quad\Longrightarrow\quad\frac{\delta B}{B_0}\sim\frac{\delta v}{v_{\rm ph}}$$

- 两个特例可手算核对：Alfvén（$\omega=kv_A\cos\varphi$）给 $\delta B/B_0=\delta v/v_A$；垂直传播的 fast（$\omega=kv_f$）给 $\delta B/B_0=\delta v/v_f$——比例常数都是速度，且正是相速度量级；
- 量纲上也只可能这样：$v_{\rm ph}=\omega/k$ 是线性感应方程里**唯一**带速度量纲的量，剩下的只能是 $\cos\varphi$ 之类的 $O(1)$ 几何因子；
- 反过来说，$\delta v$ 与 $\delta B/B_0$ 是**绑死**的，不能独立给定——这正是"不知道 $k$ 也能靠幅度比判波模"的根据；
- 失效处：平行传播的声学支 $\delta\boldsymbol{B}=0$（比值无定义）；mirror 在等离子体静止系里 $\omega=0$，线性波关系不适用，但结构静止就意味着该系里 $\delta v\approx0$，所以 $D_R\approx0$ 是"冻结"的直接结果，不是把 $(v_{\rm ph}/V_0)^2$ 外推到 $v_{\rm ph}=0$ 得来的。

所以：

- $D_R$ **不是**"多普勒效应与动理学效应的比值"。叫 "Doppler" 是因为 $D_R$ 小意味着==仪器看到的时间变化几乎全部来自 $\boldsymbol{k}\cdot\boldsymbol{V}_0$ 的平流==，没有内禀的波动部分；
- **slow 模**：$v_s\neq0$（沿 $B_0$ 的声学支），只要 $v_s$ 能与 $V_0$ 相比，$D_R\gtrsim1$ → 判为真正传播的波；
- **mirror**：等离子体静止系里零相速度，结构"冻结"在等离子体里、被整体带着走，在结构坐标系里没有相对流动（$\delta v\approx0$）→ $D_R\approx0$；
- ==两者都会被流带着走，区别只在于**在等离子体静止系里有没有自己的相速度**==。

**关键推论（容易被忽略）**：外磁鞘里 $V_0\gg v_s$，慢模的 $D_R\sim(v_s/V_0)^2\ll1$，会被判成 mirror。所以 $D_R$ 判据实际上**只在内磁鞘/停滞区（$V_0$ 掉到 $v_s$ 以下）才把 slow 与 mirror 分开**。Cluster 统计正是如此：准垂直扰动绝大多数是 mirror-like（$D_r\approx0$），slow 只出现在日地连线附近和内磁鞘，"因为停滞线上流速降到慢模速度以下"（[Pang et al. 2022, ApJ 940, 120](https://doi.org/10.3847/1538-4357/ac9d2d)）。

### 为什么判据里没有 $T_\perp/T_\parallel$

- 判据表里全是**涨落量之比**（$\delta B_\perp/\delta B_\parallel$、$\delta p$ vs $\delta p_B$、$\delta v$ vs $\delta B$）；而 $T_\perp/T_\parallel$ 是**背景态**的属性，不是涨落关系，所以不进这张表；
- $T_\perp>T_\parallel$ 加 $\beta$ 阈值回答的是"mirror ==为什么会出现=="，不是"==怎么把它认出来=="。识别只靠三条涨落特征：压缩（$T_R<1$）+ 压力平衡（$P_R<0$）+ 不传播（$D_R\approx0$）；
- 本文用的是 SWIA 的标量温度矩，**没有把 $A_T=T_\perp/T_\parallel$ 作为输入**，所以判据里不可能出现它；
- 代价：任何"压力平衡 + 不传播的压缩结构"（不论来自镜像不稳定、还是激波后的熵模、驻波慢模）都会被判成 mirror；反过来弱各向异性也可能因噪声落进同一区间。所以严格的做法是==把 $D_R$ 与 $A_T$、密度-平行磁场相关系数 $\mathrm{CC}(\delta n,\delta B_\parallel)$ 一起看==（[Pang et al. 2022](https://doi.org/10.3847/1538-4357/ac9d2d) 就是 $T_r$、$D_r$、$\mathrm{CC}$、$A_T$ 四者并用）。

### 在 MAVEN 上的实现流程（Ruhunusiri et al. 2015 / Duanmu et al. 2026）

1. SWIA 离子密度、速度、温度（4 s）+ MAG 磁场（32 Hz 平均到 4 s）；
2. 切 **128 s** 窗口，FFT → 逐频率得到 $\delta B,\ \delta V,\ \delta P_i,\ \delta P_B$ 与各自均值；
3. 逐频率算 $T_R,C_R,P_R,D_R$，按上表判成 **(准平行 fast / 准垂直 fast / Alfvén+准平行 slow / 准垂直 slow / mirror)**，得到**波模谱**；
4. ==取磁场功率谱峰值所在频率对应的波模，作为该窗口的"主波模"==；
5. 空间分箱（例如 100×100 km 网格）统计主波模的出现占比，得到**发生率图**。

### 使用时的注意事项

- **频率覆盖很受限**：128 s 窗口 → 最低频率 ≈ $1/128\approx7.8$ mHz；SWIA 4 s 采样 → 奈奎斯特 0.125 Hz。也就是只能覆盖 ULF/Pc5 段，更高频的哨声、离子声波、Langmuir 波完全不在此列，要看高分辨 MAG/波仪器。
- **阈值本身是经验值**：判据是"落在区间内"的硬阈值，磁鞘里多支波模共存时"主波模"由功率谱峰值决定，次要波模信息会被丢掉；换句话说它给的是==主导涨落的性质==，不是"只有这一种波"。
- **对等离子体矩质量敏感**：$\delta P_i$、$\delta V$ 来自 SWIA 矩，低计数（磁鞘热化、或拾取离子占比高的区域）时噪声会系统性抬高 $T_R$、$C_R$。
- **不要把它当"色散关系"用**：传输比只给幅度关系，得到的"波模"是统计意义上的主导模式，不能反推 $k$ 或相速度。
- 用发生率梯度做边界诊断时（Duanmu et al. 2026），==统计边界是长期平均位置，不是单次穿越的固定面==，且低发生率波模（mirror）的"首次跨阈"位置对阈值极其敏感。

## 相关笔记

- 应用实例：[[火星磁层边界相关文章]]（用波模发生率反演 BS / MLB / MPB / ionopause）
- 磁鞘里的低频波谱形态：[[火星上平台状的谱]]、[[火星上Maven观测到的1Hz波]]
- 数据源与采样限制：[[SWIA]]
- 另一种矢量分解方法（边界法向而非波模）：[[MVA]]
- 时间-频率分析：[[Wavelet Analysis]]
