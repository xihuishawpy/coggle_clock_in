
[TOC]
# 在目录下创建py文件，并进行运行

## os模块
在Python中有一个内置库os，是一个系统接口库，operating system interfaces。在linux系统中处理数据、运行脚本的时候，经常会操作文件和目录，所以os库就是起这个作用，对于固定逻辑的文件、目录的操作，都可以写成脚本的形式。


下面就介绍几种常用方法：

1. getcwd
   
   获取当前目录路径

   ```python
    [I have no name!@i-7lo31rsr Ceallach_Shaw]$pwd
    /home/coggle/Ceallach_Shaw
    [I have no name!@i-7lo31rsr Ceallach_Shaw]$vi os_test.py

   ```

   新建py文件，引入os模块，利用getcwd方法，打印当前路径。注意：这里用的vi，所以退出保存的方法是，esc退出insert模式，切换到命令行模式，输入wq，回车保存。（后文对vi的使用不再赘述了，忘记了的同学就翻阅上一篇文章 [Linux基础（一）](https://mp.weixin.qq.com/s/KjvEMM_dKfI5T9WxYyHl2w)
   
   ```python
   import os

   print(os.getcwd())
   ```

   命令行下，运行py文件，打印的路径与当前路径一致。

   ```shell
    [I have no name!@i-7lo31rsr Ceallach_Shaw]$python3 os_test.py 
    
    /home/coggle/Ceallach_Shaw
   ```

2. path.abspath('.')
   
   `.`代表当前路径，查看当前目录的绝对路径

   ```python
   import os

   #print(os.getcwd())

   print(os.path.abspath('.'))
    ```

    运行py文件：

    ```shell
    [I have no name!@i-7lo31rsr Ceallach_Shaw]$python3 os_test.py 
    
    /home/coggle/Ceallach_Shaw
    ```

    补充：
    - “.”表示当前目录，也可以用“./”表示；

    - “..”表示上一级目录，也可以用“../”表示；

    - “~” 代表用户的宿主目录；

    - “/”处于Linux文件系统树形结构的最顶端，我们称它为Linux文件系统的root，它是Linux文件系统的入口（参考上一篇文章内容中的目录结构图[Linux基础（一）](https://mp.weixin.qq.com/s/KjvEMM_dKfI5T9WxYyHl2w)）

3. listdir('.')

    列出当前目录下所有文件与目录

    ```python
    import os

    l=os.listdir('.')
    print(l)
    ```

    ![](https://files.catbox.moe/ga84oz.png)

    如果只是需要列出当前目录下的所有目录（文件不需要），则在for循环后面加个条件判断是否为目录 `if os.path.isdir(x)`

    ```python
    [x for x in os.listdir('.') if os.path.isdir(x)]
    ```
    ![](https://files.catbox.moe/h6e85x.png)


4. mkdir 、rmdir

   mkdir-创建目录、rmdir-删除目录

   在当前目录下创建一个dir2的目录：
   
   ```python
   import os

   os.mkdir(os.getcwd()+'/dir2')
   ```
   
   dir2目录创建成功：
    
   ```shell
   [I have no name!@i-7lo31rsr Ceallach_Shaw]$python3 os_test.py 
   [I have no name!@i-7lo31rsr Ceallach_Shaw]$ls
   affairs.txt  coggle  dir  dir2	os_test.py  test.py  test2.txt
   ```

   补充：
   - removedirs，递归删除目录
   - remove，移除文件

5. rename
   
   `文件或者目录`重命名 

   把目录dir2重命名为dir3:
   ```python
   import os

   os.rename('dir2','dir3')
   ```
    ```shell

    [I have no name!@i-7lo31rsr Ceallach_Shaw]$python3 os_test.py 
    [I have no name!@i-7lo31rsr Ceallach_Shaw]$ls
    affairs.txt  coggle  dir  dir3	os_test.py  test.py  test2.txt
    ```

6. path.splitext

    输入文件的绝对路径，得到文件扩展名

    ```python
    import os

    print(os.path.splitext(os.getcwd()+'os_test.py'))
    ```
    输出：

    ('/home/coggle/Ceallach_Shawos_test', '.py')

     列出当前目录下所有.py文件，需加上条件判断是否为文件 `if os.path.isfile(x)`:

    ```python
    l=[x for x in os.listdir('.') 
                if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
    
    print(l)
    ```
    ![](https://files.catbox.moe/we1n6d.png)


## sys模块

sys 模块主要负责`与 Python 解释器进行交互`，该模块提供了一系列用于控制 Python 运行环境的函数和变量。


1. sys.argv
   
   实现从程序`外部`向程序传递参数，简单说，sys.argv[]就是一个从程序外部获取参数的桥梁，其返回的时一个参数列表。第一个元素是程序本身，随后才依次是外部给予的参数。

   下面就是打印python的帮助信息：

   ![](https://files.catbox.moe/4bgbht.png)

   ```python
   import sys

   print(sys.argv)
   ```
   输出：
   ![](https://files.catbox.moe/j3og6x.png)

   可见，输出第一个参数是文件名，后面的参数就是外部传入的参数。

   我们经常看到在命令行的情况下，输入--help,--version等参数，即可打印出相关信息。sys.argv的作用就是传递外部参数值到目标方法/函数中去。

   再看一个书上的相关例子，关于--help,--version参数的实现：

   ```python
   import sys
   def readfile(filename):
      f=open(filename)
      while True:
         line=f.readline()
         if len(line)==0:
               break
               print("wrong! EOF")
         print(line)
   if len(sys.argv) < 2:
      print ('No action specified.')
      sys.exit()
   
   # 从第二个参数（外部传入的第一个参数），以--开头的，并且从这个参数的第三个字符开始截取
   if sys.argv[1].startswith('--'):
      option = sys.argv[1][2:]
      # 如果--后面的字符是version，则输出Version 1.2
      if option == 'version':
         print ('Version 1.2')
      elif option == 'help':
         print ('''\
   This program prints files to the standard output.
   Any number of files can be specified.
   Options include:
   --version : Prints the version number
   --help    : Display this help''')
      else:
         print ('Unknown option.')
      sys.exit()
   else:
      for filename in sys.argv[1:]:
         readfile(filename)   

   ```

2. sys.path

   >获取指定模块搜索路径的字符串集合。

   当我们导入一个模块时：import xxx，默认情况下python解析器会搜索并按照当前目录、已安装的内置模块和第三方模块这个顺序打印出来，搜索路径存放在sys模块的path中：

   ```python
   import sys

   print(sys.path)
   ```

   ![](https://files.catbox.moe/6espxk.png)

   现在有一种很常见的情况，就是你写好了Py文件，放在另外一个目录下，如果当前目录下的py文件需要引用写好的py文件里的方法的时候，就需要将存放py脚本的目录加入sys.path的列表中。

   对于`需要引用的模块和需要执行的脚本文件不在同一个目录时`，可以按照如下形式来添加路径：
   ```python
   import sys  
   sys.path.append(’需要引用模块的地址')  

   # sys.path.append(..)   # 这代表添加当前路径的上一级目录
   ```


## 练习

1. 使用os模块打印/usr/bin/路径下所有以m开头的文件

```python
import os 

# 先切换到/usr/bin/目录下
c = os.chdir('/usr/bin/')

# path.isfile方法判断是否为文件，startswith判断字符串开头首字母m
l = [x for x in os.listdir(c) 
            if os.path.isfile(x) and x.startswith('m')]

print(l)
```

![](https://files.catbox.moe/lil113.png)

2. 打印命令行参数

```python
import sys

# 第一个参数为文件名，外部传入的参数从第二个参数开始
print(sys.argv[1:])

```

![](https://files.catbox.moe/wm4fhc.png)




---

参考链接：

1. https://docs.python.org/zh-cn/3/library/os.html#os-file-dir
2. https://www.cnblogs.com/peterwong666/p/11059673.html
