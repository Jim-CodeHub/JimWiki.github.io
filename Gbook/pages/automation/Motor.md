
# 1 Concept 
电机（Electric Machine）是将电能与其它形式能进行*相互转换*的装置，
大部分电机应用**电磁感应**原理，机械能转换为电能的电机称为“发电机”，电能转换为机械能的电机称为“电动机”
新型超声波电机应用压电效应进行能量转换


主要有三种分类：

- 能量转化方向； 电动机、发电机
- 磁场方向；径向磁场电机、轴向磁场电机
- 电流形式：交流电机、直流电机

电机就是上述三种分类的交集

# 有刷直流电机

1.简介
有刷直流（BDC）电机是比较传统的电动机，其定子上是永磁体和电刷，转子上是线圈绕组和换向器，电能通过换向器、电刷进入电机（的转子），换向器与电刷接触以实现电流换向进而实现电机连续旋转。
有刷电机成本低，但因其电刷与换向器的接触式设计导致其可靠性差、寿命短、换向火花易产生电磁干扰等诸多缺点。
2.驱动
BDC电机一般不需要复杂的驱动电路，接通电源就可以旋转、关闭电源就可以（惯性）停止，但如果要实现双向旋转、速度控制、反馈电路等就需要设计驱动电路。
(1)BDC的启动
电机通电后，因电阻和电感较小且开始阶段反电动势很小，导致电流相对较大，最大可达到额定电流的15~20倍，这一电流会使电网受到扰动、机组受到机械冲击、换向器发生火花，因此直接合闸启动只适用于功率不大于4千瓦的电动机（启动电流为额定电流的6-8倍）。
(2)双向转动
BDC电机的双向转动硬件上需要H桥电路，软件上只需变换通电顺序，下图为简易的H桥电路模拟。也可使用MC33035芯片实现。
 
(3)PWM调速
BDC电机的速度由电压大小而定，一般有两种方式进行调速：①改变励磁电压，降压升速、升压降速。②改变电枢电压，升压升速、降压降速（常用）。
PWM是用数字方式控制模拟输出的电压调节手段，其调速就是改变上述某种方式的电压来进行调速。注：PWM调节BDC电机常用于智能小车的调速。

# 无刷直流电机
1.简介
无刷直流电机（BLCD）使用半导体开关器件来实现电子换向，成本相对较高，但有使用寿命长、易于控速等诸多优点。按电子换向器的实现方式可分为有位置/有霍尔传感器（参看附录A）无刷电机和无位置/无霍尔传感器无刷电机。
2.组成
无刷电机系统由电机本体和驱动器组成，电机本体由多极绕组定子、永磁体转子组成，通过改变定子绕组线圈的通电顺序即可驱动转子转动，通过控制定子绕组线圈的通电大小即可控制转子力矩，通过控制定子绕组线圈的通电频率即可控制转子的运转速度。
![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/无刷电机1.png)
![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/无刷电机2.png)

外转子无刷电机驱动系统
逆变器将直流电转变为三相交流电，PWM控制信号决定其6个功率晶体管的开关及频率从而控制流经定子线圈的电流变化。
3.驱动
无刷电机驱动电路可选专业驱动器或驱动芯片，如摩托罗拉生产的MC3303X系列芯片，控制部分只需给出使能、方向和刹车信号，其它逻辑都由芯片解决，这种方式控制的缺点是不能控制电机转速。
![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/无刷电机驱动.png)

给定EN=1、DIR0/1、Brake=0/1，电机就转动/停止。
4.接线
无刷电机一共有八根引线，其中三根是信号线，五根是霍尔传感器接线，信号线接电机U/V/W，五根霍尔接线中有一个VCC一个GND，余下三个为SA/SB/SC。

# 步进电机
1.简介
步进电机是一种将电脉冲转化为角位移或线位移的执行机构，是现代数字程序控制系统中的主要执行元件，广泛应用于开环控制领域。
2.分类
步进电机分为反应式（VR）、永磁式（PM）和混合式（HS）。按定子绕组可分为二相（1.8°）、三相（1.2°）和五相（0.72°）等。反应式和永磁式步各有缺点，混合式集成了它们的优势，具有步距角小、转矩大等特点。目前最受市场欢迎的是二相混合式步进电机，约占市场份额的97%。
3.特点
步进电机的角位移与脉冲数成正比、速度和加速度与脉冲频率成正比、 步距角精度在3%~5%，且没有累积误差。
步进电机的频矩特性
由于反向电动势作用，步进电机的力矩与脉冲频率成反比。
多数步进电机在1KHz时转矩迅速衰减，在0.1KHz时达最高力矩，在5KHz~10KHz左右衰减为0（0~100Hz↑ 100~1000Hz↓）。

![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/步进频矩特性曲线.png)
从曲线可看出，电机启动时不应高于最高启动频率，否则可能丢步、堵转；给定一个适当的启动频率而不是从零开始，以使步进较快进入到运行速度。
步进细分与转矩关系
在频率一定时，增大细分数将使转矩增大，在频矩特性曲线上表现为横向拉伸。
测试YOFO-42系列两相混合步进，频率为约10000Hz时，设置细分数为200、400时电机堵转，力矩很小，可以用手反向扭转。设置细分数为800、3200时力矩大，用手不能反转方向。
温度与转矩的关系
高温（＞90°）将使转子退磁，进而导致电机力矩下降或丢步；高温可能来自环境温度或由电机高功率、大负载的持续输出引起。
电压电流与频矩特性
改变电压和电流都会影响电机的频矩特性，电压和电流增大会增加静力矩（正比）并同时增加转速（正比）。相对来说，在恒压下改变电流对电机力矩影响更突出、在恒流下改变电压对电机速度影响更突出。
在增压时可适当减少电流，以减少高功率输出导致高温引起的力矩下降，反之亦然

步进电机没有最低启动频率，可以今年给一个低电平，明年给另一个高电平，电机仍然能走一步，但有最高启动频率，超过最高启动频率步进电机将堵转（或出现不稳定现象），最高启动频率跟步进细分、电机大小、负载大小等有关。

细分越大，最高启动频率越大（这个没什么好说的，就是频矩特性）。电机越大转子越重，最高启动频率越低，比如同样是100Hz，在小电机上不用加速就能正常运行，换了一个大电机反而转不动了！。负载也是一样，负载变大，就需要加速曲线来启动。

4.组成
步进电机系统由电机本体和驱动器组成，电机本体由多相绕组定子、永磁体转子组成，定子绕组表面和转子表面都刻有相同尺寸的小齿，当转子小齿与一组定子绕组重合时，其它转子小齿刚好与其它组定子绕组错位，通过定子绕组换相即可驱动转子转动。

![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/步进组成.png)
两相步进电机一般有八个定子绕组，间隔选取四个绕组成一相。
5.驱动
步进电机驱动电路可选专业驱动器（如：FYDH807T）或驱动芯片（如：Allegro A4982SLPT、达林顿驱动器ULN2003）。步进驱动器至少应具备功率放大能力，其次应具备脉冲分配和细分功能。
仅有电流放大能力的步进驱动器增加了编程的复杂性，如使用ULN2003驱动时，必须设定脉冲节拍以驱动步进电机转动方向和精度。而对于具备脉冲分配功能的步进驱动器，只需给定脉冲和方向即可。
![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/步进驱动.png)

一个步进电机驱动器/驱动芯片理论上可以驱动多个步进电机，但因电气和时序等问题影响会减少驱动器使用寿命、降低步进电机精度。
步进驱动器的细分功能是一种通过精确控制步进电机的相电流而提高步距角精度的电子阻尼技术，细分技术提高了步进电机分辨率、减弱或消除了步进电机固有的低频振荡、增大了步进电机（30-40%的）输出转矩。
6.接线
步进电机的外部引线数与相数没有必然的联系，许多二相步进电机有8根引线，这种电机既可以串联连接又可以并联连接。不同的接线方式并不影响驱动程序的编写。

![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/步进切线.png)
串联连接的电机，电流较小，低频力矩较大（适用低速）；并联连接的电机，电感较小，所以启动、停止速度较快，高频力矩有所增大（适用高速）。
步进电机与驱动器接线方式如下图：MP表示脉冲接线、DIR表示方向接线、MF表示使能接线（置低电平时脱机）。

![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/步进剑仙2.png)
其中VBB为+24V， PG为+24地。
7.加减速控制
加减速控制的目的是解决步进电机堵转（或失步）和过冲的现象。导致步进电机堵转（或失步）现象的原因：①启动频率：启动频率高于最高启动频率限制；②跳变频率：频率突变导致转矩瞬时下降。导致步进电机过冲的原因是步进电机在高频运行时瞬时停止。
常见的加减速方案有：梯形曲线、指数曲线、S曲线等；其中S型加减速曲线最为平滑，生成S型加减速曲线也可通过多种基本曲线，如正弦曲线、Sigmoid曲线等，其中Sigmoid曲线是标准S曲线函数，它在生物学和神经网络中有广泛应用。
(1)Sigmoid公式
公式：f(x) = 1/(1+e-x)；
特点：x在(-10, 10)之间取值，f(x)在(0, 1)之间变化，曲线以(0，0.5)对称。

![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/步进sigmoid.png)

可以看出在[-5, 5]区间以外，其函数值变化非常小，所以可按实际情况裁剪使用。

(2)Sigmoid变换
![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/步进sigmoid2.png)
坐标变换规则：①平移，左加右减，上加下减。②伸缩，X：压缩乘、拉伸除；Y：拉伸乘、拉伸除。注：伸缩表示伸缩为原值的多少倍。
Sigmoid函数是函数模型，需要在其基础上进行变换才能成为实际应用函数。使用Sigmoid曲线驱动步进电机，其Y轴变换为频率f，X轴变换为采样点，通过坐标变换可得
Y = (Fset-Fmin)/(1+exp(-Flexible*(x-num)/num)) + Fmin（加速曲线）：

设FSet=100，Fmin=50，x∈[0,100]
注：①其中Fset为设置的运行频率、Fmin为设置的起始频率、flexible为压缩率（4~6）、num是x取值范围的一半；②加减速时间与取样·动频率成正比；③减速曲线公式为Y = Fset - (Fset-Fmin)/(1+exp(-Flexible*(x-num)/num))。

(3)Sigmoid使用
取适当数量样本，使用公式得到频率数组并转换为半周期数组或定时器计数器重载值数组，然后通过软件延时或定时器定时方式得到50%PWM方波来驱动步进电机。
设得到半周期数组int Tarr[] = {10, 11, 13, 18, 20, 30, 38, 45, 50, 53}、定时器计数器重载值数组int Carr[] = {}。
举例 - 软件延时方式：
for (int i = 0; i < sizeof(Tarr)/sizeof(int); i++)
{
	PORT = 1; 
	delay(Tarr[i]); 
	PORT = 0;
	delay(Tarr[i]); 
}
//加速完成退出循环，进入匀速运行阶段。减速时倒取数组即可。

int Countflag = 0; 

int i = 0; 
TCNT0 = Carr[0];
ISR(TIMER0) 
{ 
	if (i < sizeof(Carr)/sizeof(int)) 
	{ 
		PORT ^= 0X01; 
		if (Countflag++ == 2) 
		{ 
			Countflag = 0; 
			TCNT0 = Carr[i++]; 
		} 
	} 
}
//加速完成脉冲结束，进入匀速运行阶段。减速时倒取数组即可。


举例 - 定时器定时方式：
附：定时器计数器重载值计算方法
通过定时器时钟Tclk，则计数一次的时间为1000/Tclk毫秒，计算Thalf/(1000/Tclk)得到计数次数（当次数小于255时采用8bit定时器，否则采用16bit定时器），于是得定时器计数器重载值TCNT = 255 - Thalf/(1000/Tclk)。


PS:步进电机不能突然换向，（当然也跟运行频率相关），突然换向可能导致失步，这是因为电机内部或驱动器的原因，所以换向之后要延时一点再给脉冲。

# 伺服电机

伺服电机 尾部具有（高精度）码盘，并在一侧接有两个接口，接入驱动器， 一个输入，一个输出，

输入：系统 -》 驱动器 -》伺服电机

系统 《- 驱动器 《- 伺服电机 : 输出

伺服电机通过码盘将运行数据实时传送给驱动器，由驱动器返回系统，系统进行反馈处理 并输出校正


步进电机没有配备码盘，输出默认是没有反馈的，输出精度与电机质量、负载、环境、电流电压等都有关系，
要使用步进电机组成伺服系统，则必须接入反馈装置

伺服电机 也不能完全取代步进电机， 伺服电机的反馈源是在伺服电机的本身上，所以不能适用所有场景，适用于直接输出而不考虑负载的复杂情况时很适用

相比这点，步进电机就比较灵活，因为可以将反馈系统放置到任何位置，比如步进电机驱动胶辊、胶辊下走纸张，我想要纸张定位的精度，而不是胶辊旋转的精度，
这样伺服电机就不适用了，因为胶辊带动纸张是会漂移的，所以不能对胶辊直接定位，
这时将反馈系统（如编码器）放到纸张运行的适当位置，就可以达到控制效果

https://www.youtube.com/watch?v=Gzo9m0tMD0A


//TBD 伺服系统、伺服电机http://www.xfoyo.com/jishuhefuwu/jiejuefangan/267.html
http://www.xfoyo.com/jishuhefuwu/jiejuefangan/320.html

# 减速电机

减速电机（齿轮电机）是减速机和电机的结合设备，目的是提高输出扭矩、降低负载惯量，常用于低转速、大扭矩的传动设备。

通过减速机中输入轴配有较少的齿轮来啮合输出轴上的大齿轮，大小齿轮的齿数之比就是传动比，减速机在原动机和执行机构之间起匹配转速和传递扭矩的作用

# 电机控制系统
//TBD
材料1：电动机的转速与力矩成反比，因此通常配合减速器（并集成）使用，如：减速步进电机、减速无刷电机等。
材料2：编码器、反馈、闭环、开环
材料3：伺服电机、伺服系统
材料4：步进、伺服:https://www.zhihu.com/question/37374664


附录A：霍尔效应与霍尔传感器

1.霍尔效应
洛伦兹力指出：在磁场中的通电导体其内部的载流子会因磁场而产生偏移；在洛伦兹力的作用下会使通电导体产生感应电动势，这就是霍尔效应。
2.霍尔传感器：
根据霍尔效应做成霍尔器件，就是以磁场为工作媒介将物体的运动参量转变为数字电压的输出，使之具备传感和开关的功能，这就是霍尔传感器、霍尔开关器件。
无刷电机的霍尔传感器通常以120°或60°的角度排列在定子绕组上，为控制部分提供相序控制的依据，同时也可以作为无刷电机的闭环反馈系统。

![1](https://github.com/Jim-CodeHub/Skills-list/raw/master/image/PCB/霍尔效应.png)

附录B：步进电机应用术语

步距角	对应一个脉冲信号，电机转子转过的角位移，一般二相步进电机步距角为1.8°、三相1.5°、五相0.72°
步距角精度	步进电机每转过一个步距角的实际值与理论值的误差。用百分比表示： （误差/步距角）× 100%
相数	步进电机内部线圈组数
转矩（扭矩）	使物体发生转动的特殊力矩
保持转矩（静力矩）	步进电机通以额定电流但没有转动时，定子锁住转子的力矩。它是步进电机最重要的参数之一，通常步进电机在低速时的转矩接近静力矩
定位转矩（定力矩）	步进电机没有通电的情况下，定子锁住转子的力矩
最大空载起动频率	电机在某种驱动形式、电压及额定电流下，在不加负载的情况下，能够直接起动的最大频率
最大空载运行频率	电机在某种驱动形式、电压及额定电流下，电机不带负载的最高运行频率
丢步（失步）	步进电机转动步距角个数小于给定脉冲数。一般当电机力矩偏小、加速度偏大、速度偏高、摩擦力不均匀等都会使丢步现象发生
拍数	完成一个磁场周期性变化所需脉冲数或导电状态，或指电机转过一个齿距角所需脉冲数。步距角=360/（拍数*齿数），二相电机步距角1.8°，齿数为50，则拍数为4，即工作节拍为二相四拍
径向间隙	径向负载时轴偏离距离（伸出食指与地面平行，向手指挂载重量会使手指偏移，这就是径向负载, 而负载时的偏移与原来位置间的距离就是颈项间隙）
轴向间隙	轴向负载时轴移动距离（伸出食指向下与地面垂直，向手指挂载重量会使手指关节脱离一定距离，这就是轴向负载，而负载时的关节偏移与关节原位置的距离就是轴向间隙）
法兰面	与轴垂直的电机前表面
径向负载	距离法兰面一定距离，径向所能承受的最大力
轴向负载	轴向所能承受最大力


附录C：步进电机计算公式

关系	公式	单位
频率与转速	f =（n * S）/ 60	Hz或pps
线速度与转速	v = π* D * n	m/min
步距角	θ = 360 / S 或 θ = 360 /（Z *M）	°
推导公式
转速与频率	n = f * 60 / S	r/min或rpm
转速与线速度	n = v /（π* D）	r/min或rpm
线速度与频率	v = π* D * f * 60 / S	m/min
频率与线速度	f = v * S /（π* D * 60）	Hz或pps
说明：n为角速度，单位：r/min或rpm。S为步进一圈步数。D为步进同轴齿轮直径，单位：m 。Z为转子齿数，M为运行拍数。f为频率，单位Hz，即1秒所需脉冲数量。
注意：1.从公式可以看出，所需线速度固定时角速度不变，如果增加步进细分值则必须同时增大驱动频率。2.步进电机没有最低启动频率，脉冲之间时间间隔可以无限长。

实际使用电机，还是需要看转速，因为脉冲频率跟细分相关 所以不能直接表达运行速度（细分情况跟精度有关）


附录D：电机选型
TBD

---------
APPEND

同步异步指的是转子转速与定子旋转磁场转速是同步（相同）还是异步（滞后），因而只有交流能产生旋转磁场，*只有交流电机有同步异步的概念*

同步电机——原理：靠“磁场总是沿着磁路最短的方向上走”实现转子磁极与定子旋转磁场磁极逐一对应，转子磁极转速与旋转磁场转速相同。 特点：同步电机无论作为电动机还是发电机使用，其转速与交流电频率之间将严格不变。同步电机转速恒定，不受负载变化影响

异步电机——原理：靠感应来实现运动，定子旋转磁场切割鼠笼，使鼠笼产生感应电流，感应电流受力使转子旋转。转子转速与定子旋转磁场转速必须有转速差才能形成磁场切割鼠笼，产生感应电流。

区别：（1）同步电机可以发出无功功率，也可以吸收；异步电机只能吸收无功。（2）同步电机的转速与交流工频50Hz电源同步，即2极电机3000转、4极1500、6极1000等。异步电机的转速则稍微滞后，即2极2880、4极1440、6极960等。（3）同步电动机的电流在相位上是超前于电压的，即同步电动机是一个容性负载。同步电动机可以用以改进供电系统的功率因素。

同步电机无法直接启动：刚通电一瞬间，通入直流电的转子励磁绕组是静止的，转子磁极静止；定子磁场立即具有高速。假设此瞬间正好定子磁极与转子磁极一一对应吸引，在定子磁极在极短的时间内旋转半周的时间之内，会对转子产生吸引力，半周之后将会产生排斥力。由于转子有转动惯量，转子不会转动起来，而是在接近于0的速度下左右震动。因此同步电机需要鼠笼绕组启动。转速差使其产生感应电流，而感应电流具有减小转速差的特性（四根金属棒搭成井形，内部磁场变密会减小面积，变疏会增加面积，阻止其变化趋势），因而会使转子转动起来，直到感应电流与转速差平衡（没有电流就不会有力，因而不会消除转速差，猜测与旋转阻力有关）



有刷无刷电机都是针对直流电机而言的，为了直流电能产生使转子持续旋转的磁场才 需要刷结构

步进电机是有刷还是无刷？

首先步进电机是直流电机，但既不是有刷也不是无刷，步进的驱动不是靠电刷或霍尔的换向而产生的磁场来旋转的，步进电机的驱动是靠脉冲信号产生的磁场来旋转的


伺服电机与步进电机的区别？

控制器 -》 驱动器 -》 步进电机

控制器 -》 驱动器 -》 伺服电机
			 |			|
			 --<--------
				反馈

控制器 -》 驱动器 -》 伺服电机
  |						 |
  --<---------------------
				反馈

最大的区别是在**反馈**上，步进电机是开环的，理论上发1个脉冲走一步，但发N个脉冲丢了多少步也不知道
伺服电机自带反馈系统，能让控制器和驱动器知道所走的精度

步进电机在精度要求较高的场合会使用编码器来反馈

控制器 -》 驱动器 -》 步进电机 -> 编码器
  |									| 
  --<--------------------------------

实际上伺服电机的反馈原理与这种方式类似

另外，伺服电机的负载、转矩、启动/响应速度等性能都高于步进电机，
这是特殊内部结构和工艺等原因造成的，因此伺服电机也比较贵

伺服系统 servo system
就是指类似伺服电机这种反馈系统，
可以说凡是具有严格反馈的系统都可以成为伺服系统


-------

电机选型


----------

交流电机 主要分为 同步电机和异步电机

同步和异步是在交流电机上的分类 而不是直流电机上的分类

1. 构造差异 constructional difference

	- 同步电机: 定子具有轴向槽，该槽由定子绕组组成，用于特定数量的极
	- 异步电机：

--------------

卷取作业 与 力矩电机


力矩电机的负载增大时，需求转矩增大，进而速度减小，这种特性非常适合卷曲作业.


卷取设备通常需求固定张力，固定速度。但进行卷取作业时，卷取侧的直径会逐步变大，受其影响每转一圈卷取的量也会逐步变多，并且需求更大的转矩。因此，为了保持固定张力和固定速度，电机有必要要在直径变大时逐步减速并增大转矩.


