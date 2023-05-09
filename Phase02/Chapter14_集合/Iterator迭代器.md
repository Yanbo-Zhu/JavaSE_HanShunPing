

# 1 ListIterator

https://blog.csdn.net/u012804490/article/details/29180561

ListIterator的父接口是Iterator，是List接口中特有的迭代器。

ListIterator在Iterator的基础上，又新添了很多方法：

Iterator中的方法：
1、判断是否有下一个元素：hasNext(); 
2、获取下一个元素：            next();
3、删除迭代器指向的元素：remove();

ListIterator新添的方法：
4、判断是否有前一个元素：hasPrevious();
5、获取前一个元素：            previous();
6、添加元素：                        add(e);
7、获取next后续元素的索引：    nextIndex();
8、获取previous后续元素的索引：previousIndex();
9、替换指定元素：                 set(E e);


## 1.1 例子
使用ListIterator的好处：可以并发执行操作，Iterator不能，Iterator如果并发执行操作，迭代器会出现不确定性行为。
```java
package list_set;
 
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
 
public class Main {
	public static void main(String[] args)
	{
		List list=new ArrayList();
		listIteratorDemo(list);
	}
	public static void listIteratorDemo(List list)
	{
		list.add("abc1");
		list.add("abc2");
		list.add("abc3");
		list.add("abc4");
		
		// 迭代器
		Iterator it=list.iterator();
		
		while(it.hasNext())
		{
			//System.out.println(it.next());
			Object obj=it.next(); // ConcurrentModificationException并发修改异常
			if(obj.equals("abc2"))
			{
				list.add("hello");
			}
			System.out.println(obj);
		}
	}
}
```

![](https://img-blog.csdn.net/20140607113225734?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjgwNDQ5MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

使用ListIterator就可以解决这种异常！

代码如下：
```java
package list_set;
 
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.ListIterator;
 
public class Main {
	public static void main(String[] args)
	{
		List list=new ArrayList();
		listIteratorDemo2(list);
	}
	
	private static void listIteratorDemo2(List list) {
		list.add("abc1");
		list.add("abc2");
		list.add("abc3");
		list.add("abc4");
		
		// 获取列表迭代器
		ListIterator it=list.listIterator();
		
		while(it.hasNext())
		{
			Object obj=it.next();
			if(obj.equals("abc2"))
			{
				it.add("hello");
			}
			System.out.println(obj);
		}
		System.out.println(list);
	}
}
```

![](https://img-blog.csdn.net/20140607114134640)