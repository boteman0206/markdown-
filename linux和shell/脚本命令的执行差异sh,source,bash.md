# 不同命令执行脚本方式的差异

#### sh方式执行：

````shell
[dmtsai@study bin]$ echo ${firstname} ${lastname}
    &lt;==确认了，这两个变量并不存在喔！
[dmtsai@study bin]$ sh showname.sh
Please input your first name: VBird &lt;==这个名字是鸟哥自己输入的
Please input your last name:  Tsai 
Your full name is: VBird Tsai      &lt;==看吧！在 script 运行中，这两个变量有生效
[dmtsai@study bin]$ echo ${firstname} ${lastname}
    &lt;==事实上，这两个变量在父程序的 bash 中还是不存在的！
````

##### 执行方式：

``开启子程序的方式执行脚本， 执行完成之后，子程序 bash 内的所有数据便被移除，因此上表的练习中，在父程序下面 echo ${firstname} 时， 就看不到任何东西了``

![](../picture/sh.gif)



#### source方式执行：

````shell
[dmtsai@study bin]$ source showname.sh
Please input your first name: VBird
Please input your last name:  Tsai
Your full name is: VBird Tsai
[dmtsai@study bin]$ echo ${firstname} ${lastname}
VBird Tsai  &lt;==嘿嘿！有数据产生喔！
````

##### 执行方式：

``因为 source 对 script 的执行方式可以使用下面的图示来说明！ showname.sh 会在父程序中执行的，因此各项动作都会在原本的 bash 内生效！这也是为啥你不登出系统而要让某些写入 ~/.bashrc 的设置生效时，需要使用“ source ~/.bashrc ”而不能使用“ bash ~/.bashrc ”是一样的啊！ ``

![](../picture/source.gif)

