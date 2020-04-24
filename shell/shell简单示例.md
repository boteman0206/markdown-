# shell

### 简单使用：

```shell
#!/bin/bash
# first bash to hello world  exit作用：退出的时候回传100 使用echo $? 输出exit的值
echo "hello this is a shell!"
exit 100
```

**exit作用：退出的时候回传100 使用echo $? 输出exit的值**



### 在一个shell脚本中调用其他shell：

```shell
#!/bin/bash
# first bash to hello world  exit作用：退出的时候回传100 使用echo $? 输出exit的值
PATH=/home/atomu/temp/shell
export PATH
echo "hello this is a shell!"
source shell2.sh
exit 100
```

``注意点： PATH=/home/atomu/temp/shell 指定了path之后后面才可以 直接使用 source shell2.sh， 否则会找不到脚本shell2.sh``

