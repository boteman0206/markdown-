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

