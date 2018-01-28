### Mutual Information什么意思？



### pushbroom image 什么意思？

第II部分A小节，This is the case with pushbroom images.

broom本身是“扫把”的意思，pushbroom其实就是一种成像方式，也就是线阵扫描，一维的成像在运动后形成二维的扫描面，就像扫把扫地一样，在大面积成像的时候用到，例如测绘，航天。

这种成像方式不是perspective projection，（一个维度是正交投影，另外一个维度是perspective projection），当然也不能用一般的rectify方法。



### hyperbolas epipolar lines

hyperbolas是双曲线的意思，文章引用了一个很有意思的结论，pushbroom相机在使用时，一维线性运动会把双目的极线变成双曲线。



### parallel projection

平行投影，也就是正交投影。



### Linear and Non-linear movement

什么是线性的运动和非线性的运动？

比如扫描仪加速扫描，但是扫描频率不变？这就会导致扫描速度慢的地方会被拉长，是不是指这个意思？

这种问题在pushbroom相机做测绘的时候还是比较常见的。

文章在这里又引用了另一个结论：非线性运动成像，极线会不规则，无法rectify。



### symbolize

这个词不太好翻译，将某某符号化，比较意译可以翻译为用数学表达式描述了某某关系。



### assumption of constant disparities

constant disparities in the vicinity of $\boldsymbol{P}$ ，这里的constant到底是什么意思？是指整个window所有pixel都是相同的值，还是指P的Window和Q的Window对应像素值相等？

是下面这一种呢？

12		12		12

12		P(12)	12

12		12		12

还是这一种？

3		7		20			3		7		20	

31		P(5)		4			31		Q(5)		4

4		21		17			4		21		17

应该是第二种吧，第一种依赖太强了，邻域里的值都相等，那还用什么邻域，直接用目标像素不就行了。

the implicit assumption of  constant disparity inside the area is violated at discontinuities.

在不连续处，这种constant是不成立的，这应该是指，在边缘区域，例如，物体的边缘距离为1m，而背景距离为2m，那么图像中，左右两图在边缘处的“样子”可能是不同的，例如左图有右图看不到的地方，或者反过来。



### entropy

信息论里面的“熵”。

joint entropy，耦合熵？



### insensitive to recording

which is insensitive to recording and illumination changes。

到底是对recording不敏感，还是对recording changes 不敏感？

recording不是记录的意思吗？什么叫对记录不敏感？



### well registered images 什么意思？

从文章给出的上下文看，应该是well matched image的同义词？



图像的亮度概率分布？

probability distributions P of intensities of associated images.

亮度分布和概率有什么关系？



### warp

one image needs to be warped according to the disparity image D

