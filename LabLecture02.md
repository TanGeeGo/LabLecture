# Lab Lecture 02 —— 编程语言的使用与调试
* 目前实验室中使用更多的是python和matlab，以及少数同学使用C语言
* python在深度学习和科学计算方面有较大优势，而且开源的社区可以非常快速地推动语言的进步，符合社区的需求。IDE推荐：VScode（自定义，自定义，还是自定义！），Cursor（冉冉升起的new choice），pycharm（全家桶，很多需要的功能都已集成好，就像一个matlab）。
* matlab是在python之前进行科学计算的不二之选（可以看到很多老的物理仿真都是通过matlab实现的），但由于其闭源的社区特性，更新速度不如python。我目前基本只会使用matlab进行绘图，其可交互的科学绘图编辑还是更优于python的任何一个第三方包的。
* C语言，编译以后运行速度快，如果要把程序落地做到FPGA驱动的相机上，是必须的。

### 以下内容中，**加粗**的条例是我在平时coding过程中遇到频次比较高的点，是需要重点注意的。新同学们可以重新学习一遍，老同学们也可以查漏补缺一下。
## python tutorial
1. 基础内容（仅列举需要重视的知识点，具体内容需要自己查漏补缺）
    * syntax、**variables**、strings、**numbers**（注意数值位数的区别）、booleans、constants、**type conversion**
2. 操作数
    * comparison（>[=], <[=], ==）,logical运算（and, or, not）
3. 控制流
    * **if ... elif ... else** statement
    * **for loop** with **range()/enumerate()/list()**
    * **while** —— execute a code block as long as a condition is True
    * **break**, **continue** and pass
        ```python
        for index in range(n):
            # more code here 
            if condition:
                continue / break
            else:
                pass
        ```
4. 函数
    * **def ...()** to make a function
    * default parameters and keyword arguments
    * recursive functions
    * lambda expressions
    * **docstrings** with """ (string) """
        ```python
        def add(a, b):
            "Return the sum of two arguments"
            return a + b
        ```
        ```bash
        >>> help(add)
        add(a, b)
            Return the sum of two arguments
        ```
    * ***args** and ****kwargs** 当函数需要传入不确定的参数时，需要用到这两个内容。多用于子类继承父类的成员函数中
5. 列表
    * **list** python中最常用的数据结构
        ```python
        a = list([1, 2, 3]), a.k.a, [1, 2, 3]
        ```
    * tuple 内容不变的list
    * sort a list (in place) with sorted() (sort())
        ```python
        sorted(list) # 生成一个新的list
        list.sort()  # 在原有的list地址上进行inplace操作
        ```
    * **slice the list**
    * unpack the list
    * **iterate over a list**
    * find the index of an element
        ```python
        a = ['1', '2', '3']
        print(a.index('2'))
        # output: 1
        ```
    * **iterable**, 与控制流中的for循环相同
    * 元素变换方法map(func, list) / 元素滤波方法 filter(func, list) / 元素合并方法 reduce()
        ```python
        def double(a):
            return a * 2

        a = [1, 2, 3]
        a_double = map(double, a)
        # output: [2, 4, 6]

        a_larger = filter(lambda x: x>1, a)
        # output: [2, 3]

        def sum(a, b):
            return a+b

        a_reduce = functools.reduce(sum, a)
        # output: 6 (1+2+3)
        ```

6. 字典
    * **dict** 和list一样，也是一种数据结构，通过键值keys进行索引
    * 使用**enumerate**进行遍历的方法
7. 集合
    * set (不常用，可以不了解)
8. 异常处理
    * **try ... except** 用于处理大型程序中有可能会崩的情况、数值计算出Nan的情况等等
        ```python
        try:
            # code that may cause an exception
        except ValueError as error:
            # code to handle the exception
        ```
    * **assume** 用于check部分运算的关键节点
9. 常用的包以及第三方包
    * **import** 希望能区别你的import到底import了什么？python文件？module？function？
    * 通过**sys** module来check import的路径
    * os, time, math, numpy, opencv-python (cv2), pytorch, tensorflow, jax...
10. 调试
    * jupyter notebook
    * ipython with *# %%* （如果你比较适应逐步调试，随时监视变量的话）
    * python内置的time module （如果你敲代码够快，小的问题可以直接用time.sleep调试）

## matlab tutorial
1. 科研绘图
    * **set** 设置图形对象属性。
        ```matlab
        p = plot(rand(4))
        set(p, 'Color', 'red')
        ```
        其中可以设置的**gcf**和**gca**的属性，可以通过matlab图窗的属性检查器调出，比如在字体这块，可以有'FontName', 'FontWeight', 'FontSize'三个tag可以进行调整...
    * 其他一些我常用的绘图函数：plot, scatter, boxplot, surf...
    * 最近作图中发现有些flow类型的图只能通过[R语言](https://www.runoob.com/r/r-tutorial.html)绘制（python上有类似的包，但效果不太行），所以大家可以根据自己的需求看是否要使用。
    * 科研绘图模板网站：[R-CHARTS](https://r-charts.com)
