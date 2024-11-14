---
{"dg-publish":true,"permalink":"//blog/log//matlab/"}
---

[[日常/BLOG/学习log/雷达/雷达\|雷达]]



## 测量距离
总的思路就是：分别测量发出信号和反弹之后的返回信号的相位，通过 θ=2Πft 可以知道发出信号与返回信号的时间差，若再知道信号波的速度c，也就可以知道反弹物体和雷达之间的距离d了，即
$$
2d=ct---------------------------（1）
$$
此处是2d是因为这个时间t包含了过去和反弹的两端距离。
<font color=yellow>但是</font>这种方法具有很大的限制，他要求发出信号和返回信号之间的相位变化一定要在一个周期内，而对于雷达信号的频率来说，一个周期内信号传播的距离也就几厘米。并不能实际使用。
#### 实际数据处理
另外实际上，在实际信号处理中对chirp信号做1DFFT，其中频域图的峰值点所对应的频率即为中频频率。
![Pasted image 20241102151723.png|400](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E5%AE%9E%E9%AA%8C%E5%AE%A4%E9%A1%B9%E7%9B%AE/%E9%9B%B7%E8%BE%BE/%E7%90%86%E8%AE%BA/Pasted%20image%2020241102151723.png)
获得中频频率之后，就可以计算出距离R
![Pasted image 20241102151903.png|600](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E5%AE%9E%E9%AA%8C%E5%AE%A4%E9%A1%B9%E7%9B%AE/%E9%9B%B7%E8%BE%BE/%E7%90%86%E8%AE%BA/Pasted%20image%2020241102151903.png)

还可以用==距离分辨率==来算，更方便。
### 测距范围
由上图可以确定出混频得到的中频频率，
$$
 f = B/T*t=S*t=2*d*S/c  ----------------（2）
$$
T为一个chirp信号的持续时间，B为带宽，t为接收信号的延迟时间。
### 距离分辨率
由公式（2）可以推得距离分辨率与频率分辨率的关系为
$$
🔺f =2*🔺d*S/c  ----------------（2）
$$
### 速度分辨率(不考虑物体速度)
速度分辨率可以从FFT来推到，即
$$
	🔺f = fs/N1
	
$$
$$
N = fs*T
$$

其中fs为采样频率，N1为fft点数，N为采样点数，T为时宽即chirp持续时间。
则距离分辨率为 
$$
🔺d = fs*c/(N1*2*S)= fs*c*T/(N1*2*B)=N*c/(2*N1*B)
$$
当N = N1时，即采样点数等于fft点数有
$$
🔺d = c/(2*B)
$$

==PS:补充一点，快时间维度的意思就是距离维度，慢时间维度就是速度（多普勒）维度==
## 测量速度
混频器输出的信号为中频信号，它的初始相位就是发送和接收信号的相位差
![Pasted image 20241015140318.png](/img/user/Pasted%20image%2020241015140318.png)
![Pasted image 20241015140329.png](/img/user/Pasted%20image%2020241015140329.png)
🔺┏时往返的时延。
所以两个连续的chirp信号的相位差可以用来估计物体速度v，设两chirp信号时间间隔为TC
![Pasted image 20241015140543.png](/img/user/Pasted%20image%2020241015140543.png)
![Pasted image 20241015140612.png](/img/user/Pasted%20image%2020241015140612.png)

#### 实际数据处理
而在实际处理中，往往是对每帧的接收数据做2DFFT,也就是对一个chirp信号的采样点维度和chirp数维度，分别能得到距离信息和速度信息。
![Pasted image 20241102152529.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E5%AE%9E%E9%AA%8C%E5%AE%A4%E9%A1%B9%E7%9B%AE/%E9%9B%B7%E8%BE%BE/%E7%90%86%E8%AE%BA/Pasted%20image%2020241102152529.png)

然后得到目标点里可以得到多普勒频率，之后可以算出速度。
![Pasted image 20241102152622.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E5%AE%9E%E9%AA%8C%E5%AE%A4%E9%A1%B9%E7%9B%AE/%E9%9B%B7%E8%BE%BE/%E7%90%86%E8%AE%BA/Pasted%20image%2020241102152622.png)
更方便的是用==速度分辨率==来计算。

### 最大测速范围
最大可以测量的相对速度是
![Pasted image 20241015140730.png](/img/user/Pasted%20image%2020241015140730.png)

## 调皮连续波内容
和距离测量也是同样的原理，只不过雷达在移动，或者说被测物体在移动（运动是相对的）。传输的距离就变成 2d+2vt了，而由于移动状态，返回信号会产生一个多普勒频移，入射角为Θ的信号，会产生cosΘ缩放的多普勒频移。如图![Pasted image 20240625095348.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240625095348.png)
根据![Pasted image 20240625095412.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240625095412.png)
通过算出多普勒频移可以得到物体运动的速度。




> [!NOTE] 调皮连续波的知乎文章中的问题
> ![Pasted image 20240624203029.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240624203029.png)
> 传播的距离为什么是![Pasted image 20240624203109.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240624203109.png)呢
> ![Pasted image 20240624203356.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240624203356.png)
> A:此处忽略了一半的位移，应该是雷达不动，物体动，

# 测角

角度的测量需要至少2个RX天线，利用物体到每个天线的不同距离导致2D-FFT峰值的相位变化来估计到达角，

![Pasted image 20241015141015.png](/img/user/Pasted%20image%2020241015141015.png)

相位的变化在数学上可以推导出下式：
![Pasted image 20241015141323.png](/img/user/Pasted%20image%2020241015141323.png)​

  而根据图12中的几何关系，可以得到：
![Pasted image 20241015141311.png](/img/user/Pasted%20image%2020241015141311.png)

  那么，
![Pasted image 20241015141302.png](/img/user/Pasted%20image%2020241015141302.png)

  同时，角度的准确测量也离不开∣ Δ ω ∣ < π，即
![Pasted image 20241015141246.png](/img/user/Pasted%20image%2020241015141246.png)
### 最大角度测量范围
即，两个间隔为d的天线可提供的最大视角为
![Pasted image 20241015141239.png](/img/user/Pasted%20image%2020241015141239.png)
当两个天线之间的间隔d=λ/2，会导致±90°的最大角视场。

==用测距的原理来测角，通过相位的改变，来测远端天线的距离，通过与天线间距离d的比值来得到角度==

#### 实际处理
实际处理中，通过取最大值来得到目标点之后，对比目标在同一帧里两个天线的接收信号的相位，计算出相位差，可以测得角度。

==注意==，要控制相位差在[-pi,pi]之间。需要加一个判断相位差的语句。

### 获取啁啾信号
通过二次瞬时相位复信号来分解为啁啾信号
![Pasted image 20240625164930.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240625164930.png)

一般表达式为![Pasted image 20240626151921.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240626151921.png)

# FMCW测量单目标距离
## 几何法
几何法与之前的测距原理相同，都是找发出信号和返回信号的时间差来算频率差，然后得到距离。

## 直观法
<font color=yellow>没看懂啥意思</font>
![Pasted image 20240626152358.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240626152358.png)
 
## DSP
没看出来和之前有啥区别。



> [!NOTE] 问题3
> 测量速度的时候，由于返回信号的幅度很小，那就很容易被淹没在噪声里，如何能够确定返回信号是哪个？
> A：一个方法![Pasted image 20240626152125.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240626152125.png)
> 0 ┏0 fb三角形与0 B Tc三角形相似，所以![Pasted image 20240626152250.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240626152250.png)
> Tc 、B 、 已知，只用知道┏0就可以知道fb
> 

matlab参考代码如下[[日常/BLOG/学习log/雷达/雷达测距测速测角--maltab\|雷达测距测速测角--maltab]]


参考文献：
[1] https://blog.csdn.net/Dandan2530/article/details/125282773?ops_request_misc=%257B%2522request%255Fid%2522%253A%252247749982-D2DD-4529-862B-45276932986F%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=47749982-D2DD-4529-862B-45276932986F&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-125282773-null-null.142^v100^pc_search_result_base6&utm_term=fmcw%E9%9B%B7%E8%BE%BE%E6%B5%8B%E8%B7%9D%E5%8E%9F%E7%90%86&spm=1018.2226.3001.4187

[2] https://blog.csdn.net/yaozekun/article/details/132889415?spm=1001.2014.3001.5502

[3]  [FMCW雷达距离多普勒(RDM)处理方法中距离分辨率和速度分辨率的推导_dopplerfft算法-CSDN博客](https://blog.csdn.net/qq_41248471/article/details/104276739)
[4]  [参考网站](https://blog.csdn.net/CUGzhazhadong/article/details/119541284)
