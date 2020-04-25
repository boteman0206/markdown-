# &&(and):



````shell
范例一：使用 ls 查阅目录 /tmp/abc 是否存在，若存在则用 touch 创建 /tmp/abc/hehe
[dmtsai@study ~]$ ls /tmp/abc && touch /tmp/abc/hehe
ls: cannot access /tmp/abc: No such file or directory
# ls 很干脆的说明找不到该目录，但并没有 touch 的错误，表示 touch 并没有执行
[dmtsai@study ~]$ mkdir /tmp/abc
[dmtsai@study ~]$ ls /tmp/abc && touch /tmp/abc/hehe
[dmtsai@study ~]$ ll /tmp/abc
-rw-rw-r--. 1 dmtsai dmtsai 0 Jul  9 19:16 hehe
````



# ||(or)：

````shell
范例二：测试 /tmp/abc 是否存在，若不存在则予以创建，若存在就不作任何事情
[dmtsai@study ~]$ rm -r /tmp/abc                &lt;==先删除此目录以方便测试
[dmtsai@study ~]$ ls /tmp/abc || mkdir /tmp/abc
ls: cannot access /tmp/abc: No such file or directory  &lt;==真的不存在喔！
[dmtsai@study ~]$ ll -d /tmp/abc
drwxrwxr-x. 2 dmtsai dmtsai 6 Jul  9 19:17 /tmp/abca   &lt;==结果出现了！有进行 mkdir
````

