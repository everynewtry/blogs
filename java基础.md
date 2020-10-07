## 1.数据类型

###  基本数据类型（primitive type）

#### 数值类型

1. 整数类

   - byte: 占一个字节

   - short：占个字节

   - int：占四个字节

     十进制，八进制（以0开头）十六进制（以0x开头）

     例如：int i1=10；（十进制）

     int i2=010；（八进制）

     int i3=0x10；（十六进制）

   - long：占八个字节

2. 浮点类型（小数）

   - float占4个字节

   - double占8个字节

     bigdecimal类

3. 字符类型

   char占2个字节，仅仅代表一个字，例如“中”，而“中国”就不行

   char本质是一个数值类型，可以进行加减运算

   例如：

   ```java
   char c='a';
   c++;
   System.out.println(c);
   ```

   

#### 布尔类型

1.  占一位

## 引用数据类型（reference type）

类，接口，数组

## 数据类型换

低---------------------------------------------------->高

byte,short,char->int->long->float->double

**类型转换从高到低需要强制类型转化，从低到高可以自动完成转化**

注意：

1. 转化的时候可能出现内存溢出或者精度问题
2. 例如：

```java
int money=10_000_000_000;
int years=20;
int total=money*yeras;//因为是int 输出时就会溢出
long  total=money*yeras;//计算结果是int还是会溢出
//解决方法：要先把一个数转化为long
long total=money*((long)years);
System.out.println(total);
    

```

# 2.数组

## 一维数组

### 数组的定义

```java
 //动态初始化  第一种方式
int data[]=null;
data =new int[3];
data[0]=1;
data[1]=2; 
data[2]=3; 

//第二种方式
int  data[]=new int[3];
data[0]=1;
data[1]=2; 
data[2]=3; 

//静态初始化
int data[]=
    
//二维数组
int[][] arr = new int[3][2];//第一种 
int[][] arr = new int[3][];//第二种 
int[][] arr = {{1,2},{3,4}};//第三种
```

# 3.内部类

## 分类

1. 成员内部类

```java
public class Outer1 {
    private int id =10;
    public  void  out(){
        System.out.println("这是一个外部类");
    }
    //成员内部类,需要用public修饰
    public class  Inner{
        public  void in(){
            System.out.println("这是一个内部类");

        }
        //访问外部私有变量
        public void getId(){
            System.out.println(id);
        }
    }
```



```java
public class application {
    public static void main(String[] args) {

        Outer1 outer1=new Outer1();
        outer1.out();
        //使用内部类
        Outer1.Inner inner= outer1.new  Inner();
        inner.in();
        inner.getId();

    }
}
```



2. 静态内部类

**静态内部类和成员内部类的实例化有区别**

```java
public class Outer1 {
    private int id =10;
    public  void  out(){
        System.out.println("这是一个外部类");
    }
    //成员内部类,需要用public修饰
    public static class  Inner{
        public  void in(){
            System.out.println("这是一个内部类");

        }
        //访问外部私有变量 ,当使用static修饰之后，就无法访问外部私有变量，
        // 因为先初始化static修饰的静态内部类
        //除非id用static修饰
       /* public void getId(){
            System.out.println(id);
        }*/
    }

} 
```

---

```java
3.public class application {
    public static void main(String[] args) {

        Outer1 outer1=new Outer1();
        outer1.out();
        //使用内部类
        Outer1.Inner inner=new Outer1.Inner();
        inner.in();
        inner.getId();

    }
}
```

**注：**

**一个java文件中可以有多个class修饰的类，但是只能有一个类被public修饰**



3. 局部内部类

   将类写在方法里面

   局部内部类是定义在一个方法或者一个作用域里面的类，它和成员内部类的区别在于局部内部类的访问仅限于方法内或者该作用域内。

   **注意，局部内部类就像是方法里面的一个局部变量一样，是不能有public、protected、private以及static修饰符的。**

4.  匿名内部类

```java
public class application {
    public static void main(String[] args) {
        //直接用接口实例化，然后重写方法，为匿名内部类
    App a=new App(){
        @Override
        public Outer1 outer() {
            return new Outer1();
        }
    };
    Outer1 out1=a.outer();
    out1.out();
}
}

interface App{
   public  Outer1 outer();
}
```

# 4.异常

## 异常体系

Throwable作为所有异常的超类。

异常分为两大类，错误Error和异常Exception

**Error**

Error类对象由java虚拟机生成并抛出，大多数错误与编写者无关。

例如：OutOfMemoryError(Jvm内存不足)，NoClassDefFoundError（类定义错误）

**Exception**

**RuntimeException**(运行时异常)：

- ArrayIndexOutofBoundsException,数组下标异常
- NullPointerException，空指针异常
- ArithmeticException，算术异常
- MissingResourceException，丢失资源
- ClassNotFoundException，找不到类

这些异常一般是由程序逻辑错误引起的。

**非RuntimeException异常，编译时异常**

**Error和**

这些异常通常由程序的逻辑错误引起。

**throws**

方法抛出异常，让此方法的调用者接受异常

void  test（int  a）throws Exception{

内部抛出了异常且没有捕获

}

try{

test（1）；

}

catch（Exception e）{

e.printStackTrace（）；

}