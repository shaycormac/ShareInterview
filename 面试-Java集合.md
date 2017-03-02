

######1、列举java的集合和继承关系
![](http://upload-images.jianshu.io/upload_images/1479978-eba64be1ecc7d6f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######2、Collection 和 Collections的区别。
* Collection是集合类的上级接口，继承与他的接口主要有Set 和List.
* Collections是针对集合类的一个帮助类，他提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。

######3、Java集合框架的优点？
* 使用核心集合类降低开发成本，而非实现我们自己的集合类。
* 随着使用经过严格测试的集合框架类，代码质量会得到提高。
* 通过使用JDK附带的集合类，可以降低代码维护成本。
* 具有更强的复用性和可操作性。

######4、Collection和Map的区别？
二者没有必然联系；
* Collection存储的是一组对象；
* Map是以“键值对”的形式对对象进行的管理。

######5、List和Set的
* List：元素有序可重复，查找元素效率高，插入删除效率低，因为会引起其他元素位置改变，实现类有ArrayList、LinkedList、Vector。List和数组类似，可以动态增长，根据实际存储的数据的长度自动增长List的长度。
* Set：元素无序不可重复，检索效率低下，删除和插入效率高，插入和删除不会引起元素位置改变 ，实现类有HashSet、TreeSet。

######6、List的实现类有哪些？各自有什么特点？
![](http://upload-images.jianshu.io/upload_images/1479978-7e7b418b25831e32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* ArrayList：轻量级，运行快，查询快
* LinkedList：增删操作快
* LinkedList比ArrayList消耗更多的内存，因为LinkedList中的每个节点存储了前后节点的引用

######7、ArrayList和Vector有何异同点？
* ArrayList和Vector在很多时候都很类似。
  * 两者都是基于索引的，内部由一个数组支持。
  * 两者维护插入的顺序，我们可以根据插入顺序来获取元素。
  * ArrayList和Vector的迭代器实现都是fail-fast的。
  * ArrayList和Vector两者允许null值，也可以使用索引值对元素进行随机访问。
* 以下是ArrayList和Vector的不同点。
  * Vector是同步的，而ArrayList不是。然而，如果你寻求在迭代的时候对列表进行改变，你应该使用CopyOnWriteArrayList。
  * ArrayList比Vector快，它因为有同步，不会过载。
  * ArrayList更加通用，因为我们可以使用Collections工具类轻易地获取同步列表和只读列表。

######7、Map的实现类？
* HashMap 轻量级 线程不安全的,允许key或value为null JDK1.2
* HashTable 重量级 线程安全的 不允许key或value为null   JDK1.0

######8、List、Map、Set三个接口，存取元素时，各有什么特点？
* List 以特定次序来持有元素，可有重复元素。
* Set 无法拥有重复元素,内部排序。
* Map 保存key-value值，value可多值。

######9、HashMap和 HashTable 的区别
* HashMap是非线程安全的，HashTable是线程安全的。
* HashMap的键和值都允许有null值存在，而HashTable则不行。
* 因为线程安全的问题，HashMap效率比HashTable的要高。
![](http://upload-images.jianshu.io/upload_images/1479978-f1c65c0831989926.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######10、Iterater和ListIterator之间有什么区别？
* 我们可以使用Iterator来遍历Set和List集合，而ListIterator只能遍历List
* Iterator只可以向前遍历，而LIstIterator可以双向遍历。
* ListIterator从Iterator接口继承，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置。

######11、Set里的元素是不能重复的，那么用什么方法来区分重复与否呢? 是用==还是equals()? 它们有何区别?
* 使用equals()区分。
* ==是用来判断两者是否是同一对象（同一事物），而equals是用来判断是否引用同一个对象。
* set里面存放的是对象的引用，所以当两个元素只要满足了equals()时就已经指向同一个对象，也就出现了重复元素。所以应该用equals()来判断。

######12、集合和数组的区别
* 数组在定义时必须定义长度，而集合在定义时不必定义长度。
* 数组在定义时必须要声明数组的数据类型，而集合不必。但是一般情况下我们都是存储统一数据类型的数据，我们可以使用泛型的写法来限制集合里面的数据类型。

![](http://upload-images.jianshu.io/upload_images/1479978-54a924a99631f1b8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)