## 仪器的三级产品（每 4 秒一个完整扫描周期内，FPGA 同时产出）

|产品|内容|去向|
|---|---|---|
|**P0（Raw）**|完整 96 能量 × 24 偏转 × 24 阳极 全分辨率数组|只送 PFDPU，**正常不下行**|
|**P1（Coarse）**|16 φ × 4 偏转 × 48 能量（10 个小阳极两两合并成 16 个 22.5° bin、6 个偏转合并 1 个、相邻能量两两相加）|Sheath 模式下行|
|**P2（Fine）**|10 φ × 12 偏转 × 48 能量|Solar Wind 模式下行|

## Fine 算法（P2）的具体流程——这是"仪器自己的算法"

SIS 文档原文（`maven_sis_SWIA-r1_3` 电子学一节）：

> _"The P2 product **buffers all small anode counts for an entire cycle**, **finds the peak count rate**, and sends back the small anode count rates for **48 energy steps and 12 deflection steps around the location of the peak** to the PFDPU, with a delay of 4 seconds from data acquisition during which the peak-search and buffer selection occurs."_

翻译成流程就是：

1. **攒一个完整 4 秒周期**的 10 个细阳极（4.5° 分辨率）计数，即 96 能量 × 24 偏转全表；
2. **找峰值**：在这个 (能量, 偏转) 表里定位计数率最大处（= 太阳风束的位置）；
3. **切窗口**：只把峰值附近 **48 能量档 × 12 偏转档**发下去（所以 fine 文件是 [10, 12, 48]，而不是全分辨率）；
4. **下一帧窗口跟着束走**：这就是为什么 fine 文件里每帧的 `estep_first`/`dstep_first` 都不一样（2015-02-26 那天中位数 ~26/5）——**窗口是滑动跟踪的**。

## SWIA 模式（Solar Wind / Sheath）判断机制

## 判据一句话版

**飞行软件实时监视 P1（Coarse）与 P2（Fine）两个产品的总计数之比**：

- **P2 里集中了大部分计数**（窄的强束，能量/角度分布局促）→ 切 **Solar Wind 模式**；
- **大量计数落在 P2 覆盖窗之外**（分布扩散、热化）→ 切 **Sheath 模式**。

### 来源 ①：SIS 文档（`maven_sis_SWIA_r1.3.pdf`，第 2.5 节 Operational Modes）

> "The switch from 'Solar Wind' to 'Sheath' modes is **internally triggered, based on the ratio of the total counts contained in the P1 product to that in the P2 product**. If most of the counts are contained in P2, the telemetry switches to 'Solar Wind' mode. If the counts are more distributed, with significant counts not contained in the narrow energy/angle range covered by P2, then the telemetry switches to 'Sheath' mode. The telemetry mode switches are based on **two programmable count ratio thresholds**, and are only allowed to occur at a **programmable 2^n interval**..."

关键点：**阈值是两个可编程参数**（SIS 未给数值），切换有防抖间隔。

### 来源 ②：Halekas et al. 2015 论文（`halekas2015_swia_paper.pdf`，4.1.3 节 Attenuator Control and Mode Switching）——补充了关键数值

> "In order to determine when to switch between 'Solar Wind Mode' and 'Sheath Mode' ... the software monitors **the ratio between the total number of counts contained in the P1 product and the P2 product**, switching to the former if P2 contains most of the counts (a narrow distribution), and to the latter if P1 contains many counts not in P2 (a more spread-out distribution). This mode control utilizes **programmable thresholds**, and occurs subject to a **programmable minimum interval between mode changes (4 × 2^N seconds)**. As for the attenuator control, **we typically set N = 7, corresponding to at least 512 seconds between mode changes**."

补充细节：

- **切换最小间隔 = 4 × 2^N 秒，实际设置 N=7 → 至少 512 秒（~8.5 分钟）才能切一次**（防止在振荡边界来回抖动）；
- **衰减器**同理：监视 **P2 峰值计数率**，超/低于两个可编程阈值就开/关，间隔也是 4×2^N（N=7 → 512s）+ 硬件 ~5 分钟超时 + 最大动作次数限制；
- **P2 窗口本身（48 能量 × 12 偏转）的 framing 也是可编程的**，可调窗口位置/大小；
- 论文还说阈值参数计划在轨调优（"We plan to tune mode and attenuator threshold parameters..."）——**所以具体比值数值从未公开**，它是飞行软件里的可配置参数。