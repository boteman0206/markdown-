#### Collections的常用方法



```java
void reverse(List list)//反转
void shuffle(List list)//随机排序
void sort(List list)//按自然排序的升序排序
void sort(List list, Comparator c)//定制排序，由Comparator控制排序逻辑
void swap(List list, int i , int j)//交换两个索引位置的元素
void rotate(List list, int distance)//旋转。当distance为正数时，将list后distance个元素整体移到前面。当distance为负数时，将 list的前distance个元素整体移到后面。
```



#### 定制排序：

```java
// 定制排序的用法
List list = new ArrayList(Arrays.asList(1,23,3,56,7,12));
Collections.sort(list, new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
        /**
        * 升序排的话就是第一个参数.compareTo(第二个参数)
        * 降序排的话就是第二个参数.compareTo(第一个参数)
        */
        return o2.compareTo(o1);
    }
});
System.out.println("定制排序后：");
System.out.println(list);
```