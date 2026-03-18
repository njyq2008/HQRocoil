# HQRocoil-Mini Open-Source Fully Domestic Rogowski Coil Flexible Current Probe

## Video Link:
[Bilibili Video – Performance Demo & Evaluation](https://www.bilibili.com/video/BV18ocuzFEsE/)

## Project Introduction
The Rogowski coil is an AC current measurement sensor based on electromagnetic induction. Due to its unique air-core structure, it offers outstanding advantages such as no saturation, excellent linearity, wide bandwidth response, and minimal intrusion into the circuit under test. It works by detecting the voltage signal induced by the changing current in a conductor (i.e., M*di/dt) and then reconstructing the original current waveform through an integrator circuit. This makes it especially suitable for measuring high-frequency, high-current, and complex signals containing fast transient components. With these characteristics, the Rogowski coil holds an irreplaceable position in modern electrical measurement, widely used in key fields such as power electronic converters, renewable energy generation and energy storage systems, motor drives and control, smart grid monitoring, pulsed power technology, and electrical fault diagnosis. Its wide bandwidth allows accurate capture of switching transients in power devices and analysis of power quality disturbances; its linearity ensures accurate current measurement over a wide dynamic range; and its non-contact measurement method introduces extremely low parasitic effects into the system under test, significantly enhancing measurement credibility. In recent years, with the evolution of wide-bandgap semiconductors such as SiC and GaN toward higher switching speeds and the increasing demand for wide-bandwidth, high-precision measurements in power systems, the application value of Rogowski coils continues to grow in areas such as switching characteristic evaluation, pulsed current analysis, and high-frequency oscillation capture. Combined with modern high-performance analog integrators, Rogowski coils are moving toward higher bandwidth, lower noise, stronger anti-interference capability, and intelligent integration, becoming an important measurement foundation supporting the evolution of next-generation power electronics and smart grid technologies.

![image.png](https://image.lceda.cn/oshwhub/pullImage/60fe3d7117264253b604503882a28a9b.png)

HQRocoil-Mini is an open-source, fully domestic-component-based Rogowski coil flexible current probe. Its goal is to provide an ultra-low-cost, 100% domestically sourced flexible current probe capable of measuring high-frequency AC or pulsed currents in tight spaces. The probe consists of two parts: the coil itself and the integrator. The coil is a customized version from Boniu Technology, optimized to be compact enough to fit between the pins of TO-220 or even smaller packages, and can be slipped over the leads of many through-hole power components for non‑intrusive current measurement. The integrator conditions the signal from the Rogowski coil and provides an output voltage proportional to the current in the conductor encircled by the coil.

The original intention of this open-source project is to compare with imported products costing thousands of dollars, achieve 60-70% of their user experience at less than 5% of the replication cost (under 500 RMB), and give anyone interested in Rogowski coils the opportunity to own their own flexible current probe – even making it a handy tool for debugging motors and testing power devices.

This project fully discloses the hardware design of the Rogowski coil integrator, BOM, enclosure, and panel design. I believe everyone can easily replicate it and use it themselves. If you prefer not to build it yourself, a ready-to-use, debugged finished product can also be obtained on Taobao for 499 RMB. The open-source materials provided here are exactly the same as those used for the final product. I sincerely hope that **it will not be widely used for commercial purposes**. Your support is the greatest motivation for me to provide more interesting and practical open-source solutions!

## References and Acknowledgements
- This project partially references the open-source project westonb/rogowski-relief on GitHub. Respect and thanks to the original author!
- This project is supported by the JLCPCB Spark Plan 2026. Thank you!
- The core component ZGA2011 used in this project is provided free of charge by Zhentuo Semiconductor. Many thanks!

## Project Features
HQRocoil-Mini adopts the classic open-ended flexible Rogowski coil design. One end of the coil can be detached, slipped over the conductor under test, and then reinserted into the coil handle. This allows non‑intrusive current testing without breaking the circuit connection. The integrator provides a sensitivity of 10 mV/A via a BNC output, which can be connected to a 1MΩ oscilloscope input through a coaxial cable or BNC adapter. The rated measurement range is up to 150 A, with a maximum of 200 A. The integrator is powered by a built‑in lithium battery with a runtime exceeding 2 hours. Users only need a coaxial cable to perform measurements – simple and convenient. The carefully designed integrator enclosure even allows direct connection to the oscilloscope port via an adapter, further eliminating the clutter of extra test leads.

Example applications for HQRocoil-Mini:
- Resonant converter tank current measurement
- Inductor current ripple measurement
- Inrush current measurement
- Lightning surge current measurement
- Double pulse testing
- ...

## Project Specifications

### Typical Electrical Performance
- **Bandwidth**: 18 Hz – 20 MHz
- **Sensitivity**: 10 mV/A @ 1MΩ or 5 mV/A @ 50Ω
- **Maximum Current**: ±150 A typ., 200 A max.
- **Recommended Operating Range**: ±(3 A – 150 A)
- **Noise**: 15 mVpp

### Coil
- **Outer Diameter**: ≤ 1.5 mm
- **Length**: 82 mm

### Power Supply
- **Battery**: 500 mAh, 3.7 V
- **Runtime**: 3 hours typ., 2 hours min.
- **Charging**: Type-C, 5 V / 0.5 A

## Hardware Description
### Schematic and PCB Thumbnails
All drawings are A4 size.
![image.png](https://image.lceda.cn/oshwhub/pullImage/a0e79cb13d364c88984531aaf2d8914e.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/79aa8d11e9734a39bc2fe02d59766e6a.png)

It is worth mentioning that the PCB is designed as a 6‑layer board. However, a 4‑layer board (or even a 2‑layer board, though significantly more challenging) would also suffice. The reason for using 6 layers is simply that the author exchanged a mere 20 “gold beans” for a large stack of 150 RMB coupons for 6‑layer boards and felt uncomfortable letting them go to waste.

### Power Supply
The power section is relatively simple, consisting of three parts: battery management, negative voltage generation, and LDOs.
- **Input**: USB Type-C, elegantly terminated with 5.1k resistors.
- **Battery Management**: ETA9697E8A (Yutech) handles battery charging/discharging and converts the battery voltage to 5V for downstream stages. NTC over‑temperature protection is enabled, but no additional battery protection IC is included on the board, so the battery must include its own protection circuit.
- **Negative Voltage Generation**: SGM3204YN6G/TR (SGMICRO) charge pump converts 5V to -5V.
- **LDOs**: GM1501AUJZ-2.5-R7 and GM1206AUJZ-2.5-R7 (Common-Mode Semiconductor) convert ±5V to clean ±2.5V to power the analog signal conditioning circuitry of the integrator.

![image.png](https://image.lceda.cn/oshwhub/pullImage/a38714c907914035a9768c7b4027b368.png)

### Integrator
The integrator consists of three parts: the main integrator, servo feedback, and high‑pass filter with amplification.
- **Main Integrator**:
    - The Rogowski coil is the HQRocoil-Flex-1608-P2R7 from Boniu Technology, connected to the integrator via an IPEX interface. A footprint for a gas discharge tube (GDT) is reserved at the integrator input to provide minimal protection in case of insulation breakdown and high‑voltage discharge.
    - The main integrator input includes a reserved position for a high‑frequency low‑pass filter and an adjustable capacitor. If users employ a different coil, they can adjust this compensation network to achieve a smooth frequency response. The coil selected for this project has an integrated compensation network, so the adjustable capacitor is not needed.
    - The main integrator op‑amp is the ZGA2011G1 from Zhentuo Semiconductor, a low‑noise CMOS operational amplifier designed for high‑precision applications. Its exceptional low‑noise performance (only 0.77 μV from 0.1 Hz to 10 Hz) makes it ideal for noise‑sensitive applications. The ZGA2011 supports a supply voltage from 2.7 V to 5.5 V, ensuring excellent linearity and dynamic performance under various operating conditions. Additionally, it features a gain‑bandwidth product of 28 MHz and a slew rate of 10 V/μs, maintaining stable performance at high frequencies. These outstanding characteristics make it the first choice for a domestically sourced integrator op‑amp, priced at less than 3 RMB.
    - An adjustable resistor is reserved to modify the low‑frequency integration time constant, allowing adjustment of the low‑frequency gain of the Rogowski coil.
    - The integrator op‑amp footprint is SOT23‑6, but the selected op‑amp is SOT23‑5. Users can experiment with other op‑amps to improve noise, bandwidth, or other performance metrics.

![image.png](https://image.lceda.cn/oshwhub/pullImage/f78e9abcc0e94fae9348f98439972c59.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/f38eb75160904bb3975502c488aa4fb7.png)

- **Servo Feedback**:
    - The servo feedback circuit can be thought of as a very slow integrator. From a DC analysis perspective, its goal is to null the DC output of the main integrator. In reality, it implements a low‑frequency high‑pass filter, not only nulling DC but also attenuating low‑frequency 1/f noise of the integrator. For more information, refer to TI’s application note [A new filter topology for analog high-pass filters](https://www.ti.com/lit/an/slyt299/slyt299.pdf).
    - The servo feedback uses the TLV333 from TZC Semiconductor. Many other domestic semiconductor manufacturers offer OPA333 and TLV333 equivalents, which should be interchangeable.

![image.png](https://image.lceda.cn/oshwhub/pullImage/a1d312ad77ea4f16b7afdeb5fb396f5b.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/7280af40349e4f6e97255d1d11e8fd6d.png)

- **High‑Pass Filter with Amplification**:
    - The integrator output is followed by a high‑pass filter with amplification. This stage fine‑tunes the gain and provides additional high‑pass filtering at low frequencies. The op‑amp is the SGM8061 from SGMICRO, a high‑speed voltage‑feedback CMOS op‑amp well suited for this application.
    - An adjustable resistor is reserved to adjust the output gain across the full bandwidth, and a position for gain peaking is provided to compensate for cable attenuation or other effects. When using the coil selected for this project, these compensation features are not needed to achieve a smooth gain curve.

![image.png](https://image.lceda.cn/oshwhub/pullImage/99e5c636a1f0492ca0445975439a24fe.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/669b44cd6cec40caacff5c56b711e08c.png)

## Software Code
This project is a pure analog solution; no software code is involved.

## Important Notes
### Calibration Challenges
The main challenge in calibration is characterizing the frequency‑domain response. For the specific procedure, please refer to the video. The author used a vector network analyzer (VNA) for calibration, measuring S21 and adjusting two potentiometers to achieve the desired smooth curve. When using a VNA, a wideband amplifier is typically needed to boost the signal, as the sensitivity is otherwise too low for the VNA to detect. However, designing a wideband amplifier with a smooth response from a few Hz to hundreds of MHz is no trivial task.

![image.png](https://image.lceda.cn/oshwhub/pullImage/4eb721aec60145c9baae8ca11442ce6d.png)

Alternatively, one could calibrate using a signal generator and oscilloscope. However, this requires a relatively accurate current sensing reference, and because the probe sensitivity is low, you may need to wind a multi‑turn coil to amplify the current under test.

![image.png](https://image.lceda.cn/oshwhub/pullImage/8c3b4e3349f24bd1bf84ec7f28d69376.png)

### Usage Precautions
Since the coil can be damaged and this type of probe is often used to measure exposed high‑voltage conductors (e.g., power device pins), **the oscilloscope must be grounded** for personal safety.

Additionally, because the unit contains a battery, avoid using it in high‑temperature environments. Do not subject the battery to overcharging, over‑discharging, short‑circuiting, puncturing, or other hazardous operations to prevent the risk of battery explosion.

## Assembly Process
### PCBA, 3D Enclosure, and Panel Renderings
![image.png](https://image.lceda.cn/oshwhub/pullImage/54b5f75a8e314bf49e8b5306a8a894c4.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/e44b8bcad3d14ddaa5cf9e675b52dcac.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/e8f3581b903e412eb39f148f34cf1609.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/5168a2c6b75640acb7c16bd1ecf33f05.png)

### Assembly Steps
The enclosure design is generous, so assembly should not pose many difficulties. Follow these steps:
1. Install the M8 cable gland into the threaded hole of the 3D‑printed enclosure.
2. Remove the anti‑deformation supports inside the 3D‑printed enclosure, and insert the 3D‑printed power switch piece into the enclosure.
3. Gently place the mainboard, ensuring the switch button aligns with the notch in the power switch piece.
4. Secure the mainboard to the enclosure with M2 screws.
5. Fix the BNC female connector using washers and nuts.
6. Pass the coaxial cable of the coil through the M8 cable gland and secure the cable within the gland.
7. Connect the coil's IPEX connector to the mainboard.
8. Install the battery on the back side of the mainboard.
9. Debug the mainboard performance, test the frequency response, and calibrate the gain characteristics.
10. Attach the printed front and back panels to the enclosure.

## Photos of the Actual Product
- PCBA
![image.png](https://image.lceda.cn/oshwhub/pullImage/221fef84f9e148118e92aa2af0ec449b.png)

- Coil
![image.png](https://image.lceda.cn/oshwhub/pullImage/68d0cf8f3332499ba673192af362577a.png)

- Complete Unit
![image.png](https://image.lceda.cn/oshwhub/pullImage/9681ffa9f7b1488091c4fdeac3da7c5c.png)

- Video Screenshots (Performance Curves)
    - High‑Frequency Bandwidth Test
    ![image.png](https://image.lceda.cn/oshwhub/pullImage/f02cd99a22c0480d90ffec68de8a9bf3.png)
    - Low‑Frequency Bandwidth Test
      (Note: Blue trace is a commercial probe, red trace is this open‑source project.)
    ![image.png](https://image.lceda.cn/oshwhub/pullImage/c19eaf4f6b1845df9eaf695717014382.png)
    - Noise Floor Test
    ![image.png](https://image.lceda.cn/oshwhub/pullImage/ba0ce08a2b3e4a1da850f365f96f8aab.png)
    - High‑Current Pulse Train – Time‑Domain Waveform Comparison
      (Note: Blue is commercial, red is this project.)
    ![image.png](https://image.lceda.cn/oshwhub/pullImage/71db62f1052b460e8d09ad3fffa46020.png)
    - High‑Speed Current – Time‑Domain Waveform Comparison
      (Note: Blue is a Japanese commercial probe, green is a British commercial probe, red is this project, yellow is the reference. The compared commercial probes have only 30 MHz bandwidth, while the yellow reference shunt has nearly 1 GHz bandwidth, so both are inaccurate. Detailed explanation is in the video.)
    ![image.png](https://image.lceda.cn/oshwhub/pullImage/f7954bdbd5a84ba7a7e16f01a5bc432c.png)

## Cost Breakdown
Cost calculation based on a batch of 5 units, excluding the coil:
- PCB: 6‑layer board, after coupon about 30 RMB/board
- Components: eBOM approx. 57.8 RMB/unit; SMT assembly fees extra
- Enclosure: JLC BLACK, 10.8 RMB/unit
- Panels: Free from JLCPCB (PCB printing)
- **Total: 98.6 RMB** (excluding coil)


# HQRocoil-Mini 开源全国产方案 罗氏线圈柔性电流探头
## 视频链接：
[B站视频-效果展示与性能评测](https://www.bilibili.com/video/BV18ocuzFEsE/)
## 项目简介
罗氏线圈（Rogowski Coil）是一种基于电磁感应原理的交流电流测量传感器，因其独特的空心结构，具备不饱和、线性度好、宽频带响应及对被测电路几乎无侵入等一系列突出优势。它通过检测载流导体电流变化时感生的电压信号（即M*di/dt），再经积分电路还原出原始电流波形，特别适用于高频、大电流及含有快速瞬变分量的复杂信号测量场景。凭借上述特性，罗氏线圈在现代电气测量领域中占据着不可替代的地位，广泛应用于电力电子变换器、新能源发电与储能系统、电机驱动与控制、智能电网监测、脉冲功率技术及电气故障诊断等多个关键领域。其宽频带特性使其能够精确捕捉功率器件开关瞬态、分析电能质量扰动；其线性度确保了大动态范围内电流测量的准确性；而其非接触的测量方式则对被测系统引入极低的寄生效应，显著提升测量可信度。近年来，随着以碳化硅（SiC）、氮化镓（GaN） 为代表的宽禁带半导体器件向着更高开关速度演进，以及电力系统对宽频带、高精度测量需求的日益严苛，罗氏线圈在开关特性评估、脉冲电流分析、高频振荡捕捉等方面的应用价值持续凸显。结合现代高性能模拟积分器，罗氏线圈正朝着更高带宽、更低噪声、更强抗干扰能力及智能化集成方向不断发展，成为支撑新一代电力电子与智能电网技术演进的重要测量基础。

![image.png](https://image.lceda.cn/oshwhub/pullImage/60fe3d7117264253b604503882a28a9b.png)
HQRocoil-Mini是一款开源的、基于全国产器件实现的罗氏线圈柔性电流探头，目标是用超低的成本、100%国产的器件实现一款能用于在狭窄空间内测量高频交流电流或脉冲电流的极细柔性电流探头。探头由线圈本体和积分器两个部分组成。线圈本体采用波妞科技定制的线圈，经过优化，体积小巧，足以穿过TO-220甚至更小封装晶体管的引脚之间，并可套在许多通孔功率元件的引脚上，实现非侵入式电流测量。积分器负责调理来自罗氏线圈的信号，并提供与被罗氏线圈环绕的导体中电流成正比的输出电压。
本开源项目的初衷，是想对比万元级进口产品，以5%不到的复刻成本（500元以内），达到其60-70%的使用体验，让任何想体验罗氏线圈的朋友都有机会拥有一把自己的柔性电流探头，甚至让这款探头能成为大家调试电机、测试功率器件时的称手工具。

本项目完整公开罗氏线圈积分器硬件、BOM、外壳、盖板的设计，相信大家都可以轻松完成复刻并自己使用。如果不想自己做，也可在TB上以499的价格直接获取一套调试好到手可用的成品。
本项目开源的资料和最终生产的是完全一致的，私心希望大家**不要广泛地用于商业目的**，大家的支持是我提供更多有趣实用开源方案的最大动力！

## 参考和致谢
- 本项目部分参考了Github上开源项目westonb/rogowski-relief，向原作者表示敬意和感谢！
- 本项目获得嘉立创星火计划2026支持，感谢！
- 本项目核心器件ZGA2011由徵拓半导体免费提供，非常感谢！


## 项目功能
HQRocoil-Mini采用了经典的开放绕线式柔性罗氏线圈设计，线圈有一端可以抽出，套装在被测导体上，随后插回线圈把手上。因此，无需破坏电路连接，就可轻松实现非侵入电流测试。积分器通过BNC输出提供10mV/A的灵敏度，通过同轴电缆或者BNC转接头连接示波器1MΩ输入进行测量，实现额定150A最大200A的测量量程。积分器内置锂电池，续航超过2小时，用户只需一根同轴线即可进行测试，简单方便。精心设计的积分器外壳更允许用户通过转接头直接连接到示波器端口，可进一步免除多余测试线缆多占空间的烦恼。

HQRocoil-Mini用途举例：
（1）谐振变换器谐振腔电流测试；
（2）电感电流纹波测试；
（3）浪涌电流测试；
（4）雷击电流测试；
（5）双脉冲测试
……

## 项目参数

### 典型电气性能

- **带宽**：18Hz - 20MHz
- **灵敏度**：10mV/A @ 1MΩ 或 5mV/A @ 50Ω
- **最大电流**：±150A typ., 200A max.
- **推荐使用范围**：±(3A~150A)
- **噪声**：15mVpp

### 线圈
- **外径**：≤ 1.5mm
- **长度**：82 mm

### 供电
- **电池**：500mAh，3.7V
- **续航**：3小时typ.，2小时min.
- **充电**：Type-C，5V/0.5A

## 硬件说明
### 原理图和PCB缩略图
图纸均为A4大小
![image.png](https://image.lceda.cn/oshwhub/pullImage/a0e79cb13d364c88984531aaf2d8914e.png)

![image.png](https://image.lceda.cn/oshwhub/pullImage/79aa8d11e9734a39bc2fe02d59766e6a.png)

值得一提的是，PCB为六层板设计，但是实际上本项目用四层板（甚至双面板，但是难度会大不少）完全是可以完成的。之所以使用六层板，是因为作者用区区20金豆兑换了一大把150元六层板券，用不掉难受。

### 电源
电源部分比较简单，由电池管理、负压生成、LDO三部分组成。
- 输入：USB Type-C，优雅5.1k。
- 电池管理：钰泰ETA9697E8A负责电池充放电管理，同时可将电池电压转换为5V电压供后级使用，NTC过温保护功能启用，但板上没有额外添加电池保护IC，因此电池需带保护板。
- 负压生成：圣邦SGM3204YN6G/TR电荷泵实现5V向-5V的转换
- LDO：共模半导体GM1501AUJZ-2.5-R7和GM1206AUJZ-2.5-R7实现±5V电压向纯净的±2.5V电压的变换，用于给积分器模拟信号调理电路供电。

![image.png](https://image.lceda.cn/oshwhub/pullImage/a38714c907914035a9768c7b4027b368.png)

### 积分器
积分器由三部分组成：主积分器，伺服反馈，高通滤波放大。
- 主积分器：
    - 罗氏线圈采用波妞科技的HQRocoil-Flex-1608-P2R7，通过IPEX接口接到积分器。积分器输出有预留空气放电管位置，当绝缘皮破损、发生高压放电时GDT能提供微量保护。
    - 主积分器输入预留一个高频的低通滤波位置，同时预留了可调电容位置，用户如果不采用上述线圈，可自行调节这里的补偿网络以获得平滑的频响特性。本项目所选的线圈内部集成了补偿网络，不需要可调电容。
    - 主积分器运放采用徵拓半导体的ZGA2011G1，这是一款专为高精度应用设计的低噪声CMOS运算放大器。其卓越的低噪声特性（0.1Hz至10Hz 时仅为 0.77μV）使其成为对噪声敏感应用的理想选择。ZGA2011支持2.7V至5.5V电源供电，确保在各种工作条件下都能提供出色的线性度和动态性能。此外，它拥有28MHz的增益带宽积和10V/μs 的压摆率，能够在高频应用中保持稳定的性能表现。这些优异的特性使得其成为积分器的国产首选。价格仅不到3元。
    - 积分器预留一个可调电阻，可以调节低频积分时间常数，用于调整罗氏线圈的低频增益。
    - 积分器运放保留了SOT23-6，选择的运放是SOT23-5的，用户可以自行尝试更换其他运放来提升噪声、带宽等性能指标。

![image.png](https://image.lceda.cn/oshwhub/pullImage/f78e9abcc0e94fae9348f98439972c59.png)


![image.png](https://image.lceda.cn/oshwhub/pullImage/f38eb75160904bb3975502c488aa4fb7.png)


- 伺服反馈：
    - 伺服反馈电路可简单看成一个很慢的积分器，从直流分析角度直观理解，它控制的目标是让积分器的输出直流调零；当然实际上它是实现了一个低频的高通滤波，不仅直流调零，还能削弱积分器的低频1/f噪声。更多信息可以参考TI的应用手册[A new filter topology for analog high-pass filters](https://www.ti.com/lit/an/slyt299/slyt299.pdf)
    - 伺服反馈采用了台舟半导体的TLV333，当然很多其他国产半导体厂商都有OPA333和TLV333，应该可以替换使用。

![image.png](https://image.lceda.cn/oshwhub/pullImage/a1d312ad77ea4f16b7afdeb5fb396f5b.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/7280af40349e4f6e97255d1d11e8fd6d.png)
- 高通滤波放大：
    - 积分器的输出接一级高通滤波放大器，这一级用于微调增益，同时进一步提供低频段的高通滤波。运放采用圣邦SGM8061，这是一颗高速电压反馈CMOS运放，十分适合在此处应用。
    - 放大器预留一个可调电阻，可以全频段调节输出增益，并且预留了做增益隆起的位置用于补偿线缆衰减等。当采用本项目所选的线圈时，不使用这些预留的补偿也可实现较为平滑的增益曲线。

![image.png](https://image.lceda.cn/oshwhub/pullImage/99e5c636a1f0492ca0445975439a24fe.png)

![image.png](https://image.lceda.cn/oshwhub/pullImage/669b44cd6cec40caacff5c56b711e08c.png)



## 软件代码
本项目为纯模拟方案，无软件代码。

## 注意事项
### 调试难点
调试难点在于如何标定其频域特性。具体操作方法可以参考视频。
作者采用矢量网络分析仪进行标定，通过测量S21并调整两个可调电位器来达到想要的平滑曲线。当使用VNA标定时，一般需要自己做一个宽带放大器来提高增益，不然会因为灵敏度太低，矢网测不出来。不过特性平滑的兼顾数Hz到数百MHz的宽带放大器可不太好做：）

![image.png](https://image.lceda.cn/oshwhub/pullImage/4eb721aec60145c9baae8ca11442ce6d.png)
大家也可以考虑用信号源+示波器进行标定，但是首先需要有一个比较准的电流检测装置才行，其次，探头灵敏度比较小，大家可能需要绕制一个多匝的线圈来放大被测电流。

![image.png](https://image.lceda.cn/oshwhub/pullImage/8c3b4e3349f24bd1bf84ec7f28d69376.png)

### 使用注意事项
由于线圈有破损的可能，且这种探头常用来测量高压裸露导体（比如功率器件的管脚），使用时为确保人身安全，示波器必须接地。
此外，由于内部有电池，不宜在高温环境下使用，也不要对电池进行过充过放短路针刺等一系列危险操作，以防电池爆炸。

## 组装流程
 ### PCBA、3D外壳、面板示意图
 
![image.png](https://image.lceda.cn/oshwhub/pullImage/54b5f75a8e314bf49e8b5306a8a894c4.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/e44b8bcad3d14ddaa5cf9e675b52dcac.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/e8f3581b903e412eb39f148f34cf1609.png)
![image.png](https://image.lceda.cn/oshwhub/pullImage/5168a2c6b75640acb7c16bd1ecf33f05.png)

### 组装步骤
外壳设计十分宽松，组装不应遇到太多困难。组装步骤如下：
（1）将M8葛兰头安装在3D打印的外壳螺纹处；
（2）剪除3D打印外壳内的防变形支撑，将电源开关3D打印件嵌入外壳；
（3）轻轻放下主板、保证开关按钮与电源开关打印件缺口对齐；
（4）用M2螺丝固定主板与外壳；
（5）用华司、螺母固定BNC母座；
（6）线圈本体的同轴电缆套过M8葛兰头，并将线缆固定在葛兰头内；
（7）将线圈IPEX接头扣在主板上
（8）主板背面安装电池；
（8）调试主板性能，测试频响曲线，标定增益特性；
（9）将打印的正反面板粘贴在外壳上。



## 实物图
- PCBA

![image.png](https://image.lceda.cn/oshwhub/pullImage/221fef84f9e148118e92aa2af0ec449b.png)

- 线圈

![image.png](https://image.lceda.cn/oshwhub/pullImage/68d0cf8f3332499ba673192af362577a.png)

- 整机

![image.png](https://image.lceda.cn/oshwhub/pullImage/9681ffa9f7b1488091c4fdeac3da7c5c.png)


- 视频截图展示
这里主要是想展示性能实测曲线，但是当时拍视频时太懒，没有留原图，只好视频里截图了。
    - 高频带宽测试
    
![image.png](https://image.lceda.cn/oshwhub/pullImage/f02cd99a22c0480d90ffec68de8a9bf3.png)
    - 低频带宽测试
      （注：蓝色为商用探头，红色为本开源项目）
    
![image.png](https://image.lceda.cn/oshwhub/pullImage/c19eaf4f6b1845df9eaf695717014382.png)
    - 底噪测试
 
![image.png](https://image.lceda.cn/oshwhub/pullImage/ba0ce08a2b3e4a1da850f365f96f8aab.png)
    - 大电流脉冲序列-时域波形对比
  （注：蓝色为商用，红色为本项目）
![image.png](https://image.lceda.cn/oshwhub/pullImage/71db62f1052b460e8d09ad3fffa46020.png)

    - 高速电流-时域波形对比
  （注：还上蓝色为日本品牌商用，绿色为英国品牌商用，红色为本项目，黄色为基准；所对比商用探头均只有30M带宽，黄色基准分流器有近1GHz带宽，因此都测不准，详细解说见视频）
![image.png](https://image.lceda.cn/oshwhub/pullImage/f7954bdbd5a84ba7a7e16f01a5bc432c.png)


## 成本核算
成本核算按5套计算，不含线圈
 - 线路板：6层板，券后约30元/块
 - 器件：eBOM约57.8元/套，SMT费用需另计
 - 外壳：JLC BLACK，10.8元/套
 - 面板：立创免费打
 总计：98.6元