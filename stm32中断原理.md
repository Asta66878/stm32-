1.搭载STM32的开发环境
1.安装jdk
2.安装STM32CubeMX
安装结果如下
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e7eb50ce252646a4b099c5e9d7df037b.png#pic_center)

1.配置STM32CubeMX：安装HAL库

（1）选择自己想要安装的HAL库版本号

（2）下载HAL库，HAL库环境安装完成

安装结果如下
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4ef0f197350b4862b2721ba54d9d62ca.png#pic_center)

3.安装MDK5
2.利用HAL库新建keil5工程
1.STM32CubeMX建项目
1.选择的单片机型号以及点击开始工程项目

2.配置时钟，进入上面的RCC，有两个时钟，一个是HSE和LSE，将HSE那里设为Crystal/Ceramic Resonator

3.进入GPIO选择引脚 并且配置其工作模式：

选择了三个GPIO：PA0,PA1,PA2设置为了GPIO_output；以及一个PB5设置为外部中断源：

4.在GPIO界面里面点击PB5，对PB5的进行中断配置：

5.在CLK Configuration中，进行时钟配置

6.进入Project Manager，进行工程设置点击生成工程与代码：

2.keil工程编写
1.点击刚刚生成的keil5工程文件，双击main.c文件，在main函数上方进行编写一个中断函数HAL_GPIO_EXTI_Callback（）以及自定义一个中断的标识符号flag

输入以下代码

uint32_t flag=0;
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin){
  if(GPIO_Pin == SWITCH_Pin){
     GPIO_PinState pinState = HAL_GPIO_ReadPin(SWITCH_GPIO_Port,SWITCH_Pin);
      if(pinState==GPIO_PIN_SET)
      {
          flag=1;
      }

      else if(pinState==GPIO_PIN_RESET)
      {
          flag=0;
      }
      } 
   }
2.在main函数里的while(1)里面写上如下代码：

if(flag==1)
      {
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_0,GPIO_PIN_RESET);
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_1,GPIO_PIN_SET);
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_2,GPIO_PIN_SET);
          HAL_Delay(1000);
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_0,GPIO_PIN_SET);
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_1,GPIO_PIN_RESET);
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_2,GPIO_PIN_SET);
          HAL_Delay(1000);    
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_0,GPIO_PIN_SET);
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_1,GPIO_PIN_SET);
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_2,GPIO_PIN_RESET);
          HAL_Delay(1000);
      }
      else if(flag==0)
      {
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_0,GPIO_PIN_SET);
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_1,GPIO_PIN_SET);
          HAL_GPIO_WritePin(GPIOA,GPIO_PIN_2,GPIO_PIN_SET);
      }
3.程序烧录
实验结果如下
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/61296059283548f6ada44577abbb656b.png#pic_center)

（三）proteus仿真
1.搭建固定项目
1.新建工程

结果如下
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7cffb0ea2e404851ba7bffc161d66ba6.png#pic_center)

2.绘制系统电路
1.选择发光二极管，选择红、黄、绿三种颜色的led

2.分别拖入原理框内，并与对应的端口相连接

3.分别给3个LED配置电源

4.将开关拖入原理图内，与设置的外部中断端口相连，并配置电源

结果如下
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/587a67abd3e54f32ab0a7260946e7de6.png#pic_center)

3.仿真运行
选对晶振频率，接入电网，开始仿真

结果如下
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/76fbe3411c9c4777b4ed134386ee7904.png#pic_center)

