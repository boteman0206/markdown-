# read：

**读取键盘的输入变量**

````shell
[dmtsai@study ~]$ read [-pt] variable
选项与参数：
-p  ：后面可以接提示字符！
-t  ：后面可以接等待的“秒数！”这个比较有趣～不会一直等待使用者啦！
范例一：让使用者由键盘输入一内容，将该内容变成名为 atest 的变量
[dmtsai@study ~]$ read atest
This is a test        &lt;==此时光标会等待你输入！请输入左侧文字看看
[dmtsai@study ~]$ echo ${atest}
This is a test          &lt;==你刚刚输入的数据已经变成一个变量内容！
范例二：提示使用者 30 秒内输入自己的大名，将该输入字串作为名为 named 的变量内容
[dmtsai@study ~]$ read -p "Please keyin your name: " -t 30 named
Please keyin your name: VBird Tsai   &lt;==注意看，会有提示字符喔！
[dmtsai@study ~]$ echo ${named}
VBird Tsai        &lt;==输入的数据又变成一个变量的内容了！
````



# declare / typeset :

``declrae和typeset的功能一样，就是宣告变量的类型。如果使用 declare 后面并没有接任何参数，那么 bash 就会主动的将所有的变量名称与内容通通叫出来，就好像使用 set 一样啦！ ``

````shell
[dmtsai@study ~]$ declare [-aixr] variable
选项与参数：
-a  ：将后面名为 variable 的变量定义成为阵列 （array） 类型
-i  ：将后面名为 variable 的变量定义成为整数数字 （integer） 类型
-x  ：用法与 export 一样，就是将后面的 variable 变成环境变量；
-r  ：将变量设置成为 readonly 类型，该变量不可被更改内容，也不能 unset
范例一：让变量 sum 进行 100+300+50 的加总结果
[dmtsai@study ~]$ sum=100+300+50
[dmtsai@study ~]$ echo ${sum}
100+300+50  &lt;==咦！怎么没有帮我计算加总？因为这是文字体态的变量属性啊！
[dmtsai@study ~]$ declare -i sum=100+300+50
[dmtsai@study ~]$ echo ${sum}
450         &lt;==瞭乎？？
````

``1： 变量默认是字符串``

``2： bash环境中的数值运算，默认最多仅仅能达到整数形态，所以1/3结果是0``



# 阵列（array）：

````shell
范例：设置上面提到的 var[1] ～ var[3] 的变量。
[dmtsai@study ~]$ var[1]="small min"
[dmtsai@study ~]$ var[2]="big min"
[dmtsai@study ~]$ var[3]="nice min"
[dmtsai@study ~]$ echo "${var[1]}, ${var[2]}, ${var[3]}"
small min, big min, nice min
````





