![image](https://github.com/daliang0101/OpenGL/blob/main/images/GLsummary.png)  
## 术语  
* **顶点**：需要统一处理的数据包（开发者负责定义构成顶点的所有数据），如位置、颜色信息   

* **片元**： 可以视为“候选像素”，即可以存放到帧缓冲的像素
  
* **像素**：显示器上最小的可见单元

* **渲染**：从模型到生成最终图像的过程  
  
* **着色器**：为GPU编译的小型程序

## 语法  
* 函数：gl 前缀  

* 常量：GL_ 前缀，由#defines定义    

* 一类功能的函数：函数开头部分相同，在后缀做小的改变提示参数类型   
> 例：glUniform2f()和glUniform2fv()，2f表示传入2个GLfloat型参数；2fv表示用一个一维GLfloat数组传入2个GLfloat值  

## 渲染模式  
* **立即模式**：  
>1.  早期使用的固定管线模式 
>2.  优点：绘制图形很方便，容易理解、使用
>3.  缺点：效率太低，控制OpenGL计算的自由度低（OpenGL的大多数功能都被库隐藏起来，难以把握它是如何运行的）
>4.  3.2开始，规范文档开始废弃立此模式 

* **核心模式**  
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

## 渲染管线（一系列数据处理过程，将应用程序数据转换成最终渲染的图像 ）  
![image](https://github.com/daliang0101/OpenGL/blob/main/images/renderPipeline.png)
* **顶点着色（可编程）** 
>1. 对绘制命令传输的每个顶点，执行顶点着色器程序   

>2. 简单情况：将数据复制并传递到下一个着色器阶段，即传递着色器（pass-through shader）  

>3. 复杂情况：执行大量计算得到顶点在屏幕上的位置（一般使用变换矩阵计算位置）  

>4. 复杂程序可能包含许多顶点着色器，但同一时刻只能有一个顶点着色器起作用   
* **图元装配**：按照顶点传输到OpenGL服务端到时，指定的图元模式(点、线、三角形等)，把顶点组装(个人理解：按照图元规则，确定如何在顶点之间进行几何连线)成对应的图元  

* **剪切**：对落在视口(viewport)之外的顶点关联的图元做出改动，保证不对视口之外的像素进行绘制（OpenGL自动完成）  
  
* **光栅化**：把剪切后的图元传递到光栅化单元，生成对应的片元  
>1. 判断某一部分几何体所覆盖的屏幕区域  

>2. 对片元着色器中的每个变量，进行线性插值，然后把结果传递给片元着色器  

>3. 注意点：OpenGL实现光栅化和数据插值的方法与具体的平台相关，无法保证在不同平台上的插值结果总是相同  
   
* **片元着色（可编程）**
>1. 计算片元的最终颜色  

>2. 可以中止某个片元的处理，即“片元丢弃”(discard)  

>3. 对比顶点着色器

>>* 顶点着色决定了图元应该位于屏幕上的什么位置  

>>* 片元着色使用位置信息来决定片元的颜色

* 逐片元操作  
>1. 进行深度、模版测试，来决定一个片元是否可见  

>2. 片元通过了所有激活的测试，则被直接绘制到帧缓冲  

>3. 若开启混融(Blending)模式，则片元颜色会与该像素当前颜色相迭加，形成一个新的颜色值并写入帧缓存  



