### 项目六 无源蜂鸣器模块播放音乐

**1.实验说明**

前面课程中介绍了套件中的有源蜂鸣器模块的使用方法。在这里介绍套件中的无源蜂鸣器模块，它主要采用12\*8.5MM 5V 2K无源蜂鸣器元件。无源蜂鸣器元件内部不带震荡电路，控制时，在蜂鸣器元件正极输入不同频率的方波（电压5V），负极接地，控制蜂鸣器响起不同频率的声音。该元件的中心频率是2KHz。无源蜂鸣器驱动频率与发生频率之间是一一对应的关系，即驱动频率是2KHz的方波，那听到的声音频率也是2KHz。

实验中，利用无源蜂鸣器模块上蜂鸣器输出各种频率的声音，然后控制无源蜂鸣器模块上蜂鸣器播放完整音乐。

**2.实验器材**

- keyes brick 无源蜂鸣器模块*1

- keyes UNO R3开发板*1

- 传感器扩展板*1

- 3P 双头XH2.54连接线*1

- USB线*1


**3.接线图**

![](media/image-20251022122239566.png)

**4.测试代码**

代码1：

```
int beeppin = 3; //定义蜂鸣器引脚为D3

void setup() 
{
  pinMode(beeppin, OUTPUT);//定义蜂鸣器管脚为输出模式
}

void loop() 
{
  tone(beeppin, 262);//DO播放1000ms
  delay(1000);
  tone(beeppin, 294);//Re播放750ms
  delay(750);
  tone(beeppin, 330);//Mi播放625ms
  delay(625);
  tone(beeppin, 349, 500);//Fa播放500ms
  delay(500);
  tone(beeppin, 392, 375);//So播放375ms
  delay(375);
  tone(beeppin, 440, 250);//La播放250ms
  delay(250);
  tone(beeppin, 494, 125);//Si播放125ms
  delay(125);
  noTone(beeppin);//停止播放一秒
  delay(1000);
}
```

代码2：

```
#define D0 -1
#define D1 262
#define D2 293
#define D3 329
#define D4 349
#define D5 392
#define D6 440
#define D7 494
#define M1 523
#define M2 586
#define M3 658
#define M4 697
#define M5 783
#define M6 879
#define M7 987
#define H1 1045
#define H2 1171
#define H3 1316
#define H4 1393
#define H5 1563
#define H6 1755
#define H7 1971
//列出全部D调的频率
#define WHOLE 1
#define HALF 0.5
#define QUARTER 0.25
#define EIGHTH 0.25
#define SIXTEENTH 0.625
//列出所有节拍
int tune[] =       //根据简谱列出各频率
{
  D5, D5, D6, D5, M1, D7,
  D5, D5, D6, D5, M2, M1,
  D5, D5, M5, M3, M1, D7, D6,
  M4, M4, M3, M1, M2, M1
};
float durt[] =      //根据简谱列出各节拍
{
  0.5, 0.5, 1, 1, 1, 1 + 1,
  0.5, 0.5, 1, 1, 1, 1 + 1,
  0.5, 0.5, 1, 1, 1, 1, 1,
  0.5, 0.5, 1, 1, 1, 1 + 1
};
int beeppin = 3; //蜂鸣器的PIN
int length;

void setup() 
{
  pinMode(beeppin, OUTPUT);     //设置蜂鸣器引脚输出模式
  length = sizeof(tune) / sizeof(tune[0]); //计算长度
}

void loop() 
{
  for (int x = 0; x < length; x++)
  {
    tone(beeppin, tune[x]);
    delay(500 * durt[x]); //这里用来根据节拍调节延时，500这个指数可以自己调整，在该音乐中，我发现用500比较合适。
    noTone(beeppin);
  }
  delay(2000);
}
```

**5.代码1说明**

在本实验中，用到了函数tone（）。

```
tone(pin, frequency)；
```

pin为生成音调的arduino引脚,设置了3；frequency为音调频率，单位为Hz,数据类型为unsigned int（范围0 ~ 65,535 ((2^16) -1)）。tone函数在引脚上生成指定频率（和50％占空比）的方波。直到调用noTone（）（停止生成音调）为止。该引脚可以连接到压电蜂鸣器或其他扬声器以播放音调。tone（）一次只能产生一种音调。如果某个音色已经在其他引脚上播放，则对tone（）的调用将无效。使用tone（）函数将干扰引脚3和11（Mega以外的板上）上的PWM输出。同时tone（）不能产生低于31Hz的音调。如果要在多个引脚上演奏不同的音高，则需要在一个引脚上调用noTone（），然后在下一个引脚上调用tone（）。

**6.代码2说明**

列出了所有D调的频率，方便后面使用。然后根据简谱列出各频率，再列出各节拍，用到的一个节拍为500ms，这个可以调整，然后循环响起音调与对应节拍就成了一首歌曲。

**7.测试结果**

上传测试代码1成功，上电后，模块上无源蜂鸣器循环播放对应频率对应节拍的声音。上传测试代码2成功，上电后，模块上无源蜂鸣器循环播放《生日快乐》歌曲。