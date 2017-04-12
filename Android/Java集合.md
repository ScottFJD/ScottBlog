#Java 容器类（集合类）

java 容器类  用途就是保存对象，分两种不同的方式

##Collection
一个独立元素的序列。List 必须按元素的插入顺序保存element元素；Set 不能有重复的element；Queue 按照排队顺序来确定element的顺序，与插入的顺序相同。
java SDK 中List 、Set 接口都是继承自Collection

###List
List将元素维护在特定的序列中。使用此接口可以控制每个元素插入的位置，用户可以使用索引（下标）来访问List中的元素。
####ArrayList
它长于随机访问元素，但是在List的中间插入和移除元素时较慢。每个ArrayList实例都有一个容量（Capacity），即用于存储元素的数组的大小。这个容量可随着不断添加新元素而自动增加，但是增长的算法并没有定义。当需要插入大量元素时，在插入前可以调用ensureCapacity方法来增加ArrayList的容量以提高插入效率。
####LinkedList
它通过代价较低的在List中间进行插入和删除操作，提供了优化的顺序访问。LinkedList在随机访问方面相对比较慢。
此外LinkedList提供额外的get，remove，insert方法在LinkedList的首部或尾部。这些操作使LinkedList可被用作堆栈（stack），队列（queue）或双向队列（deque）。

注意LinkedList没有同步方法。如果多个线程同时访问一个List，则必须自己实现访问同步。一种解决方法是在创建List时构造一个同步的List：
List list = Collections.synchronizedList(new LinkedList(…));

#####常用方法：
* contains() 用来确定某个对象是否在列表中。
* indexOf() 获取对象所处位置的索引编号。
* subList() 获取列表的某个片段。
* containsAll() 判断某个列表中是否存在containsAll()所指的列表
* retainAll() 保留两了列表所共有的元素
* removeAll() 移除list中removeAll()所指的元素
* set() 在指定索引位置 替换原先的参数
* isEmpty() 判断是否为空
* addAll() 插入新的列表
* clear() 清空
* toArray() 将list转换成数组
* Arrays.asList() 将数组转成list

[具体的应用见github](放GitHub地址)

###Set
每个相同的项只保存一次；Set的类型有 HashSet、TreeSet和LinkedHashSet。 TreeSet 是按结果的升序保存对象，LinkedHashSet 按照添加的顺序保存对象。HashSet 可以最快的获取元素，存储方式特殊。

###Interator

迭代器 简单概述就是不需要知道容器类型，就可以对容器进行遍历。
例如：


```
list<Pet> pets = Pets.arrayList(12);
Interator<Pet> it = pets.interator();
while(it.hasNext())
{
	Pet p = it.next();
	System.out.print(p.id()+":"+p+" ");
}
```
查看源码可知

```
    public Iterator<E> iterator() {
        return new Itr();
    }

    /**
     * An optimized version of AbstractList.Itr
     */
    private class Itr implements Iterator<E> {
    
    在这个class 中对iterator接口的方法做了具体的实现过程。
    hasNext()
    next()
    remove()
    forEachRemaining()
    ......
    } 
```
其实继续往下看会发现
在ArrayList 的中实现了List接口在List接口继承自Collection接口，在Collection接口中创建了Iterator接口实例

每个容器类中都实现了Iterator接口，所以我们可以不需知道它到底是哪个容器类（例如：ArrayList、LinkedList、HashSet、TreeSet）就可以对它进行遍历。而这也展示了iterator的真正威力，能够将遍历序列的操作与序列底层的结构分离。



ListIterator 这个接口又是干嘛的


default void remove() default


##Map
一组成对的“键值”对象，允许使用键来查找值。分三种类型 HashMap、TreeMap和LinkedHashMap。 与Set的差不多，HashMap 查找速度最快，TreeMap 按结果的升序排列 LinkedHashMap 按插入的顺序排列。

Map.put(Key,value) 方法增加一个值

Map.get(key) 获取键相关联的值

##总结：
* 数组将数字与对象联系起来。它保存类型明确的对象，查询对象时，不需要对结果做类型转换。可以是多维的，可以保存基本类型（字符、布尔、数值）的数据。但是数组一旦生成，其容量就不能改变。
* Collection 保存单一的元素，而Map保存相关联的键值对。有了java范型，就可以指定容器存放的对象类型。Collection和Map都可以在你向其中添加更多的元素的时候，自动调整尺寸。容器不能持有基本类型，但是自动包装机制会仔细地执行基本类型到容器中所持有的包装器类型之间的双向转换。
* 数组和list都是排好序的容器，list能自动扩展。
* 如果进行大量的随机访问，就使用ArrayList；如果经常从表中间插入或删除元素，就使用LinkedList
* 各种Queue以及栈的行为，由LinkedList提供支持。
* Map是一种将对象与对象相关联的设计。HashMap设计用来快速访问；而TreeMap保持“键”始终处于排序状态，没有HashMap快。LinkedMap保持元素插入的顺序，但是也通过散列提供了快速访问能力。
* Set不接受重复元素。HashSet提供最快的查询速度，而TreeSet保持元素处于排序状态。LinkedHashSet 以插入顺序保存元素。
* 新程序中不应该使用过时的Vector、Hashtable和Stack。


