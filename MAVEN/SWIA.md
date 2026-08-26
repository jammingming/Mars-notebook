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