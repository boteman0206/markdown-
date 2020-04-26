## case...esac：

**示例模板**

````shell
case  $变量名称 in   &lt;==关键字为 case ，还有变量前有钱字号
  "第一个变量内容"）   &lt;==每个变量内容建议用双引号括起来，关键字则为小括号 ）
    程序段
    ;;            &lt;==每个类别结尾使用两个连续的分号来处理！
  "第二个变量内容"）
    程序段
    ;;
  *）                  &lt;==最后一个变量内容都会用 * 来代表所有其他值
    不包含第一个变量内容与第二个变量内容的其他程序执行段
    exit 1
    ;;
esac                  &lt;==最终的 case 结尾！“反过来写”思考一下！
````

##### 示例代码：

````shell
[dmtsai@study bin]$ vim hello-3.sh
#!/bin/bash
# Program:
#     Show "Hello" from $1.... by using case .... esac
# History:
# 2015/07/16    VBird    First release
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
case ${1} in
  "hello"）
    echo "Hello, how are you ?"
    ;;
  ""）
    echo "You MUST input parameters, ex&gt; {${0} someword}"
    ;;
  *）   # 其实就相当于万用字符，0~无穷多个任意字符之意！
    echo "Usage ${0} {hello}"
    ;;
esac
````



#### 结合function功能使用：

````shell
function fname（） {
    程序段
}
````



``另外， function 也是拥有内置变量的～他的内置变量与 shell script 很类似， 函数名称代表示 $0 ，而后续接的变量也是以 $1, $2… 来取代的～ 这里很容易搞错喔～因为“ function fname（） { 程序段 } ”内的 $0, $1… 等等与 shell script 的 $0 是不同的。以上面 show123-2.sh 来说，假如我下达：“ sh show123-2.sh one ” 这表示在 shell script 内的 $1 为 "one" 这个字串。但是在 printit（） 内的 $1 则与这个 one 无关。 我们将上面的例子再次的改写一下，让你更清楚！ ``

````shell
[dmtsai@study bin]$ vim show123-3.sh
#!/bin/bash
# Program:
#    Use function to repeat information.
# History:
# 2015/07/17    VBird    First release
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
function printit（）{
    echo "Your choice is ${1}"   # 这个 $1 必须要参考下面指令的下达，代指调用函数传递的参数
}
echo "This program will print your selection !"
case ${1} in       # 这里的这个${1} 指的是执行shell脚本所带的参数
  "one"）
    printit 1  # 请注意， printit 指令后面还有接参数！
    ;;
  "two"）
    printit 2
    ;;
  "three"）
    printit 3
    ;;
  *）
    echo "Usage ${0} {one&#124;two&#124;three}"
    ;;
esac
````

