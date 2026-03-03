### 项目十七 光折断计数

**1.实验说明**

这个套件中包含一个 keyes brick光折断传感器，它主要采用1个ITR-9608光电开关。它属于对射遮断式光电开关光学开关传感器。当用纸片挡住传感器凹槽后，传感器信号端为高电平，自带D1 LED熄灭；否则传感器信号端为低电平，自带D1 LED亮起。

在这里，通过检测传感器信号端高低电平，通过代码设置，模拟出流水线上利用类似传感器，对产品进行计数。

**2.实验器材**

- keyes brick光折断传感器*1

- keyes UNO R3开发板*1

- 传感器扩展板*1

- 3P双头XH2.54连接线*1

- USB线*1


**3.接线图**

![](media/image-20251022142856593.png)

**4.测试代码**

```
int PushCounter = 0; //计数赋初值0
int State = 0; //当前的状态
int lastState = 0; //之前的状态

void setup() 
{
  Serial.begin(9600);//设置波特率为9600
  pinMode(3, INPUT);//设置模式为输入
}

void loop() 
{
  State = digitalRead(3);//读取当前状态
  if (State != lastState) //如果与之前的状态不相同
  {
    if (State == 1) //光线折断
    {
      PushCounter = PushCounter + 1;//计数加1
    }
  }
  lastState = State;//更新状态
  Serial.println(PushCounter);//打印计数
}
```

**5.代码说明**

通过以下表格，我们可以了解这个代码的逻辑设置。

| 初始设置                               | PushCounter设置为0（累计通过物体数目）<br>State设置为0（传感信号端数值）<br>lastState设置为0（传感器信号端上一循环数值） |                                 |
| -------------------------------------- | ------------------------------------------------------------ | ------------------------------- |
| 当物体开始穿过传感器凹槽时（一瞬间）   | State检测到变为1，lastState为0，两个数据不相等。             | PushCounter设置为PushCounter加1 |
| 当物体穿过传感器凹槽过程中（循环）     | State检测到变为1，lastState设置为1，两个数据相等。           | PushCounter不变                 |
| 当物体刚穿过传感器凹槽过程中（一瞬间） | State检测到变为0，lastState设置为1，两个数据不相等。         | PushCounter不变                 |
| 当物体完全穿过传感器凹槽后（循环）     | State检测到变为0，lastState设置为0，两个数据相等。           | PushCounter不变                 |

**6.测试结果**

上传测试代码成功，按照接线图接好线，利用USB上电后，打开串口监视器，设置波特率为9600；串口监视器显示PushCounter数据，每个物体穿过传感器凹槽，PushCounter数据不断加1。

![](media/image-20251022143233562.png)