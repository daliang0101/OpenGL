![image](https://github.com/daliang0101/OpenGL/blob/main/images/GLsummary.png)  
## 术语  
### 顶点(Vertices)
>* 描述几何图形顶点的数据包（开发者负责定义构成顶点的所有数据）   
>
>* 数据包中各个分量被称为属性：位置坐标、颜色值、纹理坐标等  
>
>* OpenGL绘图的原始资料  

### 图元(Primivtives)   
* 最小、最简单、不可分割的形状，用于构成3D图形   

* 类型  
>     点：GL_POINTS，对应一个几何顶点(Vertices)，大小默认一个像素，glPointSize()可改变点大小    
>
>     线：GL_LINES，对应一对几何顶点，默认宽度一个像素，glLineWidth()可改变线宽   
>
>     线带：GL_LINE_STRIP，对应一组几何顶点，依次对顶点连线形成，不闭合   
>
>     线环：GL_LINE_LOOP，对应一组几何顶点，依次对顶点连线形成，闭合  
>
>     三角形：GL_TRIANGLES，对应某三个几何顶点   
>
>     三角形带：GL_TRIANGLES_STRIP，共用一个线带(strip)上的顶点的一组三角形（映射纹理图片时常使用该类型）  

* OpenGL就是把几何顶点组装成这些基本图形，再由基本图形组成目标图形

### 片元  
> 可以视为“候选像素”(即可以存放到帧缓冲的像素)，光栅化阶段图元被分割映射到与屏幕像素对应的一个个片段
  
### 像素  
> 显示器上最小的可见单元

### 渲染
> 从模型到生成最终图像的过程  
  
### 着色器
> 为GPU编译的小型程序，渲染阶段处理顶点位置、颜色 

### VAO
> Vertex Array Object，顶点数组对象

### VBO
> Vertex Buffer Object，顶点缓冲对象

### FBO
> Frame Buffer Object，帧缓冲对象

## 语法  
### 函数
> gl 作前缀  

### 常量
> GL_ 前缀，由#defines定义

### 一类功能的函数集
> 函数开头部分相同，在后缀做小的改变提示参数类型  
>
> 例：glUniform2f()和glUniform2fv()，2f表示传入2个GLfloat型参数；2fv表示用一个一维GLfloat数组传入2个GLfloat值  

## 渲染模式  
### 立即模式：  
>*  早期使用的固定管线模式  
>
>*  优点：绘制图形很方便，容易理解、使用  
>
>*  缺点：效率太低，控制OpenGL计算的自由度低（OpenGL的大多数功能都被库隐藏起来，难以把握它是如何运行的）  
>
>*  OpenGL3.2开始，规范文档开始废弃立此模式 

### 核心模式
>*  迫使开发者使用现代的函数，试图使用一个已废弃的函数时，会抛出一个错误并终止绘图  
>
>*  所有OpenGL的更高的版本都是在3.3的基础上，引入了额外的功能，并没有改动核心架构  
>*  新版本只是引入了一些更有效率或更有用的方式去完成同样的功能  
>*  所有的概念和技术在现代OpenGL版本里都保持一致  
>*  更高的灵活性和效率，可以更深入的理解图形编程(学习难度大)  

## 状态机  
>* 一系列的变量，描述OpenGL此刻应当如何运行  
>
>* OpenGL本质是一个巨大的状态机   
>
>* OpenGL的状态通常被称为OpenGL上下文(Context)   
>
>* 更改OpenGL状态的途径：设置选项，操作缓冲，使用当前OpenGL上下文来渲染  

## 扩展  
>* OpenGL的一大特性就是对扩展(Extension)的支持 
>
>* 当一个显卡公司提出一个新特性或者渲染上的大优化，通常会以扩展的方式在驱动中实现  
```
    if(GL_ARB_extension_name) {
        // 使用硬件支持的全新的现代特性
     }  else  {
        // 不支持此扩展: 用旧的方式去做
     }    
 ```

## 渲染管线
> 一系列数据处理过程，将应用程序数据转换成最终渲染的图像
![image](https://github.com/daliang0101/OpenGL/blob/main/images/renderPipeline.png)
### 顶点着色（可编程）
>* 对绘制命令传输的每个顶点，执行顶点着色器程序   
>
>* 简单情况：将数据复制并传递到下一个着色器阶段，即传递着色器（pass-through shader）  
>
>* 复杂情况：执行大量计算得到顶点在屏幕上的位置（一般使用变换矩阵计算位置）  
>
>* 复杂程序可能包含许多顶点着色器，但同一时刻只能有一个顶点着色器起作用   

### 图元装配
> 按照顶点传输到OpenGL服务端到时，指定的图元模式(点、线、三角形等)，把顶点组装(个人理解：按照图元规则，确定如何在顶点之间进行几何连线)成对应的图元

### 剪切
> 对落在视口(viewport)之外的顶点关联的图元做出改动，保证不对视口之外的像素进行绘制（OpenGL自动完成）  
  
### 光栅化
   把剪切后的图元传递到光栅化单元，生成对应的片元  
>* 判断某一部分几何体(图元)所覆盖的屏幕区域，将图元分割映射成与屏幕像素对应的片元  
>
>* 对片元着色器中的每个变量，进行线性插值，然后把结果传递给片元着色器  
>
>* 注意点：OpenGL实现光栅化和数据插值的方法与具体的平台相关，无法保证在不同平台上的插值结果总是相同  
   
### 片元着色（可编程）
* 计算片元的最终颜色  

* 可以中止某个片元的处理，即“片元丢弃”(discard)  

* 对比顶点着色器

>     顶点着色决定了图元应该位于屏幕上的什么位置；  
>
>     片元着色使用位置信息来决定片元的颜色；

* 逐片元操作  
>     进行深度、模版测试，来决定一个片元是否可见；  
>
>     片元通过了所有激活的测试，则被直接绘制到帧缓冲；  
>
>     若开启混融(Blending)模式，则片元颜色会与该像素当前颜色相迭加，形成一个新的颜色值并写入帧缓存；  

## 通用例程(绘制三角形)
### 初始化函数 init()  
>* 设置应用程序用到的数据，如顶点、着色器、纹理  
>
>* 着色管线装配（把应用程序数据 和 着色器程序的变量关联起来）
```
声明着色器属性位置布局
enum Attrib_IDs { vPosition = 0 };

// 定义顶点数组
GLfloat  vertices[6][2] = {
              { -0.90f, -0.90f }, {  0.85f, -0.90f }, { -0.90f,  0.85f },  // Triangle 1
              {  0.90f, -0.85f }, {  0.90f,  0.90f }, { -0.85f,  0.90f }   // Triangle 2
         };
         
// 创建并激活着色器程序
GLuint shaderProgram = LoadShader(shaderInfoObj); // 封装的创建方法着色器方法
glUseProgram(shaderProgram);

// 声明VAO、VBO
GLuint VAO, VBO;

// 初始化VAO、VBO
glGenVertexArrays(1, &VAO);
glGenBuffers(1, &VBO);

// 绑定VAO、VBO
glBindVertexArray(VAO);
glBindBuffer(GL_ARRAY_BUFFER, VBO);

// 上传顶点数据到OpenGL服务端
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, 0);

// 着色管线装配
glVertexAttribPointer(vPosition, 2, GL_FLOAT, GL_FALSE, 0, (void*)0);
glEnableVertexAttribArray(vPosition);

```
### 绘制函数 display()   
>* glClear(...) 清屏  
>
>* glBind...() 绑定渲染所需的对象(可以有多个VAO, VBO，可以在这个函数中切好绑定的对象，对每一次绑定动作分别执行渲染)  
>
>* glDraw...() 开始渲染  

```
// 设置清屏颜色
glClearColor(0.1f, 0.1f, 0.1f, 1.0f);

// 执行清屏动作
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);

// 绑定需要绘制的VAO（根据需求，可以绑定不同的VAO）
glBindVertexArray(VAO);

// 开始绘制
glDrawArrays(GL_TRIANGLES, 0, 6);

```

### 主函数 main()  
>* 创建窗口 (交给glfw等三方库承担)     
>
>* 程序初始化 init()  
>
>* 循环体中执行绘制 display()  
>
>* 销毁窗口   
>
>* 释放OpenGL资源

```
glfw初始化及创建窗口代码;

// 程序初始化函数
init();

// 渲染循环体
while(!glfwWindowShouldClose(window)) {
    display();
    glfwSwapBuffers(window);
    glfwPollEvents
}

// 释放OpenGL资源
glDeleteVertexArrays(1, &VAO);  
glDeleteBuffers(1, &VBO);

// glfw 收到关闭窗口消息，执行销毁窗口动作，中止glfw(消息机制等)
glfwDestroyWindow(window);
glfwTerminate();

```
### 例程详解  
#### init中各OpenGL API详解  
* `glGenVertexArrays(1, &VAO);`  

> 原型：  
>    * glGenVertexArrays(GLsizei n, GLuint *arrays)  
>  
> 解释：  
>   * 返回n个未使用的对象名到数组arrays中，作为顶点数组对象使用，若n<0，产生GL_INVALID_VALUE错误   
>
>   
>    理解：
>     1. 与C语言内存分配返回指针类似，这里只不过是分配OpenGL服务端空间(显存空间)，同样返回一个指向显存空间的指针，赋给VAO变量；  
>
>     2. VAO代表了一块特定类型的显存空间，这个类型从API名称(VertexArrays)可以看出，    
>
>        是一块连续的数组空间，用来接收从应用程序传送过来的顶点数据；  
>
>     3. 从OpenGL状态机理解，对象代表了状态机中的各种状态，客户端调用OpenGL创建对象的API，  
>  
>        就是告诉OpenGL服务端：我需要一定数量某种状态对象，把它们的编号分配给我；
>         
>        由于同一类型的对象可以有很多，客户端如果要对某种类型的对象执行操作(如上传顶点数据、纹理贴图)，  
>
>        必须使用glBind...命令激活指定的对象，也就是告诉OpenGL服务端，
>
>        我(应用程序)想与你xxx这个编号的对象通信，之后对该类型执行的操作就会作用于激活(绑定)的对象；    

* `glBindVertexArray(VAO);`  
>
> 原型：  
>    * glBindVertexArray(GLuint array)
>
> 解释：  
>    1. 如果array非0且是由glGenVertexArrays()返回的，则激活顶点数组对象array；  
>  
>    2. 如果array为0，意味对之前绑定的顶点数组对象进行解绑定；
>  
>    3. 如果array不是glGenVertexArrays()返回的，或者已被glDeleteVertexArrays()释放掉了，会产生一个GL_INVALID_OPERATION错误；  
>  
>  
> 理解：  
>
>     1. 类似铁路道岔开关，各岔道就是某一类型的各个对象，列车就是客户端-服务端之间传递的某类型的数据；  
>  
>     2. 与哪条岔道连接，列车就驶向哪条岔到，也就是数据会流向那个对象指向的显存空间；


* `glVertexAttribPointer(vPosition, 2, GL_FLOAT, GL_FALSE, 0, (void*)0);`  

> 原型：
>    * glVertexAttribPointer(GLuint index, GLint size, GLenum type, GLboolean normalized, GLsizei stride, const GLvoid \*pointer); 
>
> 解释：




































