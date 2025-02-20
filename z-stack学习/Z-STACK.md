# Z-STACK学习记录_无线收发控制LED

## Zstack文件结构

![image-20241225170411235](D:\Desktop\Study_Note\z-stack学习\img\image-20241225170411235.png)

## Zstack协议栈工程目录

![image-20241225170606045](D:\Desktop\Study_Note\z-stack学习\img\image-20241225170606045.png)

## 析协议栈工作流程

1. zigbee工作流程

![image-20241225170714334](D:\Desktop\Study_Note\z-stack学习\img\image-20241225170714334.png)

2. 程序入口

```c
int main( void )
{
  // Turn off interrupts
  osal_int_disable( INTS_ALL ); //关闭所有中断

  // Initialization for board related stuff such as LEDs
  HAL_BOARD_INIT();             //初始化系统时钟

  // Make sure supply voltage is high enough to run
  zmain_vdd_check();            //检查芯片电压是否正常

  // Initialize board I/O
  InitBoard( OB_COLD );         //初始化I/O ，LED 、Timer 等

  // Initialze HAL drivers
  HalDriverInit();              //初始化芯片各硬件模块

  // Initialize NV System
  osal_nv_init( NULL );         //初始化Flash 存储器

  // Initialize the MAC
  ZMacInit();                   //初始化MAC 层

  // Determine the extended address
  zmain_ext_addr();             //确定IEEE 64位地址

#if defined ZCL_KEY_ESTABLISH
  // Initialize the Certicom certificate information.
  zmain_cert_init();
#endif

  // Initialize basic NV items
  zgInit();                     //初始化非易失变量

#ifndef NONWK
  // Since the AF isn't a task, call it's initialization routine
  afInit();
#endif

  // Initialize the operating system
  osal_init_system();           //初始化操作系统

  // Allow interrupts
  osal_int_enable( INTS_ALL );  //使能全部中断

  // Final board initialization
  InitBoard( OB_READY );        //最终板载初始化

  // Display information about this device
  zmain_dev_info();             //显示设备信息

  /* Display the device info on the LCD */
#ifdef LCD_SUPPORTED
  zmain_lcd_init();             //初始化LCD
#endif

#ifdef WDT_IN_PM1
  /* If WDT is used, this is a good place to enable it. */
  WatchDogEnable( WDTIMX );
#endif

  osal_start_system(); // No Return from here 执行操作系统，进去后不会返回

  return 0;  // Shouldn't get here.
} // main()
```

main 函数先执行初始化工作，包括硬件、网络层、任务等的初始化。然后执行 osal_start_system();操作系统，进去后可不会回来了。在这里，我们重点了解 2 个函数：
初始化操作系统 **osal_init_system();**
运行操作系统 **osal_start_system();**

3.  系统初始化

   ​        先来看osal_init_system();系统初始化函数，进入函数。如果用IAR看代码可在函数名上单击右键-->go to definition of…，便可以进入函数。发现里面有 6 个初始化函数，这里我们只关心 osalInitTasks();任务初始化函数，继续由该函数进入。

![image-20241225171245696](D:\Desktop\Study_Note\z-stack学习\img\image-20241225171245696.png)

![image-20241225171317977](D:\Desktop\Study_Note\z-stack学习\img\image-20241225171317977.png)

函数对taskID进行初始化，每初始化一个taskID++。大家看到了注释后面有些写着用户需要考虑， 有些 则写着用户不需考虑。没错，需要考虑的用户可以根据自己的硬件平台或者其他设置，而写着不需 考虑的也是不能修改的。SampleApp_Init()是我们应用协议栈例程的必要函数，用户通常在这里初始化自 己的东西。至此，osal_init_system();大概了解完毕

4.  运行操作系统

​			接下来看第二个函数 osal_start_system( );运行操作系统。同样用go to definition 的方法进入 该函数。

![image-20241225171422524](D:\Desktop\Study_Note\z-stack学习\img\image-20241225171422524.png)

![image-20241225171457727](D:\Desktop\Study_Note\z-stack学习\img\image-20241225171457727.png)

我们看一下 events = tasksEvents[idx];进入 tasksEvents[idx]数组定义 ，发现恰好是 osalInitTasks()函数里面分配空间初始化过的tasksEvents。而且taskID一一对应。这就是初始化与调 用的关系。taskID把任务联系起来了

5.  用户应用任务初始化

   函数SampleApp_Init()是用户应用任务初始化函数。

![image-20241225171630305](D:\Desktop\Study_Note\z-stack学习\img\image-20241225171630305.png)

6.  应用任务事件处理函数

![image-20241225171738685](D:\Desktop\Study_Note\z-stack学习\img\image-20241225171738685.png)

![image-20241225171804399](D:\Desktop\Study_Note\z-stack学习\img\image-20241225171804399.png)

7. 应用层无线消息处理

   应用层的无线消息处理是在函数SampleApp_MessageMSGCB()里

![image-20241225171900764](D:\Desktop\Study_Note\z-stack学习\img\image-20241225171900764.png)

8. 周期发送数据

​		周期发送信息SampleApp_SendPeriodicMessage()函数周期发送了“D1”，内部调用了函数 AF_DataRequest 进行无线消息发送，请注意看注释

![image-20241225171959955](D:\Desktop\Study_Note\z-stack学习\img\image-20241225171959955.png)