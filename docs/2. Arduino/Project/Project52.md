### 项目五十二 散热装置

**1.实验说明**

生活中，电脑或者电路板芯片等等经常会由于工作时间或者功耗问题而发热严重，所以常常需要一个散热装置。

在前面学会使用温度传感器和电机模块，所以这节实验可以把它们结合起来做成一个智能散热装置。当检测到环境温度高于某一个值时的时候，电机开启，从而降低环境温度，达到散热效果。

**2.实验器材**

- keyes brick 热敏电阻传感器*1

- keyes UNO R3开发板*1

- keyes brick 电机-水泵驱动模块*1

- 传感器扩展板*1

- 3P双头XH2.54连接线*1

- 4P 双头XH2.54连接线*1

- USB线*1

- 130电机+100MM连接线*1

- 电机桨*1


**3.接线图**

![](media/image-20251022173032688.png)

**4.测试代码**

```
volatile int item = 0;

void setup() 
{
  Serial.begin(9600);
  pinMode(A2, OUTPUT);
  pinMode(A3, OUTPUT);
}

void loop() 
{
  item = analogRead(A4);
  Serial.println(item);
  if (item > 600) 
  {
    digitalWrite(A2, HIGH);
    digitalWrite(A3, LOW);
  } 
  else 
  {
    digitalWrite(A2, LOW);
    digitalWrite(A3, LOW);
  }
  delay(100);
}
```

**5.代码说明**

这里代码设置跟上个实验相似，原理可参照上个实验，热敏电阻的使用与电机的驱动请参照前面实验。

**6.测试结果**

烧录好测试代码，按照接线图连接好线，利用USB线上电后，打开软件串口监视器，设置波特率为9600，可以看到对应温度的模拟值，温度度越高，模拟值越大，如下图。当这个值超过我们设定的值时，电机转动。

![](media/image-20251022173132373.png)