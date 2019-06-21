[TI雷达视频教程地址](http://training.eeworld.com.cn/TI/show/course/4132)

> 公式符号说明

| 符号      | 说明                        |
| --------- | --------------------------- |
| $v_{max}$ | 雷达可测最大速度            |
| $v_{res}$ | 速度分辨率                  |
| $d_{res}$ | 距离分辨率                  |
| c         | 光速                        |
| B         | 雷达带宽                    |
| $T_c$     | Chirp持续时间               |
| $T_f$     | Frame(N个Chirp)总的持续时间 |



# 1.1 Range estimate 距离估计
## FMCW原理
* TX与RX时间差为电磁波在雷达与目标间来回的时间
## TX/RX IF
* TX: 发送的雷达波
* RX: 接受的雷达波
* IF: mixer(混频器)混合TX和RX后的波
* ADC: 波先进行低通滤波，然后由 ADC 进行数字化。ADC采样率应具有高于IF_max的采样率

## Range-FFT
一个Chirp中进行，根据IF的(频率)$\omega$计算出距离

## Range resolution(距离分辨率/精度)

* 根据公式计算分辨率 dres = c/2B,B为雷达发送波的带宽，分辨率仅仅与带宽有关
* Chirp持续时间折衷(trade-off),更快或ADC更小的采样率

# 1.2 Phase of IF signal IF信号相位意义

在两个连续的线性调频脉冲之间测量的相位差可用于估算物体的速度

# 1.3 Doppler estimate 速度估计
> 速度估计需要用到多(N)个相邻线性调频脉冲之间的数据
## 分辨单个物体的不同频率(用多普勒原理求速度)
* 序列长度越长，累计的相位差越大，分辨率就越高。(速度太大，相位差超过$\pi$则会混淆)
* 在两个连续的线性调频脉冲之间测量的相位差即可用于估算物体的速度。

## 分辨多个物体
* 只要物体与雷达距离不同，两个Chirp计算便可用于多个物体
* 若距离相同，则需要使用N个相邻线性调频脉冲(Chirp)序列来计算
## Doppler-FFT
* 多个Chirp之间进行的FFT,根据相位差计算速度
* 长度为N的序列，速度分辨率为 $v_{res}  = \frac{\lambda}{2NT_c}$ ,其中 $T_c$为相邻线性调频脉冲之间的间隔，总时间(即帧时间$T_f$)为$NT_c$

# 1.4 System design
## 2D-FFT
* 每个Chirp进行Range-FFT的结果存入桶(bin)中,