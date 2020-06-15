# test指令:

````shell
测试的标志	代表意义
1. 关于某个文件名的“文件类型”判断，如 test -e filename 表示存在否
-e	该“文件名”是否存在？（常用）
-f	该“文件名”是否存在且为文件（file）？（常用）
-d	该“文件名”是否存在且为目录（directory）？（常用）
-b	该“文件名”是否存在且为一个 block device 设备？
-c	该“文件名”是否存在且为一个 character device 设备？
-S	该“文件名”是否存在且为一个 Socket 文件？
-p	该“文件名”是否存在且为一个 FIFO （pipe） 文件？
-L	该“文件名”是否存在且为一个链接文件？

2. 关于文件的权限侦测，如 test -r filename 表示可读否 （但 root 权限常有例外）
-r	侦测该文件名是否存在且具有“可读”的权限？
-w	侦测该文件名是否存在且具有“可写”的权限？
-x	侦测该文件名是否存在且具有“可执行”的权限？
-u	侦测该文件名是否存在且具有“SUID”的属性？
-g	侦测该文件名是否存在且具有“SGID”的属性？
-k	侦测该文件名是否存在且具有“Sticky bit”的属性？
-s	侦测该文件名是否存在且为“非空白文件”？

3. 两个文件之间的比较，如： test file1 -nt file2
-nt	（newer than）判断 file1 是否比 file2 新
-ot	（older than）判断 file1 是否比 file2 旧
-ef	判断 file1 与 file2 是否为同一文件，可用在判断 hard link 的判定上。 主要意义在判定，两个文件是否均指向同一个 inode 哩！

4. 关于两个整数之间的判定，例如 test n1 -eq n2
-eq	两数值相等 （equal）
-ne	两数值不等 （not equal）
-gt	n1 大于 n2 （greater than）
-lt	n1 小于 n2 （less than）
-ge	n1 大于等于 n2 （greater than or equal）
-le	n1 小于等于 n2 （less than or equal）

5. 判定字串的数据
test -z string	字符串的长度为零则为0 ？若 string 为空字串，则为 true
test -n string	字符串的长度为零则为0 ？若 string 为空字串，则为 false。 -n 亦可省略
test str1 == str2	判定 str1 是否等于 str2 ，若相等，则回传 true
test str1 != str2	判定 str1 是否不等于 str2 ，若相等，则回传 false

6. 多重条件判定，例如： test -r filename -a -x filename
-a	（and）两状况同时成立！例如 test -r file -a -x file，则 file 同时具有 r 与 x 权限时，才回传 true。
-o	（or）两状况任何一个成立！例如 test -r file -o -x file，则 file 具有 r 或 x 权限时，就可回传 true。
!	反相状态，如 test ! -x file ，当 file 不具有 x 时，回传 true
````



##### 示例：

````shell
[dmtsai@study bin]$ vim file_perm.sh
#!/bin/bash
# Program:
#    User input a filename, program will check the flowing:
#    1.） exist? 2.） file/directory? 3.） file permissions
# History:
# 2015/07/16    VBird    First release
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
# 1\. 让使用者输入文件名，并且判断使用者是否真的有输入字串？
echo -e "Please input a filename, I will check the filename's type and permission. \n\n"
read -p "Input a filename : " filename
test -z ${filename} && echo "You MUST input a filename." && exit 0
# 2\. 判断文件是否存在？若不存在则显示讯息并结束脚本
test ! -e ${filename} && echo "The filename '${filename}' DO NOT exist" && exit 0
# 3\. 开始判断文件类型与属性
test -f ${filename} && filetype="regulare file"
test -d ${filename} && filetype="directory"
test -r ${filename} && perm="readable"
test -w ${filename} && perm="${perm} writable"
test -x ${filename} && perm="${perm} executable"
# 4\. 开始输出信息！
echo "The filename: ${filename} is a ${filetype}"
echo "And the permissions for you are : ${perm}"
````



##### 使用中括号  [] 来判断：

**注意点**

- 在中括号 [] 内的每个元件都需要有空白键来分隔；

- 在中括号内的变量，最好都以双引号括号起来；

- 在中括号内的常数，最好都以单或双引号括号起来。

  为什么要这么麻烦啊？直接举例来说，假如我设置了 name="VBird Tsai" ，然后这样判定： 

````shell
[  "$HOME"  ==  "$MAIL"  ]
[□"$HOME"□==□"$MAIL"□]
 ↑       ↑  ↑       ↑

[dmtsai@study ~]$ name="VBird Tsai"
[dmtsai@study ~]$ [ ${name} == "VBird" ] # 错误写法
bash: [: too many arguments
#见鬼了！怎么会发生错误啊？bash 还跟我说错误是由于“太多参数 （arguments）”所致！ 为什么呢？因为 ${name} 如果没有使用双引号刮起来
[dmtsai@study ~]$ [ "${name}" == "VBird" ] # 正确写法
````

``这可是差很多的喔！另外，中括号的使用方法与 test 几乎一模一样啊～ 只是中括号比较常用在[条件判断式 if ….. then ….. fi]``



