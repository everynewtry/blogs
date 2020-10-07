# 集合（学习笔记）

## 1.java集合框架

![集合](E:\2020研究生暑假\java\图片\捕获.PNG)

![](E:\2020研究生暑假\java\图片\获.PNG)

**分析**：

1. 存储对象可以使用数组和集合

2. **数组**存储对象：

   Student[] stu =new Student[20]  ;stu[0]=new Student();.....

   **弊端**：

   - 创建后，其长度不可变
   - 真实存了几个对象并不可知



### Iterator接口 （迭代器）

用来遍历集合中的元素

```java 
Iterator i= coll.iterator();//Coll为接口Collection的实现类对象
while(i.hasNext()){
    System.out.println(i.next());
}
//错误的写法
//分析：刚开始指针在集合的第一个元素外，然后使用Next方法之后指针会下移
while((i.next())!=null){
    System.out.println(i.next());
}

//增强for循环实现集合遍历
for(Object obj:coll)//从集合coll中取出一个元素存入Object中
{
    System.out.println(obj);
}

```



### Collection接口API

**collection方法**：

**size**（）集合大小；

**add**(Object obj)给集合内添加元素

**addAll**(Collection coll)将一个Collection加入另一个Collection

**isEmpty**（）判断一个集合是否为空，返回布尔类型

**clear**()清空集合元素

**contains**（Object obj）判断集合是否包含元素obj，返回布尔类型，判断的依据是根据元素obj所在类的equals（）方法。**自定义类需要重写equals（）才能判断是否包含。**

**containsAll**（Collection coll）

**retainAll**(Collection coll) 求两个集合交集

**remove**（Object obj）移除一个元素

**removeAll**（Collection coll）移除两个集合交集 

**toArray**（）将集合转化为数组，Object[]  obj=coll.toArray();

**注：**将数组转化为集合使用collection工具了类方法，Arrays.asList(1,2,3);

**iterator**()返回一个Iterator接口的实现类的对象，用于集合的遍历。

- Iterator方法：hasNext（）
- next()

### Collection接口

**将自定义类的对象添加进set中，自定义类需要重写equals（）和hashcode()方法，以防止添加相同的对象到set中，这两个方法在Object中定义**

- Set子接口（**无序，不可重复**）

  set存储使用哈希算法，用来保证不可重复性，**具体过程是当给set中添加元素时，先使用hashCode（）方法计算hash值，而hash值决定了元素的存储位置，如果此hash'值对应的位置没有元素，则将元素存入，否则不存入**。

  1. HashSet  

     无序，因为hash函数映射到不同的位置，所以元素位置无序

  2. LinkedHashSet

     使用链表维护了元素添加进集合的顺序，因此当我们遍历LinkedHashSet时，是按照添加的顺序进行遍历。注意：存储的位置依旧无序，只是维护了一个记录添加顺序信息的链表（且是个双向链表）。

     ![](E:\2020研究生暑假\java\图片\捕.PNG)

     **由此可以发现LinkedHashSet遍历更加方便，但添加删除元素，HashSet更加快捷，因为它不需要维护一个链表**

  3. TreeSet

     - 添加的元素必须是同一类型的

     - 可以按照添加进集合的指定顺序进行遍历。默认按照从小到大的顺序遍历

     - 自定义类的对象要添加进TreeSet中，**自定义类必须实现Comparable接口,并实现compareTo方法，用来确定使用哪个属性进行排序。**

     - 要使compareTo(),equals(),hashCode()方法的结果保持一致。因此为了有正确的排序结果，必须对compareTo（）方法进行合理的编写。

       ```java
       public int compareTo(Object o){
           if(o instanceof  Person){
               Person p=(Person) o;
               return (this.age.compareTo(p.age)==0?this.name.compareTo(p.name):this.age.compareTo(p.age));
           }
           //先按照age排序在按name排序
           return 0；
       } //写在类似javabean的类的方法中
       ```

     - 排序方法有两种，自然排序和定制排序，以上为自然排序

     - 定制排序：

       ```java
       public class Customer {
           private Integer id ;
           private String name;
       
           public Integer getId() {
               return id;
           }
       
           public void setId(int id) {
               this.id = id;
           }
       
           public String getName() {
               return name;
           }
       
           public void setName(String name) {
               this.name = name;
           }
       
           public Customer(int id, String name) {
               this.id = id;
               this.name = name;
           }
       
           @Override
           public String toString() {
               return "Customer{" +
                       "id=" + id +
                       ", name='" + name + '\'' +
                       '}';
           }
       }
       ```

       ```java
       public class application {
           public static void main(String[] args) {
               //1。创建一个实现Comparator接口的类的对象，此处通过匿名类的形式进行实现
               Comparator com=new Comparator() {
                   @Override
                   public int compare(Object o1, Object o2) {
                       if(o1 instanceof Customer && o1 instanceof Customer){
                            Customer c1=(Customer) o1;
                            Customer c2=(Customer) o2;
                            return  (c1.getId().compareTo(c2.getId())==0 ? c1.getName().compareTo(c2.getName()):c1.getId().compareTo(c2.getId()));//注意如果id是int类型则不能用compareTo
       
                       }
                       return 0;//无法添加进set
                   }
               };
               //2.把此对象作为TreeSet的形参。传入
               TreeSet set=new TreeSet(com);
               //3.向TreeSet中添加Comparator接口中的compare方法中涉及的对象
               set.add(new Customer(6,"AA"));
               set.add(new Customer(2,"BB"));
               set.add(new Customer(5,"CC"));
               Iterator i=set.iterator();
               while(i.hasNext()){
                   System.out.println(i.next());//Customer必须重写toString()方法，否则无法完成输出
               }
           }
       }
       ```

       

- List子接口（**可重复**）

  1. 相当于动态数组

     **增 void add(int index,Object obj)** 在指定索引位置添加元素，不覆盖原来的元素

     **改 set(int index,Object obj)** 将指定位置的元素替换

     **查 Object get(int index)** 获得指定索引的元素

     **int indexOf(Object obj)** 获得元素第一次出现的位置

     **int lastIndexOf(Objece obj)** 获得元素最后一次出现的位置

     **List   subList(int fromIndex, int toIndex)** 

     

  2. LinkedList

     底层链表实现

  3. Vector

     是一个古老的Collection实现类，它是线程安全的

### Map（key-value对）

**map与Collection并列**

key用set存放不允许重复，而value可以重复

![](E:\2020研究生暑假\java\图片\1获.PNG)

**虚线表是实现，实线是继承**

元素查询，添加(**put但在collection中添加是add**)，删除操作：

```java
public class application {
    /*

     Map方法：
    Object put(Object key,Object value)
    Object remove（Object key）  按照指定的key删除key-value，且返回key对应的value
    void putAll(Map t)
    void clear()
    Object get(Object key)  获取指定key的value值
    boolean containsKey(Object key)   map中是否包含指定的key
    boolean containsValue(Object value )  map中是否包含指定的value 注意key和value对应的类必须重写equals方法
    int size()
    boolean isEmpty()
    boolean equals(Object obj)

    1.HashMap中的key使用set存放的不可重复，value是用collection来存放的，可重复，一个key-value是一个entry，
    所有的entry是用set存放的，也是不可重复的
    2.向HashMap中添加元素的时候，会调用key所在的equals（）方法，判断两个key是否相同，若相同只能添加一个，添加的元素
    为后添加的那个
    3.自定义对象凡是要存进集合的equals方法必须写，凡是与set和map相关的HashCode方法也必须写，而list可以省掉hashcode

     */
    public static void main(String[] args) {


    Map map =new HashMap();
    map.put("AA",123);
    map.put("BB",124);
    map.put("CC",125);
    map.put("DD",126);
    map.put(new Customer(1,"chen"),12);
    map.put(new Customer(1,"chen"),13);//需重写equlas（）方法否则能添加进去
    map.put(null,null);
    System.out.println(map.size());
    map.remove("BB");
    System.out.println(map);

}
}
```

元视图操作方法：

```java
       如何遍历map
    Set keySet（）
    Collection values()
    Set entrySet()
     */
    //遍历key集
        Set set =map.keySet();
        //set.iterator()
        for(Object o:set){
            System.out.println(o);
        }
    //遍历value集
        Collection values=map.values();
        Iterator i=values.iterator();
        while(i.hasNext()){
            System.out.println(i.next());

        }
    //遍历entry，是一个set
        Set entry=map.entrySet();
        Iterator i2=entry.iterator();
        while(i2.hasNext()){
            System.out.println(i2.next());
        }
     // 获取key-value的另一种方法
     Iterator i3=set.iterator();
     while(i3.hasNext()){
        // System.out.println(i3.next()+"------>"+map.get(i3.next())); 这样写有错，因为next()都会使指针下移
        Object obj=i3.next();
         System.out.println(obj+"----->"+map.get(obj));
     }

```

1. HashMap

   map的主要实现类

   - LinkedHashMap用链表维护一个添加的顺序，输出的时候按照这个顺序输出，与前面的LinkedHashSet相同

2. TreeMap

   按照添加进map中的元素的key的指定属性进行排序(所以key必须是同一个类的对象)，相当于TreeMap中的key就是一个TreeSet。

   **与TreeSet类似，也存在自然排序和定制排序两种类型**

   - 自然排序

     自定义类必须实现comparable接口并且重写compareTo方法

     ```java
     public class TreeMapTest {
         public static void main(String[] args) {
             Map map=new TreeMap();
             //Customer类必须实现Comparable接口，并且重写compareTo()方法，规定先用哪个属性进行排序
             map.put(new Customer(1,"chen"),98);//value假设是他的成绩
             map.put(new Customer(7,"wang"),60);
             map.put(new Customer(3,"li"),99);
             Set set=map.entrySet();
             for(Object o: set){
                 System.out.println(o);
             }
     
         }
     }
     ```

     Customer重写compareTo方法

     ```java
        public int compareTo(Object o) {
             if(o instanceof  Customer){
                 Customer c=(Customer) o;
                 return (this.getId().compareTo(c.getId())==0?this.getName().compareTo(c.getName()):this.getId().compareTo(c.getId()));
             }
             return 0;
         }
     ```

     

   - 定制排序:

     用匿名内部类的形式重写compare方法

     ```java
     public class TreeMapTest {
         public static void main(String[] args) {
     
             Map map=new TreeMap(new Comparator() {
                 @Override
                 public int compare(Object o1, Object o2) {
                     if(o1 instanceof Person && o2 instanceof Person){
                         Person c1=(Person) o1;
                         Person c2=(Person) o2;
                         return (c1.getId().compareTo(c2.getId())==0 ? c1.getName().compareTo(c2.getName()):c1.getId().compareTo(c2.getId()));
                     }
                     return 0;
                 }
             });
             map.put(new Person(1,"chen"),90);
             map.put(new Person(7,"wang"),88);
             map.put(new Person(3,"li"),60);
             System.out.println(map);
         }
     }
     ```

     

      

3. Hashtable

   - Hashtable是一个古老的Map实现类，线程安全的 

   - 与HashMap不同，Hashtable不允许使用null作为key或者value

   - 与HashMap相同，Hashtable也不保证其中key-value对的顺序

   - 已经废弃，不建议使用

   - 子类Properties常用来处理属性文件，键和值都是String 类型

     ```java
     public class testPropeties {
         public static void main(String[] args) throws IOException {
             Properties pro=new Properties();
             pro.load(new FileInputStream(new File("jdbc.properties")));//文件建在当前工程下
             String id=pro.getProperty("id");
             String password=pro.getProperty("password");
             System.out.println(id);
             System.out.println(password);
         }
     }
     ```

     

### Collections工具类

Collections类中提供了多个synchronizedXxx（）方法，该方法可使指定的集合包装成线程同步的集合（即线程安全），从而解决多线程并发访问集合时的线程安全问题

```java

public class TestCollection {
    //操作Collection和Map的工具类
   // 面试题Collection与Collections的区别
    //Collection 是一个集合而Collections是一个操作集合的工具类 工具类中的大部分方法都是静态的
    /*
    * Collections中的方法
    * reverse(List l) 反转list中的元素顺序
    * shuffle（List l）对List集合中的元素进行随机排序
    * sort（List l）默认按照从小到的顺序进行排序
    * sort（List l, Comparator）根据Comparator产生的顺序对List集合进行排序
    * swap (List l,int i,int j) 将指定list集合中的i处的元素和j处的元素进行交换
    *
    * Object max(Collection coll) 按照元素的自然顺序，返回集合中的最大元素
    * Object max(Collection coll. Comparator com) 根据Comparator指定的顺序，返回集合中的最大值
    * Object min(Collection coll)
    * Object min(Collection coll,Comparator com)
    * int frequency(Collection coll, Object obj) 返回集合中指定元素出现的次数
    * void copy(List dest,List src) 将src中的内容复制到dest中
    * boolean replaceAll(List list,Object oldVal, Object newVal)
    * */
    public static void main(String[] args) {
        List list=new ArrayList();
        list.add(124);
        list.add(12);
        list.add(1284);
        //实现List集合的复制
        //List list2=new ArrayList();//错误的实现方式
        List list2= Arrays.asList(new Object[list.size()]);//这样实现list2中就可以装的下list
        Collections.copy(list2,list);
        Collections.sort(list);
        Collections.reverse(list);
        System.out.println(list);
    }

}
```

