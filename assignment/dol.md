## DOL实例分析&编程


----------
#### 一、example 1 代码分析
* generator.c
  定义进程：每个模块都要写上 定义进程：每个模块都要写上 xxx_fire （可能被执行无 数次），至于 init 是可选择写或者不的， 是可选择写或者不的， xxx_init（只 会被执行一次）。  

     * generator_init
     
![] (http://p1.bqimg.com/567571/d7649c951c198fa1.png)

  初始化函数,这里代码的意思将当前位置置为    0，设置生产者长度。这里的local指针指向的 是.h 文件的 _local_states结构，这里的意思是生产0~length-1个自然数，如果超过就会被销毁
     * generator_fire
     
![] (http://p1.bpimg.com/567571/7e648f9476f6b538.png)

信号产生函数。这里的代码是：如果当前位置小于生产长度，则将x（这里是当前下标）写入到输出端，否则销毁进程。

*  consumer.c
    定义消费者进程
    * consumer_init
    
      ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-10-27/44095924.jpg)
      
   和generator的初始化函数一样，定义初始位置和总长度
   * consumer_fire
   
     ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-10-27/53280176.jpg)
     
 信号消费函数，若当前位置小 于设定长度，则   读出输入端信号并且打印；否则销毁进程（停下 来）。

* square.c
定义平方进程
   *  square_fire
   
        ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-10-27/17584576.jpg)
        
    信号处理函数，读入输端i，将其平方后写出到输端，也是重复length次之后就停止了
* example.xml
   * 进程定义，processes
   
           ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/46345975.jpg)
           
•process name==实现的模块的名字，比如写了xxx.c这里就是xxx了
•port type==output或者input
•name==端口的名字，在*.h的文件里面
 * 通道定义，channel
 
      ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/82409939.jpg)
      
类型是fifo先进先出，最多放10个信号量
每个channel有一个输入端和一个输出端
 * 定义各个模块之间的连接,connection
 
   ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/12912829.jpg)
   
   一条线会对应两个connection，就是A框的右手牵着这条线的左边，这条线的右边牵着B框。每条线要有2个connection
   举个例子，上面第一条条connection叫“g-c”；它从”gennerator”这个模块的”1”端口连接到“c1”这个模块（线）的”0”端口。此时还需要对应另外的一条connection，它要从“c1”这个模块（线）的其他端口，连接到其他的模块上。
   
   


----------
####  二、编程（修改example2）
* 修改 example2，让 3个square模块变成 2个
1. 进入example2文件夹获得example2.xml的修改权限
    ` ~$ cd dol/examples/example2           //进入到需要修改的文件example2.xml所在文件夹中
  ~$ sudo gedit example2.xml            //赋予编辑该文件的权限，开始修改代码 `
2. 将迭代次数由3次变为两次
  因为题目的要求是将square模块从3个变为2个，所以只需要将下图中的N变为2
  
   ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/81920767.jpg)
   
   也即修改定义的N
   
   ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/24098103.jpg)
   
   将上图的3改为2
   3. 运行修改后的example2文件
        * 编译前先删除之前编译文件产生的文件，否则运行的时候将没有改变
        
          ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/19119636.jpg)
   
        * 重新编译修改过的文件
        
          ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/69885960.jpg)
          
        * 运行编译之后的文件
        
          ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/16412065.jpg)
          
   4. 运行之后example2的输出结果为
   
  ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/1832415.jpg)
  
   5. dot流程图
   
     ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/74352529.jpg)
     
   


----------
#### 三、编程（修改example1）
* 修改example1，使其输出三次方
 1. 打开example1的文件夹，获取修改square.c的权限
 
    ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/761920.jpg)
    
 2. 修改square.c文件，将计算的部分由原来的i*i改为i*i*i
 
     ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/50821327.jpg)
     
 3. 运行修改后的example1文件
     * 删除之前编译产生的文件
     
     ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/3125596.jpg)
     
     * 重新编译修改后的文件
     
     ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/94549350.jpg)
     
     * 运行编译完成的文件
     
     ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/65615930.jpg)
     
  4. 输出的结果为
  
    ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/81975547.jpg)
    
  5. dot流程图
  
    ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/64200046.jpg)
    


----------
#### 四、实验感想

  这次实验主要是理解DOL实例的代码，并且对example1和example2进行修改，只要理解了代码修改部分并不难，需要注意的是如果要看到更改的dot图需要把之前编译产生的文件先删掉。

   
