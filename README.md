![image](https://github.com/daliang0101/OpenGL/blob/main/images/GLsummary.png)  
## 术语  
* 片元： 可以视为“候选像素”，即可以存放到帧缓冲的像素
  
* 像素：显示器上最小的可见单元

* 渲染：从模型到生成最终图像的过程  
  
* 着色器：为GPU编译的小型程序

## 语法  
* 函数：gl 前缀  

* 常量：GL_ 前缀，由#defines定义    

* 一类功能的函数：函数开头部分相同，在后缀做小的改变提示参数类型   
> 例：glUniform2f()和glUniform2fv()，2f表示传入2个GLfloat型参数；2fv表示用一个一维GLfloat数组传入2个GLfloat值  

## 渲染模式  
* 立即模式：  
>1.  早期使用的固定管线模式 
>2.  优点：绘制图形很方便，容易理解、使用
>3.  缺点：效率太低，控制OpenGL计算的自由度低（OpenGL的大多数功能都被库隐藏起来，难以把握它是如何运行的）
>4.  3.2开始，规范文档开始废弃立此模式 

* 核心模式  
>1.  迫使开发者使用现代的函数，试图使用一个已废弃的函数时，会抛出一个错误并终止绘图
>2.  所有OpenGL的更高的版本都是在3.3的基础上，引入了额外的功能，并没有改动核心架构  
>3.  新版本只是引入了一些更有效率或更有用的方式去完成同样的功能  
>4.  所有的概念和技术在现代OpenGL版本里都保持一致  
>5.  更高的灵活性和效率，可以更深入的理解图形编程(学习难度大)  

## 状态机  
* 一系列的变量，描述OpenGL此刻应当如何运行  

* OpenGL本质是一个巨大的状态机   

* OpenGL的状态通常被称为OpenGL上下文(Context)   

* 更改OpenGL状态的途径：设置选项，操作缓冲，使用当前OpenGL上下文来渲染  

## 扩展  
* OpenGL的一大特性就是对扩展(Extension)的支持 

* 当一个显卡公司提出一个新特性或者渲染上的大优化，通常会以扩展的方式在驱动中实现  
```
    if(GL_ARB_extension_name) {
        // 使用硬件支持的全新的现代特性
     }  else  {
        // 不支持此扩展: 用旧的方式去做
     }    
 ```


