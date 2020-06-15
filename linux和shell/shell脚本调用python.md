## shell脚本调用python脚本：

**1： shell调用python 脚本**

``python脚本 demo1.py``

````python

import sys
print sys.argv[0]  # 输出的是文件的名称
print sys.argv[1]  # 输出的是传入的第一个参数
print sys.argv[2]  # 输入的是传入的第二个参数

def hello(n1=90, n2=100):
    print "hello world!"


if __name__ == '__main__':
    hello()
````

`shell脚本：`

````shell
#!/bin/bash
#shell脚本调用python程序

PATH=${PATH}":/home/shellTest"
echo ${PATH}
echo "开始调用python脚本文件"

read -p "请输入你的参数1 = " p1
read -p "请输入你的参数二 = " p2

python demo1.py ${p1} ${p2}

echo "调用脚本结束"
````

