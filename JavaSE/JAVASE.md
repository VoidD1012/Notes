## JAVA基础

1. 方法重载：方法名相同，参数项不同，与返回值、返回修饰符无关
2. this关键字：代表当前对象的一个引用
3. static：静态变量，静态方法，一般将工具类中的方法定义为static
4. 静态初始化代码块：类第一次被载入时先执行static代码块；类多次载入时，static代码块只执行一次，static经常用来进行static变量的初始化。

```java
public class StaticTest {
    static {
    	System.out.println("此处可以执行类的初始化工作");
	}
}
```

5. JDK中常用的包：
   1. lang：不需要手动导入，自动加载
   2. util：工具包
   3. net：网络包
   4. io：输入输出流包
6. 静态导入：用于导入指定类的静态属性

```java
import static java.lang.Math.*;
```



## Java三大特征

1. Java三大特征——**封装**encapsulation

   * 将类的某些信息隐藏在类的内部，不允许外部程序直接访问，而是通过该类提供的方法来实现对隐藏信息的操作和访问。
   * 面向对象的封装（狭义上的封装）：将类中的属性设置为私有属性，提供共有的外部方法共程序进行调用，可以实现丰富的细节操作。保证了数据安全和规范。隐藏了对象内部的复杂性，便于外界调用，从而提高系统的可扩展性，可维护性。
   * 广义的封装：可以将完成特定功能的代码块封装成一个方法，供不同的程序进行调用

2. Java方法调用中的参数传递是值传递

3. Java三大特征——**继承**

   1. Java是单继承关系

      * 多继承是为了实现代码的复用性，却引入了复杂性，使得系统类之间的关系混乱。

   2. super是直接父类的引用，可以访问父类中被子类覆盖的方法或属性。

   3. 重写：必须存在于继承关系中

      * 重写方法必须和被重写方法具有相同方法名称、参数列表和返回类型。
      * 重写方法不能使用比被重写方法更严格的访问权限。

   4. ==：比较的是基本类型变量的值以及引用类型的值也就是内存地址。

      equals()：比较了两个对象的内容，在Object类中的父类equals方法使用的是==的比较方式，所以使用时需要在子类中重载方法。

4. Java三大特征——**多态**：同一个引用类型，使用不同的实例而执行不同操作

   1. 多态的三个要素：
      * 必须要有继承关系
      * 子类方法必须重写父类的方法
      * 父类引用指向子类对象
   2. 实现多态的两种形式：
      * 使用父类作为方法形参
      * 使用父类作为方法的返回值
   3. instanceof运算符：【对象 instanceof 类或接口】通常与强制类型转换结合使用

5. 接口interface 

   1. 接口中的变量和方法的访问修饰符都是默认的public，无法修改
   2. 接口中的方法默认是abstract的
   3. 接口是实现，子类是继承
   4. 接口中的变量都是**全局静态常量**，即使没有显式的使用static final进行修饰
   5. 接口表现是一种能力：子类在实现接口时，必须实现接口中的所有方法

6. 面向接口编程

   1. 在程序设计时，只关心实现类有何能力（方法），而不关心实现细节
   2. 面向接口的约定而不考虑接口的具体实现

7. 抽象类和接口的区别

   1. 抽象类中的方法可以有抽象方法，也可以有普通方法，但接口中只能包含抽象方法
   2. 抽象类需要使用abstract关键字进行修饰，而接口使用interface关键字
   3. 子类使用extends关键字来继承抽象类，使用implements来实现接口
   4. 子类继承抽象类的时候必须要实现所有的抽象方法，普通方法可以不重写，而接口中的所有方法都必须实现。
   5. 抽象类中可以定义成员变量，而接口中只能定义静态常量
   6. 抽象类在子类实现的时候是单继承，而接口是多继承
   7. 抽象类和接口都不能实例化，但是抽象类中可以有构造方法，而接口中不能有构造方法。
   8. 抽象类可以实现接口，而且不实现接口中的方法，而接口只能继承接口，不能实现接口。

8. 内部类

   1. 创建内部类的时候需要在内部类的前面添加外部类来进行修饰

      ```java
      OuterClass.InnerClass inner = new OuterClass.new InnerClass();
      ```

   2. 内部类中不能定义静态属性

   3. 如果外部类和内部类具有相同的成员变量或方法，内部类默认访问自己的成员变量或方法，如果要访问外部类的成员变量，需要使用【外部类.this关键字】。

   4. 几种内部类：

      1. 匿名内部类：当定义一个类，需要实现某个接口，且该接口使用过程中只需要使用一次时，可以不创建具体的类，而采用【new interface(){添加未实现的方法}】的方式来实现，叫做匿名内部类。

      2. 静态内部类：使用static修饰内部类

         【外部类类名.内部类 内部类对象名 = new 外部类类名.内部类类名();】

      3. 方法内部类：将内部类定义在外部类的方法内部



## 异常处理

1. 异常处理的方式

   1. 捕获异常：try-catch

      * 当设定的异常不满足会产生的异常时，程序会异常退出，不会执行try-catch块后的程序。
      * 使用catch捕获异常后，程序可以正常执行try-catch块后的内容。

   2. 声明异常：throws 异常类

      * 在方法调用中，可能存在多个方法之间的调用，加入每个方法中都包含了异常，可以采用在方法的最外层调用中使用throws的方式，对所有执行过程中的所有方法出现的异常进行统一处理。

   3. 抛出异常：throw 异常对象

      * 和throws配合使用

   4. 捕获异常的一些问题：

      - 情况一：try中有return，finally中没有return

      ```java
      public class TryTest {
          public static void main(String[] args) {
              System.out.println(test());
          }
      
          private static int test() {
              int num = 10;
              try {
                  System.out.println("try");
                  return num += 80;
              } catch (Exception e) {
                  System.out.println("error");
              } finally {
                  if(num > 20) {
                      System.out.println("num>20 : " + num);
                  }
                  System.out.println("finally");
              }
              return num;
          }
      }
      ```

      ![](picture\try-catch1.png)

      **分析**：“return num+=80"被拆分成了”num = num+80"和“return num”两个语句，先执行num的赋值语句，将其保存起来，在try中的“return num”执行前，先将finally中的语句执行完，而后再将90返回。

      - 情况二：try和finally中均有return

      ```java
      public class TryTest {
          public static void main(String[] args) {
              System.out.println(test());
          }
      
          private static int test() {
              int num = 10;
              try {
                  System.out.println("try");
                  return num += 80;
              } catch (Exception e) {
                  System.out.println("error");
              } finally {
                  if(num > 20) {
                      System.out.println("num>20 : " + num);
                  }
                  System.out.println("finally");
                  num = 100;
                  return num;
              }
          }
      }
      ```

      ![try-catch2](picture\try-catch2.png)

      - 情况三：finally中改变返回值num

      ```java
      public class TryTest {
          public static void main(String[] args) {
              System.out.println(test());
          }
      
          private static int test() {
              int num = 10;
              try {
                  System.out.println("try");
                  return num;
              } catch (Exception e) {
                  System.out.println("error");
              } finally {
                  if(num > 20) {
                      System.out.println("num>20 : " + num);
                  }
                  System.out.println("finally");
                  num = 100;
      
              }
              return num;
          }
      }
      ```

      ![try-catch3](picture\try-catch3.png)

      - 情况四：将num的值包装在Num类中

   ```java
   public class TryTest {
       public static void main(String[] args) {
           System.out.println(test().num);
       }
   
       private static Num test() {
           Num number = new Num();
           try {
               System.out.println("try");
               return number;
           } catch (Exception e) {
               System.out.println("error");
           } finally {
               if(number.num > 20) {
                   System.out.println("number.num>20 : " + number.num);
               }
               System.out.println("finally");
               number.num = 100;
   
           }
           return number;
       }
   }
   
   class Num {
       public int num = 10;
   }
   ```

   ![try-catch4](picture\try-catch4.png)

   **结论**：try语句在返回前，将其他所有的操作执行完，保留好要返回的值，而后转入执行finally中的语句。

2. finally唯一不执行的情况：除非在try块或者catch块中调用了退出虚拟机的方法（即System.exit(1);），否则不管在try块、catch块中执行怎样的代码，出现怎样的情况，异常处理的finally块总是会被执行的。

   ​     当程序执行try块，catch块时遇到return语句或者throw语句，这两个语句都会导致该方法立即结束，所以系统并不会立即执行这两个语句，而是 去寻找该异常处理流程中的finally块，如果没有finally块，程序立即执行return语句或者throw语句，方法终止。

## Java常用类

1. 包装类是将基本数据类型封装成一个类，包含属性和方法

   1. 装箱：基本数据类型转换为包装类

      * 自动装箱：基本类型就自动地封装到与它相同类型的包装中。如：Integer i = 100

        本质上是编译器编译时自动添加了：Integer i = Integer.valueOf(100)

   2. 拆箱：包装类转换为基本数据类型

      * 自动拆箱：包装类对象自动转换成基本类型数据。如：int a = new Integer(100);

        本质上是编译器编译时自动添加了：int a = new Integer(100).intValue();

2. 字符串

   1. 常量池在jdk1.7之后放置在堆空间中，字符串常量被放置在常量池中
      * intern()方法可以获得String对象的字符串常量在常量池中的地址
      * 在常量池中，值相同的常量只会存在一个
   2. 字符串的本质：字符数组或者字符序列
   3. StringBuffer和StringBuilder
      * StringBuffer线程安全，效率低；StringBuilder线程不安全，效率高

3. 时间处理相关类（util.Date、Calendar、sql.Date）

   1. 使用DateFormat的子类SimpleDateFormat可以将Date类按照一定规范转换为需要的时间字符串表达形式，也可以将字符串转换为Date类。
   2. Calendar类：
      * 不能使用new的方式创建对象
      * 使用getInstance()方式创建Calendar对象，是根据当前系统时间创建的
      * 使用setTime(Date类)方法，传入Date类型的参数，可以得到指定时间的日历类

4. 枚举类enum：由一组固定的常量组成的类型

   ```java
   public enum Gender {
       男, 女
   }
   ```

   ```java
   public enum EventEnum {
       //枚举实质上还是类
       //而每个被枚举的成员，实质上就是一个枚举类型的实例，所以也同样需要构造方法
       //成员默认都是public static final的，可以直接通过枚举类型名直接使用它们。
       LAUNCH("launch"), PAGEVIEW("pageview"), EVENT("event");
       
       EventEnum(string name) {
           this.name = name;
       }
       private String name;
       
       public void show() {
           System.out.println(this.name);
           //使用values可以将当前枚举类生成一个数组
           EventEnum[] ee = values();
           for(int i = 0; i < ee.length; i++) {
               System.out.println(ee[i]);
           }
       }
   }
   ```

   ```java
   public class Test {
       Gender man = Gender.男;
       Gender woman = Gender.女;
       
       public static void main(String[] args) {
           EventEnum ee = EventEnum.LAUNCH;
           ee.show();
           String e = EventEnum.PAGEVIEW.name();
       }
   }
   ```



## Java集合框架

![image-20200502003038600](picture\Java框架1.png)

1. Collection

   - 可以存放不同类型的数据，而数组只能存放固定类型的数据
   - 使用ArrayList子类实现的时候，初始化的长度是10，长度不够的时候会自动进行扩容。
   - Collection接口存储一组不唯一，无序的对象
     - List接口存储一组不唯一、有序（插入顺序）的对象
     - Set接口存储一组唯一、无序的对象
     - Map接口存储一组键值对，提供key到value的映射

2. List

   1. ArrayList实现了长度可变的数组，在内存中分配连续的空间。

   2. LinkedList采用链表存储方式。

   3. ArrayList和LinkedList的联系和区别：

      - ArrayList实现了长度可变的数组，在内存中分配连续空间。遍历元素和随机访问元素的效率比较高。
      - LinkedList采用链表存储方式。插入、删除元素的效率比较高。

   4. Vector和ArrayList的联系和区别

      - Vector也是List接口的一个子类实现，和ArrayList的实现原理相同，功能相同，都是长度可变的数组结构，很多时候可以互用。

      - 区别：ArrayList是线程非安全的，重速度轻安全，Vector是线程安全的。
      - ArrayList在扩容的时候默认原长度的1.5倍，Vector则是原来的2倍

3. Iterator迭代器

   - 所有的集合类都默认实现了Iterable的接口，实现了该接口意味着具备了使用foreach循环方式的能力。增强for循环本质上使用的也是iterator的功能。

4. Set接口

   1. Set接口存储一组唯一、无序的对象
   2. 操作数据方法与List类似，但不存在get()方法。
   3. HashSet：采用Hashtable哈希表存储结构
      - 添加速度快，查询速度快，删除速度快，但是无序
   4. LinkedHashSet：采用哈希表存储结构，同时使用链表维护次序，为HashSet补充了顺序。
   5. TreeSet：采用二叉树（红黑树）的存储结构
      - 有序（排序后的升序）查询速度比List快，但是没有HashSet快。
      - 树结构中的元素要求是同一数据类型的。
      - 添加的元素时自定义对象时，会查找对象的equals和hashcode方法，需要重写这两个方法以供进行元素之间的比较，才能按顺序存储在TreeSet中。
      - 树中的元素时默认进行排序操作的，基本数据类型自动比较，引用类型需要自定义比较器
   6. **比较器**：
      * **内部比较器**：定义在元素的类中，通过实现**Comparable接口**来进行实现。
        * 实现了Comparable接口的类通过实现compareTo方法从而确定该类对象的排序方式。
      * **外部比较器**：定义在当前类中，实现**Comparator<E> 接口**并且要在初始化类时传递到其中。
        * 重写Comparator中的compare()方法设置排序方式。
      * 注意：外部比较器可以定义成一个工具类，此时所有需要比较的规则如果一致的话可以复用，而内部比较器只有在存储当前对象的时候才能使用。
      * 两者同时存在时，外部比较器优先。

5. 泛型

   1. 基本使用：在定义对象的时候，通过泛型规定集合中的元素为同一类型。
   2. 泛型类：在定义类的时候在类名后面添加<E，K，V，A，B>起到占位的作用，类中的方法的返回值类型和属性的类型都可以使用泛型。
   3. 泛型接口：在定义接口的时候在接口名后面添加泛型<>进行占位
      - 子类在进行实现的时候，可以不填写泛型的类型，在创建具体的子类对象的时候再决定泛型的类型。
      - 子类在实现泛型接口的时候，在在实现父类接口的时候指定父类泛型的类型，此时调用其子类方法中的泛型类型必须要跟子类一致。
   4. 泛型方法

6. Map：存储key-value映射的数据

   1. 1. HashMap：数组+链表（jdk1.7） 数组+链表+红黑树（jdk1.8以后）

         - key无序且唯一（Set），value无序，不唯一（Colletion）

         - jdk1.7版源码知识点

           - HashMap初始化的时候没有分配内存空间，而是在向其中添加值的时候再分配内存。
           - HashMap的初始容量默认是16，在自定义初始容量时，程序会将该容量转换为2的n次幂。为什么要设置成2的n次幂：
             - 方便进行&运算，提高效率，应用在indexFor()方法中。
             - 在扩容之后涉及到元素的迁移过程，迁移的时候只需要判断二进制的前一位时0或者1即可，如果是0，表示新数组和旧数组的下标位置不变，如果是1，只需要将索引位置加上旧的数组的长度值即为新数组的下标。

         ```Java
         public V put(K key, V value) {
             if(table == EMPTY_TABLE) {
                 inflateTable(threshold);
             }
             if(key == null) {
                 return putForNullKey(value);
             }
             int hash = hash(key);
             //indexFor()方法：return hash & (initCapacity-1) 
             //其中，initCapacity初始容量设定为2的n次幂，就是为了方便进行&运算，&运算比取模运算的效率要高。同时也能更高效地获取Entry对象数组的索引位置。
             int i = indexFor(hash, table.length);
             for(Entry<K, V> e = table[i]; e != null; e = e.next) {
                 Object k;
                 //为了判断hash值，key和value的值是否和已经存储的对象的值一致
                 if(e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                     V oldValue = e.value;
                     e.value = value;
                     e.recordAccess(this);
                     return oldValue;
                 }
             }
             modCount++;
             //添加Entry，并在容量到达threshold的时候进行扩容，扩容大小默认2倍
             addEntry(hash, key, value, i);
             return null;
         }
         ```

         - jdk1.8以后源码知识点
           - 新加入TREEIFY_THRESHOLD=8，当链表插入的元素超过8，就将对应链表转换为红黑树。
           - 新加入UNTREEIFY_THRESHOLD=6
           - 将原来的Entry替换成了Node
           
         - HashMap和Hashtable的联系和区别：

           - 联系：实现原理相同，功能相同，底层都是哈希表结构，查询速度快，很多情况下可以互用。

           - 区别：

             1) Hashtable是早期的JDK提供的接口，HashMap是新版的JDK提供的接口。

             2) Hashtable继承Dictionary类，HashMap实现Map接口。

             3) Hashtable是线程安全的，HashMap线程非安全。

             4) Hashtable不允许null值，HashMap中key和value都允许null值

      2. LinkedHashMap：链表

         - 有序的HashMap速度快

      3. TreeMap：基于红黑树的实现

         - 有序，速度没有hash快

      4. Set和Map采用了相同的数据结构，而Set只用map的key存储数据

      5. 基本api操作：

      * 增加：
        * put(k, v) ：添加元素

      * 查找：

        * isEmpty()：判断是否为空
        * size()：返回map的大小
        * containsKey()、containsValue()：判断key、value是否存在
        * get(key)：通过key获取对应value

      * 删除：

        * clear()：清空所有元素
        * remove()：删除指定元素

      * 遍历：

        * keySet()：返回所有key的Set集合。
        * values()：返回value值的集合，但是只能获取对应的value值，不能根据value来获取key。
        * 遍历操作可以通过key或value值进行遍历，map没有迭代器接口，不能直接使用迭代器遍历。

      * map.entry：表示的是键值对的一组映射关系，key和value以键值对的形式成组出现，上面的情况都是单独取出key或者value的。可以通过entrySet()方法获取。


   ```java
   //使用entry的迭代器遍历
   Set<Map.Entry<String, Integer>> entries = map.entrySet();
   Iterator<Map.Entry<String, Integer>> iterator1 = entries.iterator();
   while(iterator1.hasNext()) {
       Map.Entry<String, Integer> next = iterator1.next();
       System.out.println(next.getKey() + "-----" + next.getValue());
   }
   ```

7. Collections工具类

   1. 与Collection的区别：
      - Collection是Java提供的集合接口，存储一组不唯一，无序的对象。它有两个子接口List和Set。
      - Collections类专门用来操作集合类，它提供了一系列的**静态方法**实现对各种集合的搜索、排序、线程安全化等操作。
   2. 提供的常用静态方法：
      - addAll()：批量添加元素。
      - sort()：排序。如果有需要可以new一个Comparator<>设定比较器
      - binarySearch()：二分查找，在查找前必须排好序。
      - fill()：替换元素
      - shuffle()：随机排序
      - reverse()：逆序排序

8. Arrays工具类：提供了数组操作的工具类，最主要是提供了集合和数组之间的转换方法。

   1. 数组转换为list：asList()

   ```java
   int[] array = new int[]{1, 2, 3, 4, 5};
   List<int[]> ints1 = Arrays.asList(array);
   
   List<Integer> ints2 = Arrays.asList(1, 2, 3, 4, 5);
   ```

   2. list转换为数组：toArray()

9. 集合总结：

   |     名称      | 存储结构 |      顺序       | 唯一性  |    查询效率    | 添加/删除效率 |
   | :-----------: | :------: | :-------------: | :-----: | :------------: | :-----------: |
   |   ArrayList   |  顺序表  |  有序（添加）   | 不唯一  | 索引查询效率高 |      低       |
   |  LinkedList   |   链表   |  有序（添加）   | 不唯一  |       低       |     最高      |
   |    HashSet    |  哈希表  |      无序       |  唯一   |      最高      |     最高      |
   |    HashMap    |  哈希表  |     Key无序     | Key唯一 |      最高      |     最高      |
   | LinkedHashSet |  哈+链   |  有序（添加）   |  唯一   |      最高      |     最高      |
   | LinkedHashMap |  哈+链   | Key有序（添加） | Key唯一 |      最高      |     最高      |
   |    TreeSet    |  二叉树  |  有序（升序）   |  唯一   |      中等      |     中等      |
   |    TreeMap    |  二叉树  |  有序（升序）   | Key唯一 |      中等      |     中等      |

   

   | 特性          | Collection    | Collection | Map           | Map     |
   | :------------ | ------------- | ---------- | ------------- | ------- |
   | 无序   不唯一 | Collection    |            | Map.values()  |         |
   | 有序   不唯一 | Arraylist     | LinkedList |               |         |
   | 无序   唯一   | HashSet       |            | HashMap       |         |
   |               |               |            | KeySet        |         |
   | 有序   唯一   | LinkedHashSet | TreeSet    | LinkedHashMap | TreeMap |
   |               |               |            | KeySet        |         |

   

10. 集合和数组的比较

    数组不是面向对象的，存在明显的缺陷，集合弥补了数组的一些缺点，比数组更灵活更实用，可大大提高软件的开发效率，而且不同的集合框架类可使用不同场合。具体如下：

    1) 数组能存放基本数据类型和对象，而集合类中只能存放对象。

    2) 数组容易固定，且无法动态改变；集合类容量可以动态改变。

    3) 数组无法判断其中实际存有多少元素，length只告诉了数组的容量；而集合的size()可以确切知道元素的个数。

    4) 集合有多种实现方式和不同适用场合，不像数组仅采用顺序表的方式。

    5) 集合以类的形式存在，具有封装、继承、多态等类的特性，通过简单的方法和属性即可实现各种复杂操作，大大提高了软件的开发效率。

11. 



## IO流

1. 文件类File

   1. File类提供了对当前文件系统中文件的部分操作。
   2. 需要注意的方法：
      - createNewFile()：会根据File对象的路径在内存中创建文件
      - getParent()：获取文件的父路径名称。同样的，该父路径名称来源于创建File对象时提供的路径参数，如果文件的路径中只包含文件名称，则父路径显示为空。
      - getAbsolutePath()：获取文件的绝对路径。
      - getCanonicalPath()：返回文件绝对路径的规范形式。主要是由于文件分隔符在不同的操作系统上有不同的表示。
      - File.separator：返回操作系统的文件分隔符（静态常量）
      - isFile()：判断File对象是否是文件。该操作会在内存中寻找该文件是否实际存在来进行判断。
      - listFiles()：返回File对象（路径为目录）中的文件和目录，返回值是File数组。
      - mkdir()：创建单级目录。
      - mkdirs()：创建多级目录。
   3. 如果文件在遍历的时候会出现空指针的问题，且程序没有错误，那么原因在于当前文件系统受保护，某些文件没有访问权限，此时会报空指针异常。

2. 流——是指一连串流动的字符，是以先进先出的方式发送信息的通道

   1. 在Java中需要读写文件中的数据的话，需要使用流的概念。流表示一个文件将数据返送到另一个文件，包含一个流向的问题，此时就需要选择一个参照物，规定将当前程序作为参照物。
   2. 从一个文件中读取数据到当前程序叫做输入流；从当前程序输出数据到另一个文件叫做输出流。
   3. 按照流向划分
      - 输入流：InputStream和Reader作为基类。
      - 输出流：OutputStream和Writer作为基类。
      - 输入输出流是相对于计算机内存来说的，而不是相对于源和目标。
   4. 按照处理数据单元划分
      - 字节流：是8位通用字节流
        - 字节输入流：InputStream基类
        - 字节输出流：OutputStream基类
      - 字符流：是16位Unicode字符流
        - 字符输入流Reader基类
        - 字符输出流Writer基类
   5. 按照功能分类：
      - 节点流：可以直接从数据源或目的地读写数据。
      - 处理流（包装流）：不直接连接到数据源或目的地，是对其它流进行封装。目的主要是简化操作和提高性能。
      - 节点流和处理流的关系：节点流处于io操作的第一线，所有操作必须通过他们进行；处理流可以对其它流进行处理。
   6. 编写io流程序步骤
      1. 选择合适的io流对象
      2. 创建对象
      3. 传输数据
      4. 关闭流对象（占用系统资源）
   7. 通过inputStream流的子类读数据

   ```java
   //1.单次读取一个字节的数据
   InputStream is = new FileInputStream("abc.txt");
   int word = 0;
   while((word = is.read()) != -1) {
       System.out.println((char)length);
   }
   ```

   ```java
   //2.采用添加缓冲区的方式进行读取，每次会将数据添加到缓冲区中，当缓冲区满了之后，一次性读取，而不是每一个字节进行读取
   InputStream is = new FileInputStream("abc.txt");
   int length = 0;
   byte[] buffer = new byte[1024];
   //缓冲区每次读取的最大字节数是1024，如果一次读取超过最大字节数，将清空当前缓冲区，用剩余字节再次填充。
   while((length = is.read(buffer)) != -1) {
       System.out.print(new String(buffer, 0, length));
   }
   ```

   ```java
   //3.设定缓冲区读取的长度
   while((length = is.read(buffer, 5, 20)) != -1) {
       System.out.println(new String(buffer, 0, length));
   }
   ```

   8. 通过OutputStream的子类写数据
      - flush()：刷新输出流并强制缓冲中的输出字节被写出。
        - 最保险的使用方式：在输出流关闭前都flush()一下，然后再关闭。
        - 一般是当某一个输出流对象中带有缓冲区的时候，就需要进行flush()。

3. IO流的子类总结——详情见IO流的xmind导图

   ![](picture\IO流的分类.png)

4. 字符流可以直接读取中文汉字，而字节流在处理的时候会出现中文乱码。

5. 在处理图片、视频等文件时，需要使用字节流。

6. 如果要将对象通过io流进行传输，那么必须要实现序列化接口（Serializable），在该对象类中必须声明一个serialVersionUID的属性，属性的值是多少无所谓，但是要有。也可以通过idea中的设置进行自动生成。

   - transient：使用此关键字修饰的变量，在进行序列化的时候不会被序列化。

7. RandomAccessFile类

   - RandomAccessFile既可以读取文件内容，也可以向文件输出数据。同时，RandomAccessFile支持“随机访问”的方式，程序块可以直接跳转到文件的任意地方来读写数据。
   
8. apache.commonsio工具包（需要下载）

   - 提供了很多操作io流相关的方法。

## 多线程

#### 1. 线程基础内容

1. 程序：Program，是一个指令的集合。
2. 进程：Process，正在执行中的程序，是一个静态概念。
   - 进程是程序的一次静态执行过程，占用特定的地址空间。
   - 每个进程都是独立的，由CPU，data，code三部分组成。
   - 缺点：浪费内存，cpu的负担
3. 线程：是进程执行中的一个“单一的连续控制流程”（a single Thread, equential flow of control)/执行路径。
   - 线程又被称为轻量级进程（lightweight process）
   - 一个进程可拥有多个并行（concurrent）的线程。
   - 一个进程中的线程共享相同的内存单元/内存地址空间，意味着，它们可以访问相同的变量和对象，而且它们从同一堆中分配对象，所以需要通信、数据交换、同步操作等。
   - 由于线程间的通信是在同一地址空间上进行的，所以不需要额外的通信机制，这就使得通信更简便，而且信息传递的速度也更快。
4. 实现多线程的时候：
   1. 方法一：继承Thread类，调用其start()方法。
   2. 方法二：实现Runnable接口，重写run()方法，创建参数为实现Runnable接口的对象的Thread对象，调用其start()方法。这种方式避免了单继承，方便共享资源，同一份资源多个代理访问
      - 这种实现方式使用了设计模式中的代理模式。
   3. 必须要重写run方法，run方法中是核心的执行逻辑
   4. 线程在启动的时候，要通过start()方法进行调用。直接调用run()方法的效果是将run方法当作普通方法来执行，而不是线程方法。
   5. 每次运行相同的代码，线程执行的顺序不一定一致，原因是多线程之间谁先抢占资源无法进行人为控制。
5. 线程的状态（5种）：创建、就绪状态、运行状态、阻塞状态、终止
6. 线程的声明周期：
   1. 新生状态：创建好当前线程对象，没有启动之前（调用start方法前）
   2. 就绪状态：当对应的线程创建完成，并调用start()方法后，所有的线程会添加到一个就绪队列中，所有的线程准备同时去抢占CPU的资源。
   3. 运行状态：当前进程获取到cpu资源后，就绪队列中的所有线程会去抢占cpu的抢占到资源的线程限制性，在执行的过程中就叫运行状态。
   4. 死亡状态：运行中的线程正常执行完所有的代码逻辑或者因为异常情况导致程序结束。
      - 进入死亡状态的方式：
        - 正常运行完成且结束
        - 线程被强制性终止，如stop()方法（已过时，不推荐使用的终止方式）
        - 线程抛出未捕获的异常。
   5. 阻塞状态：程序运行过程中，发生某些异常情况，导致当前线程无法再顺利执行下去，此时会进入阻塞状态。进入阻塞状态的原因消除后，所有的阻塞队列再次进入到就绪状态中，再次等待执行。
      - 进入阻塞状态的方式：
        - sleep()方法
        - 等待io资源
        - join()方法
7. 线程操作的相关方法：
   1. currentThread()：Thread类的静态方法，返回当前正在执行的线程对象
   2. join()：调用该方法的线程将被强制执行，**令其它线程处于阻塞状态**，当该线程执行完毕后，其它线程再执行。
   3. sleep(long millis)：使当前正在执行的线程休眠millis秒，**该线程处于阻塞状态**。
   4. yield()：当前正在执行的线程暂停一次，允许其它线程执行，而当前线程不进入阻塞状态，**进入就绪状态**；如果没有其它等待执行的线程，那么当前线程就会恢复执行。
8. 在使用多线程的时候可以实现唤醒（notify()方法）和等待（wait()方法）的过程，但是唤醒和等待操作的对象不是thread，而是我们设置的共享对象或者变量，即线程调用的资源。

#### 2. 线程同步

1. 多线程的安全性问题——多站点销售火车票的案例

2. 使用同步解决多线程的安全性问题——synchronized

   - 同步代码块

   ```java
   public class TicketRunnable implements Runnable {
       private int ticket = 5;
       @Override
       public void run() {
           for (int i = 0; i < 100; i++) {
               try {
                   Thread.sleep(200);
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
   //synchronized(共享资源所在的对象，必须是Object的子类){具体执行的代码块}
               synchronized(this) {
                   if(ticket > 0)
                       System.out.println(Thread.currentThread().getName() +
                           "正在售出第" + (ticket--) + "张票");
               }
           }
       }
       public static void main(String[] args) {
           TicketRunnable ticket = new TicketRunnable();
           Thread t1 = new Thread(ticket);
           Thread t2 = new Thread(ticket);
           Thread t3 = new Thread(ticket);
           Thread t4 = new Thread(ticket);
           t1.start();
           t2.start();
           t3.start();
           t4.start();
       }
   }
   ```

   

   - 同步方法

   ```java
   public class TicketRunnable implements Runnable {
   
       private int ticket = 5;
   
       @Override
       public void run() {
           for (int i = 0; i < 100; i++) {
               try {
                   Thread.sleep(200);
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
               sale();
           }
       }
   //将核心的代码逻辑定义成一个方法，使用synchronized关键字进行修饰，此时不需要指定共享对象，因为方法已经定义在需要使用的类中，默认由对象进行调用
       public synchronized void sale() {
           if(ticket > 0)
               System.out.println(Thread.currentThread().getName() +
                                  "正在售出第" + (ticket--) + "张票");
       }
   
       public static void main(String[] args) {
           //与同步代码块执行方式相似
       }
   }
   ```

3. 同步监视器（即被共享的资源对象）

   - synchronized(obj){}中的obj对象称为同步监视器。
   - 同步代码块中同步监视器可以是任何对象，但是推荐使用共享资源作为同步监视器。
   - 同步方法中无需指定同步监视器，因为同步方法的监视器是this，也就是该对象本身。

4. 死锁

   - 同步可以保证资源共享操作的正确性，但是也会导致死锁的产生

5. 生产者与消费者

   - 多线程访问的时候出现了数据安全的问题
     - 生产者未生产的产品被消费者取走
     - 商品的品牌和名称不对应
   - 使用了wait()方法，在产品数量发生短缺的时候，阻塞正在使用cpu资源的对象的线程，更换为生产者或者消费者来修正产品数量。
   - 对应地，需要使用notify()过程来唤醒阻塞状态的线程，否则会陷入死锁。
   - wait()和notify()方法都是对共享资源的锁的处理方法。

6. JUC解决在容器中存储资源的多线程的问题

#### 3. Java线程池

1. 为什么需要线程池
   - 在实际使用中，线程是很占用系统资源的，如果对线程管理不善很容易导致系统问题。因此，在大多数并发框架中都会使用线程池来管理线程。
   - 使用线程池管理线程的好处：
     - 使用线程池可以重复利用已有的线程继续执行任务，避免线程在创建和销毁时造成的消耗。
     - 由于没有线程创建和销毁时的消耗，可以提高系统的响应速度。
     - 通过线程池可以对线程进行合理的管理，根据系统的承受能力调整可运行线程数量的大小等。
   
2. 线程池执行所提交任务的过程
   1. 先判断线程池中核心线程池所有的线程是否都在执行任务。 如果不是，则新创建一个线程执行刚提交的任务，否则，说明核心线程池中所有的线程都在执行任务，进入第2步
   2. 判断当前阻塞队列是否已满，如果未满，则将提交的任务放置在阻塞队列中；否则，进入第3步； 
   3. 判断线程池中所有的线程是否都在执行任务，如果没有，则创建一个新的线程来执行任务，否则，交给饱和策略进行处理 
   
3. 线程池的分类（封装好特定属性的线程池子类）
   - ThreadPoolExecutor
     - newCachedThreadPool
     - newFixedThreadPool
     - newSingleThreadExecutor
   - ScheduledThreadPoolExecutor
     - newSingleThreadScheduledExecutor
     - newScheduledThreadPool
   - ForkJoinPool
     - newWorkStealingPool
   
4. 线程池的生命周期

   1. RUNNING状态：能接受新提交的任务，并且也能处理阻塞队列中的任务。 
   2. TERMINATED状态：在terminated()方法执行完后进入该状态，默认terminated()方法中什么也没有做
   3. 三种中间状态：
      - 调用shutdown()的时候会进入到SHUTDOWN状态
        - 关闭状态，不再接受新提交的任务，但却可以继续处理阻塞队列中已保存的任务。
      - 调用shutdownNow()的时候会进入STOP状态
        - 不能接受新任务，也不处理队列中的任务，会中断正在处理任务的线程。
      - 在SHUTDOWN或STOP状态下，当工作线程的数量为0的时候进入TIDYING状态
        - 如果所有的任务都已终止了，workerCount (有效线程数) 为0，线程池进入TIDYING状态后会调用 terminated()方法进入TERMINATED状态

5. 线程池的创建（源码）：可以直接调用这个创建方法生成更实用的线程池。

   ```java
   public ThreadPoolExecutor(//核心线程数的最大值，线程池新建线程的时候，如果当前线程总数小于corePoolSize，则新建的是核心线程，如果超过corePoolSize，则新建的是非核心线程。核心线程默认情况下会一直存活在线程池中，即使这个核心线程啥也不干(闲置状态)。
       					  int corePoolSize,	
                             //线程池的最大容量= 核心线程数 + 非核心线程数
                             int maximumPoolSize,
       					  //线程池中非核心线程闲置超时时长，超时会被销毁
                             long keepAliveTime,
   						  //TimeUnit是一个枚举类型，用作keepAliveTime的单位
                             TimeUnit unit,
       					  //该线程池中的任务队列，维护着等待执行（阻塞）的Runnable对象。当所有的核心线程都在干活时，新添加的任务会被添加到这个队列中等待处理，如果队列满了，则新建非核心线程执行任务。
                             BlockingQueue<Runnable> workQueue,
       					  //创建线程的工厂类
                             ThreadFactory threadFactory,
       					  //拒绝策略handler
                             RejectedExecutionHandler handler) {
           if (corePoolSize < 0 ||
               maximumPoolSize <= 0 ||
               maximumPoolSize < corePoolSize ||
               keepAliveTime < 0)
               throw new IllegalArgumentException();
           if (workQueue == null || threadFactory == null || handler == null)
               throw new NullPointerException();
           this.corePoolSize = corePoolSize;
           this.maximumPoolSize = maximumPoolSize;
           this.workQueue = workQueue;
           this.keepAliveTime = unit.toNanos(keepAliveTime);
           this.threadFactory = threadFactory;
           this.handler = handler;
       }
   ```

   

6. 常用的阻塞队列类

   - ArrayBlockingQueue
   - LinkedBlockingQueue
   - DelayQueue
   - PriorityBlockingQueue
   - SynchronousQueue：无缓冲的等待队列

7. 拒绝策略

   1. ThreadPoolExecutor.AbortPolicy（默认）：丢弃任务并抛出RejectedExecutionException异常。
   2. ThreadPoolExecutor.DiscardPolicy：丢弃任务，不抛出异常。
   3. ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行新任务。
   4. ThreadPoolExecutor.CallerRunsPolicy：通过调用线程来处理该任务

8. execute方法的执行逻辑

   1. 如果当前运行的线程少于corePoolSize，则会创建新的线程来执行新的任务；
   2. 如果运行的线程个数等于或者大于corePoolSize，则会将提交的任务存放到阻塞队列workQueue中；
   3. 如果当前workQueue队列已满的话，则会创建新的线程来执行任务；
   4. 如果线程个数已经超过了maximumPoolSize，则会使用饱和策略RejectedExecutionHandler来进行处理。 

9. 线程池的关闭

   - 遍历线程池中的所有线程，然后依次中断。关闭线程池使用的是shutdown()和shutdownNow()两个方法



## 网络编程

1. InetAddress：对IP地址的访问类。
2. Socket套接字：网络上的两个程序通过一个双向的通信连接实现数据的交换，这个连接的一端称为一个Socket
   - Java中使用Socket完成TCP程序的开发，使用此类可以方便的建立可靠的、双向的、持续性的、点对但的通讯连接。
     - 在Socket的程序开发中，服务器端使用ServerSocket等待客户端的连接
     - 对于Java的网络程序来讲，每一个客户端都使用一个Socket对象表示。
     - 在网络传输结束时，需要添加流操作完成的标志。
       - 使用Socket类的shutdownOutput()或shutdownInput()方法，否则调用的close()方法只能释放流对象资源，不能释放其它向系统请求的资源，如程序端口等。
   - Java也可以完成TCP程序的开发，使用的是DatagramSocket，接收数据包使用了DatagramPackage。



## 函数式编程

函数式编程写出的代码简洁且意图明确，使用stream接口可以代替for循环；Java函数式编程更容易编写并行程序，只要调用parallel()方法。

#### 1.Lambda表达式

1. 使用Lanbda表达式可以替代单方法接口实现，告别匿名内部类，代码看起来更简洁易懂。Lambda表达式同时还提升了对集合、框架的迭代、遍历、过滤数据的操作。

2. 特点：函数式编程、参数类型自动推断、代码量少，简洁。

3. 任何有函数式接口的地方都可以使用Lambda表达式。

4. **语法**
   
   1. 基本格式：()  -> {}
   2. ()用于传递方法的参数
   3. {}中写的是方法体
   
5. 表达式案例

   ​		()->{}   ：无参，空方法

   ​		()->{System.out.println(1);}  ：无参，打印

   ​		()->System.out.println(1)：无参，由于只有一个语句，省去{}

   ​		()->{return 100;} ：无参，只返回值，值的类型要和方法的返回值一致。

   ​		()->100 ：无参，直接写返回值，省略return

   ​		()->null ： 无参，返回null。

   ​		(int x)->{return x+1;} ：一个int类型的参数，返回值

   ​		(int x)->x+1 ：一个int类型的参数，省略return，直接返回值

   ​		(x)->x+1 ： 一个省略类型的参数，返回值省略return

   ​		x->x+1 ： 只有一个参数，省略了()和参数类型，返回值省略return

   ​		() -> get() ：无参，返回值是get()的返回值，相当于return get()语句，但是要注意返回值类型要相同。

   ```java
   public class Main {
       public static void main(String[] args) {
           String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
   //此处需要传入一个Comparator实例，因为Comparator是单方法接口，所以可以使用lambda表达式替换。
           Arrays.sort(array, (s1, s2) -> {
               return s1.compareTo(s2);
           });
           System.out.println(String.join(", ", array));
       }
   }
   ```

   

#### 2. 函数式接口

* 只有一个**抽象方法**（Object类中的方法除外）的接口是函数式接口
* 使用注解@FunctionalInterface标记只定义了单抽象方法的接口（函数式接口）
* Java提供了一系列函数式接口，用来接受后续传入的逻辑，同时规定了参数和返回值的一些要求：
  * Supplier 代表一个输出 
  * Consumer 代表一个输入
  * BiConsumer 代表两个输入
  * Function 代表一个输入，一个输出（一般输入和输出是不同类型的）
  * UnaryOperator 代表一个输入，一个输出（输入和输出是相同类型的）
  * BiFunction 代表两个输入，一个输出（一般输入和输出是不同类型的）
  * BinaryOperator 代表两个输入，一个输出（输入和输出是相同类型的）

#### 3. 方法引用

​		对于使用lambda表达式处理的问题，当抽象方法的实现恰好只是调用另外一个方法来实现，就有可能可以使用方法引用来简化lambda表达式对该方法的调用。

1. 方法引用的分类

| 类型         | 语法               | 对应的lambda表达式                      |
| ------------ | ------------------ | --------------------------------------- |
| 静态方法引用 | 类名::staticMethod | () -> 类名.staticMethod(args)           |
| 实例方法引用 | inst::instMethod   | (args) -> inst.instMethod(args)         |
| 对象方法引用 | 类名::instMethod   | (inst, args) -> 类名.staticMethod(args) |
| 构造方法引用 | 类名::new          | () -> new 类名(args)                    |

```java
//静态方法引用：
public class Test2 {
    static String put() {
        System.out.println("putting...");
        return "put";
    }
    public static void main(String[] args) {
        //普通调用
        System.out.println(put());
        //使用Supplier的lambda表达式写法
        Supplier<String> s1 = () -> Test2.put();
        System.out.println(s1.get());
        //静态方法引用：类名::要调用的静态方法名
        Supplier<String> s2 = Test2::put;
        System.out.println(s2.get());
    }
}
//实例2
public class Main {
    public static void main(String[] args) {
        String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
//直接传入了静态方法cmp的引用
//因为Comparator<String>接口定义的方法是int compare(String, String)，和静态方法cmp的参数类型和返回类型一致，可以直接把方法名作为Lambda表达式传入
        Arrays.sort(array, Main::cmp);
        System.out.println(String.join(", ", array));
    }
    static int cmp(String s1, String s2) {
        return s1.compareTo(s2);
    }
}
```

```java
//实例方法引用
public class Test3 {
    public String toUpperCase(String str){
        return str.toUpperCase();
    }
    public static void main(String[] args) {
        //new对象::方法名
        Function<String, String> f1 = new Test3()::toUpperCase;
        f1.apply("fwojw");
        //某对象如果有多次调用，可以new出对象的实例，再进行方法引用
        //实例::方法
        Test3 t3 = new Test3();
        Function<String, String> f2 = t3::toUpperCase;
        f2.apply("ridsfr");
    }
}
//实例2
public class Main {
    public static void main(String[] args) {
        String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
//String的compareTo方法只有一个参数，但是由于实例方法有一个隐含的this参数，所以compareTo()在实际调用的时候，第一个隐含参数会传入this，与Comparator的compare()方法参数一致，可以作为方法引用传入。
        Arrays.sort(array, String::compareTo);
        System.out.println(String.join(", ", array));
    }
}
```

```java
//对象方法引用
public class Test4 {
    public static void main(String[] args) {
        Consumer<Too> c1 = (Too too) -> new Too().foo();
        c1.accept(new Too());
        Consumer<Too> c2 = (Too too) -> new Too2().foo();
        c2.accept(new Too());
        Consumer<Too> c3 = Too::foo;
        c3.accept(new Too());

        BiConsumer<Too2, String> bc = (Too2 too2, String str) -> new 				Too2().show(str);
        BiConsumer<Too2, String> bc2 = Too2::show;
        bc.accept(new Too2(), "cver");
        bc.accept(new Too2(), "cfwer");
    }
}

class Too {
    public Integer fun(String s) {return 1;}
    public void foo() {System.out.println("foo");}
}
class Too2 {
    public Integer fun(String s) {return 1;}
    public void foo() {System.out.println("foo---foo2");}
    public void show(String str){System.out.println("show------too2" + str);}
}
```

```java
//构造方法引用
public class Test5 {
    public static void main(String[] args) {
        Supplier<Person> s1 = () -> new Person();
        s1.get();
        Supplier<Account> s2 = Account::new;
        s2.get();

        Consumer<Integer> c1 = (age) -> new Account(age);
        Consumer<Integer> c2 = Account::new;
        c1.accept(2342);
        c2.accept(2343);

        Function<String,Account> f1 = Account::new;
        f1.apply("fwjoeiw");
    }
}

class Account {
    public Account() {
        System.out.println("无参构造");
    }
    public Account(int age) {
        System.out.println("age狗杂");
    }
    public Account(String str) {
        System.out.println(str + "构造");
    }
}
class Person {
    public Person(){
        System.out.println("oersong");
    }
}
```



#### 4.Stream API

1. Stream是一组来处理数组、集合的API。
2. Stream的特性：
   - 不是数据结构，没有内部存储
   - 不支持索引访问
   - 延迟计算
   - 支持并行
   - 很容易生成数组或集合（List、Set）
   - 支持过滤、查找、转换、汇总、聚合等操作。
3. Stream运行机制
   - Stream分为：源source、中间操作、终止操作
   - 流的源可以是一个数组、一个集合、一个生成器方法、一个I/O通道等等
   - 一个流可以有0个或者多个中间操作，每一个中间操作都会返回新的流，共下一个操作使用。
   - 一个流只有一个终止操作。Stream只有遇到终止操作，它的源才开始执行遍历操作。
4. Stream的生成操作

```java
//由数组生成
static void gen1() {
    String[] strs = {"a", "b", "c", "d", "df"};
    Stream<String> strs1 = Stream.of(strs);
    strs1.forEach(System.out::println);
}
//由集合生成
static void gen2() {
    List<String> list = Arrays.asList("1", "2", "4", "3");
    Stream<String> stream = list.stream();
    stream.forEach(System.out::println);
}
//通过Stream.generate()方法创建
static void gen3() {
    Stream<Integer> generate = Stream.generate(() -> 1);
    generate.limit(10).forEach(System.out::println);
}
//通过Stream.iterate()方法创建
static void gen4() {
    Stream<Integer> iterate = Stream.iterate(1, x -> x + 1);
    iterate.limit(10).forEach(System.out::println);
}
//其它API创建，如子类IntStream等。。
static void gen5() {
    String str = "fsfwfwfwvwo";
    IntStream chars = str.chars();
    chars.forEach(System.out::println);
}
```

5. Stream的中间操作、终止操作

   如果调用方法之后返回的结果是Stream对象，就意味着是一个中间操作。如果返回了其它类型可能就是一个终止操作。

#### 5. 自定义注解

1. 注解Annotation

   - Annontation是Java5开始引入的新特征，中文名称叫注解。 
   - 它提供了一种安全的类似注释的机制，用来将任何的信息或元数据（metadata）与程序元素（类、方法、成员变量等）进行关联。 
   - 为程序的元素（类、方法、成员变量）加上更直观更明了的说明，这些说明信息**与程序的业务逻辑无关**，并且供指定的工具或框架使用。 
   - Annontation像一种修饰符一样，应用于包、类型、构造方法、方法、成员变量、参数 及本地变量的声明语句中。
   - Java注解是附加在代码中的一些**元信息**，用于一些工具在编译、运行时进行解析和使用， 起到说明、配置的功能。
   - **注解不会也不能影响代码的实际逻辑**，仅仅起到辅劣性的作用。包含在 java.lang.annotation 包中。 

2. 注解的作用

   1. 生成文档。这是最常见、也是Java最早提供的注解。常见的有@param、@return等
   2. 跟踪代码依赖性，实现替代配置文件功能
   3. 在编译时进行格式检查。如@override放在方法前，可以在编译时检测到该方法是否覆盖了超类方法。

3. 几个内置注解

   1. @Override：定义在java.lang.Override中，此注释只适用于修饰方法，表示一个方法声明打算重写超类中的另一个方法声明。
   2. @Deprecated：定义在java.lang.Deprecated中，此注释可以 修饰方法、属性、类，表示该元素已经过时，不鼓励程序员使用这样的元素，通常是因为它很危险戒者存在更好的选择。
   3. @SuppressWarnings：定义在java.lang.SuppressWarnings中，用来抑制编写编译时的警告信息。

4. 元注解

   ​      元注解的作用是负责注解其他注解，java中定义了四个标准的meta-annotation类型，他们被用来提供对其他annotation类型作说明。

   - @Target：用来描述注解的使用范围（注解可以用在什么地方）
     - 使用范围的取值可以在枚举类ElementType中找到。
   - @Retention：表示需要在什么级别保存该注释信息，描述注解的生命周期。一般都是Runtime
     - 声明周期的取值在枚举类RetentionPolicy中定义。
     - 三个注解的声明周期长度：Source（源码级别） < Class（类级别） < Runtime（运行时环境）
   - @Document：说明该注解将被包含在javadoc中。
   - @Inherited：说明子类可以继承父类中的该注解。

5. 自定义注解

```java
public class MetaAnnotation {
    //使用对方法的注解
    @MyAnnotation(name = "a", age = 2, id = 34)
    public void test() {}
}
//元注解
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.CLASS)
@Inherited
@interface MyAnnotation{
    //在自定义注解中定义的方法，实际上是在使用注解的时候输入的参数名称，默认的参数名是value
    String name();
    int age();
    int id();
    //或者使用default在定义时设定默认值，在使用注解时就无需赋值了
    String[] likes() default {"dffw", "fwfew"};
}
```

6. 注解的原理：反射和内省。