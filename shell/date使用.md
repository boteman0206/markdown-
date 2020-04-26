# date：

**shell的时间格式化**

````shell
date -d 'now' : 显示当前时间
date -d 'now' '+%Y-%m-%d': 格式化当前时间输出2020-04-26
date -d '2 days ago': 显示2天前的时间
date -d '-2 day' ： 显示2天前的时间（同上）
date -d '2 day' : 显示2天之后的时间
date -d '25 Dec' '+%j'    #显示12月25日在当年的哪一天
date -d '3 week' : 三周后的时间
date -d '-3 week' : 三周前的时间
date -d '1 month' : 一个月之后的时间
date -d '-1 month' : 一个月之前的时间
date -d '1 year' : 一年之后的时间
date -d '-1 year' : 一年之前的时间
````



#### 输出文件：

````shell
#!/bin/bash
# 关于输入文件名称， 然按照时间日期输出特定的文件名称
read -p "请输入你的文件名称：" filename

date1=$(date -d '2 day' '+%Y-%m-%d')
date2=$(date -d '1 day' '+%Y-%m-%d')
date3=$(date -d 'now' '+%Y-%m-%d')

filename1=${filename}${date1}
filename2=${filename}${date2}
filename3=${filename}${date3}

touch ${filename1}
touch ${filename2}
touch ${filename3}

echo ${filename1}
echo ${filename2}
````

``小指令“ $（command） ”的取得讯息、变量的设置功能， 或者使用左上角的顿号来执行指令``









