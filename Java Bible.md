# Java

## SE基础

### 基础概念

#### Java 语言有哪些特点?

1. 简单易学；
2. 面向对象；
3. 可移植性；
4. 支持多线程；
5. 可靠性（具备异常处理和自动内存管理机制）；
6. 安全性（Java 语言本身的设计就提供了多重安全防护机制如访问权限修饰符、限制程序直接访问操作系统资源）；
7. 高效性（Just In Time 编译器等技术的优化）；
8. 支持网络编程；
9. 编译与解释并存；
10. 强大的生态



#### JVM vs JDK vs JRE

![jdk-include-jre](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307081015692.png)



####  什么是字节码?采用字节码的好处是什么?

1. 在 Java 中，JVM 可以理解的代码就叫做字节码，即扩展名为 `.class` 的文件，它不面向任何特定的处理器，只面向虚拟机。
2. Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。因此，Java 程序运行时相对来说还是高效的，而且，由于字节码并不针对一种特定的机器，因此实现了一次编译随处运行。



#### 为什么说Java 是编译与解释共存的语言

`.class->机器码` 这一步 **JVM 类加载器**首先加载字节码文件，然后通过**解释器**逐行解释执行，这种方式的执行速度会相对比较慢。而且，有些方法和代码块是经常需要被调用的（即热点代码），所以后面引进了 JIT（just-in-time compilation） 编译器，而 JIT 属于运行时编译。当 JIT 编译器完成第一次编译后，其会将字节码对应的机器码保存下来，下次可以直接使用。而我们知道，机器码的运行效率肯定是高于 Java 解释器的。这也解释了我们为什么经常会说 **Java 是编译与解释共存的语言** 。

> JIT位于JVM中

HotSpot 采用了惰性评估(Lazy Evaluation)的做法，根据二八定律，消耗大部分系统资源的只有那一小部分的代码（热点代码），而这也就是 JIT 所需要编译的部分。JVM 会根据代码每次被执行的情况收集信息并相应地做出一些优化，因此执行的次数越多，它的速度就越快。

JDK 9 引入了一种新的编译模式 AOT(Ahead of Time Compilation)，它是直接将字节码编译成机器码，这样就避免了 JIT 预热等各方面的开销。JDK 支持分层编译和 AOT 协作使用。

![image-20230708103222895](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307081032928.png)



#### AOT 可以提前编译节省启动时间为什么不全部使用 AOT 

这和 Java 语言的动态特性有联关系。例如，CGLIB 动态代理使用的是 ASM 技术，而这种技术大致原理是运行时直接在内存中生成并加载修改后的字节码文件也就是 `.class` 文件，如果全部使用 AOT 提前编译，也就不能使用 ASM 技术了。为了支持类似的动态特性，所以选择使用 JIT 即时编译器。



#### Oracle JDK vs OpenJDK

| Oracle JDK           | Open JDK      |
| -------------------- | ------------- |
| 部分内容闭源         | 完全开源      |
| 有限的免费版本       | 完全免费      |
| 功能更强             | 基础功能      |
| 约3年发布一个LTS版本 | 不提供LTS版本 |



### 基本语法

#### 注释有哪几种形式？

1. **单行注释**：通常用于解释方法内某单行代码的作用。
2. **多行注释**：通常用于解释一段代码的作用。
3. **文档注释**：通常用于生成 Java 开发文档。



####  标识符和关键字的区别是什么？

标识符就是一个名字，关键字是被赋予特殊含义的标识符。

|                      |          |            | 关键字   |              |            |           |        |
| :------------------- | -------- | ---------- | -------- | ------------ | ---------- | --------- | ------ |
| 分类                 | 关键字   |            |          |              |            |           |        |
| 访问控制             | private  | protected  | public   |              |            |           |        |
| 类，方法和变量修饰符 | abstract | class      | extends  | final        | implements | interface | native |
|                      | new      | static     | strictfp | synchronized | transient  | volatile  | enum   |
| 程序控制             | break    | continue   | return   | do           | while      | if        | else   |
|                      | for      | instanceof | switch   | case         | default    | assert    |        |
| 错误处理             | try      | catch      | throw    | throws       | finally    |           |        |
| 包相关               | import   | package    |          |              |            |           |        |
| 基本类型             | boolean  | byte       | char     | double       | float      | int       | long   |
|                      | short    |            |          |              |            |           |        |
| 变量引用             | super    | this       | void     |              |            |           |        |
| 保留字               | goto     | const      |          |              |            |           |        |

> ⚠️ 注意：虽然`true`,`false`,和`null`看起来像关键字但实际上他们是字面值，同时也不可以作为标识符来使用。
>
> this的用法：this主要用来解决：变量名称冲突问题
>
> - this在方法中：哪个对象调用这个方法，this就拿到哪个对象
> - this在构造器中：哪个对象调用构造器，this就拿到哪个对象
> - this的作用：在变量名见名知意又冲突的时候，this可以解决局部变量和成员变量命名冲突的问题，可以指定访问的是当前对象的成员变量



####  自增自减运算符

“符号在前就先加/减，符号在后就后加/减”。



#### 移位运算符

- `<<` :左移运算符，向左移若干位，高位丢弃，低位补零。`x << 1`,相当于 x 乘以 2(不溢出的情况下)。
- `>>` :带符号右移，向右移若干位，高位补符号位，低位丢弃。正数高位补 0,负数高位补 1。`x >> 1`,相当于 x 除以 2。
- `>>>` :无符号右移，忽略符号位，空位都以 0 补齐。

>由于 `double`，`float` 在二进制中的表现比较特殊，因此不能来进行移位操作。
>
>移位操作符实际上支持的类型只有`int`和`long`，编译器在对`short`、`byte`、`char`类型进行移位前，都会将其转换为`int`类型再操作。
>
><hr/>
>
>当 int 类型左移/右移位数大于等于 32 位操作时，会先求余（%）后再进行左移/右移操作。也就是说左移/右移 32 位相当于不进行移位操作（32%32=0），左移/右移 42 位相当于左移/右移 10 位（42%32=10）。当 long 类型进行左移/右移操作时，由于 long 对应的二进制是 64 位，因此求余操作的基数也变成了 64。
>
>也就是说：`x<<42`等同于`x<<10`，`x>>42`等同于`x>>10`，`x >>>42`等同于`x >>> 10`。

```java
// 右移运算符代码示例：

int i = -1;
System.out.println("初始数据：" + i);
System.out.println("初始数据对应的二进制字符串：" + Integer.toBinaryString(i));
i <<= 10;
System.out.println("左移 10 位后的数据 " + i);
System.out.println("左移 10 位后的数据对应的二进制字符 " + Integer.toBinaryString(i));
i <<= 42;
System.out.println("左移 10 位后的数据 " + i);
System.out.println("左移 10 位后的数据对应的二进制字符 " + Integer.toBinaryString(i));
```

```
初始数据：-1
初始数据对应的二进制字符串：11111111111111111111111111111111
左移 10 位后的数据 -1024
左移 10 位后的数据对应的二进制字符 11111111111111111111110000000000
左移 10 位后的数据 -1024
左移 10 位后的数据对应的二进制字符 11111111111111111111110000000000
```



####  continue、break 和 return 的区别是什么？

1. `continue`：指跳出当前的这一次循环，继续下一次循环。
2. `break`：指跳出整个循环体，继续执行循环下面的语句。
3. `return` 用于跳出所在方法，结束该方法的运行。return 一般有两种用法：
   - `return;`：直接使用 return 结束方法执行，用于没有返回值函数的方法。
   - `return value;`：return 一个特定值，用于有返回值函数的方法。



### 基本数据类型

#### Java 中的几种基本数据类型了解么？

| 基本类型  | 位数 | 字节 | 默认值  | 取值范围                                   | 包装类    |
| :-------- | :--- | :--- | :------ | ------------------------------------------ | --------- |
| `byte`    | 8    | 1    | 0       | -128 ~ 127                                 | Byte      |
| `short`   | 16   | 2    | 0       | -32768 ~ 32767                             | Short     |
| `int`     | 32   | 4    | 0       | -2147483648 ~ 2147483647                   | Integer   |
| `long`    | 64   | 8    | 0L      | -9223372036854775808 ~ 9223372036854775807 | Long      |
| `char`    | 16   | 2    | 'u0000' | 0 ~ 65535                                  | Character |
| `float`   | 32   | 4    | 0f      | 1.4E-45 ~ 3.4028235E38                     | Float     |
| `double`  | 64   | 8    | 0d      | 4.9E-324 ~ 1.7976931348623157E308          | Double    |
| `boolean` | 1    |      | false   | true、false                                | Boolean   |

> - 在Java中，局部变量（如在方法内部定义的变量）没有默认值，必须显式地初始化才能使用。这是因为局部变量的值是不确定的，直到你为其赋予一个具体的值。
> - 但是类的成员变量（即类的字段）会根据其类型有默认值。
> - 所有引用类型的默认值都是null（包装类属于引用类型）。



#### 基本数据类型和包装类型的区别？

1. **用途**：除了定义一些常量和局部变量之外，在其他地方很少使用基本类型。并且包装类型可用于泛型，基本类型不可以。
2. **存储方式**：基本数据类型的局部变量存放在 Java 虚拟机栈中的局部变量表中，基本数据类型的成员变量（未被 `static` 修饰 【static修饰】的静态变量位于方法区中）存放在 Java 虚拟机的堆中。包装类型属于对象类型，我们知道几乎所有对象实例都存在于堆中。
3. **占用空间**：相比于包装类型（对象类型）， 基本数据类型占用的空间往往非常小。
4. **默认值**：成员变量包装类型不赋值就是 `null` ，而基本类型有默认值且不是 `null`。
5. **比较方式**：对于基本数据类型来说，`==` 比较的是值。对于包装数据类型来说，`==` 比较的是对象的内存地址。所有整型包装类对象之间值的比较，全部使用 `equals()` 方法。

**为什么说是几乎所有对象实例都存在于堆中呢？** 这是因为 HotSpot 虚拟机引入了 JIT 优化之后，会对对象进行逃逸分析，如果发现某一个对象并没有逃逸到方法外部，那么就可能通过标量替换来实现栈上分配，而避免堆上分配内存。

> ⚠️ 注意：**基本数据类型存放在栈中是一个常见的误区！** 基本数据类型的成员变量如果没有被 `static` 修饰的话（不建议这么使用，应该要使用基本数据类型对应的包装类型），就存放在堆中。



#### 包装类型的缓存机制了解么？

Java 基本数据类型的包装类型的大部分都用到了缓存机制来提升性能。

- `Byte`,`Short`,`Integer`,`Long` 这 4 种包装类默认创建了数值 **[-128，127]** 的相应类型的缓存数据。
- `Character` 创建了数值在 **[0,127]** 范围的缓存数据。
- `Boolean` 直接返回 `True` or `False`。
- 如果超出对应范围仍然会去创建新的对象，缓存的范围区间的大小只是在性能和资源之间的权衡。
- 两种浮点数类型的包装类 `Float`,`Double` 并没有实现缓存机制。

```java
int a = 100;
int b = 200;

Integer c = a; // 100
Integer d = a; // 100
Integer e = b; // 200
Integer f = b; // 200

long l = 100l;
Long ll = l; // 100

System.out.println(c == d);       //true  Integer值100在Integer的缓存范围中，所以指向同一个对象，即地址相同
System.out.println(e == f);       //false Integer的缓存范围是-128~127，200超出了缓存范围中，所以指向不同的对象，即地址不同
System.out.println(a == l);       //true  字符串常量池中byte short int long类型的范围是-128~127，所以指向同一个对象，即地址相同
System.out.println(c == l);       //true  包装类在与基本数据类型做==比较时，包装类型会自动拆箱为基本数据类型，所以比较的是基本数据类型，即地址相同
System.out.println(e.equals(f));  //true  包装类的equals方法重写过，当比较双方的类型不同时，返回false；当比较双方的类型相同时，比较的是实际数据值
System.out.println(c.equals(l));  //false 类型不同，返回false
System.out.println(c.equals(ll)); //false 类型不同，返回false

Float i11 = 333f;
Float i22 = 333f;
System.out.println(i11 == i22);// 输出 false

Double i3 = 1.2;
Double i4 = 1.2;
System.out.println(i3 == i4);// 输出 false

Integer i1 = 40;
Integer i2 = new Integer(40);
System.out.println(i1 == i2);        // false
// Integer i1=40 会发生装箱，等价于Integer i1=Integer.valueOf(40) 因此，i1 直接使用的是缓存中的对象。而Integer i2 = new Integer(40) 会直接创建新的对象。
```

> 《阿里巴巴Java开发规约》【强制】**所有整型包装类对象之间值的比较，全部使用 equals 方法比较**



#### 自动装箱与拆箱了解吗？原理是什么？

- **装箱**：将基本类型用它们对应的引用类型包装起来；
- **拆箱**：将包装类型转换为基本数据类型；

装箱其实就是调用了 包装类的`valueOf()`方法，拆箱其实就是调用了 `xxxValue()`方法。

因此，

- `Integer i = 10` 等价于 `Integer i = Integer.valueOf(10)`

- `int n = i` 等价于 `int n = i.intValue()`;

> ⚠️**如果频繁拆装箱的话，也会严重影响系统的性能。我们应该尽量避免不必要的拆装箱操作。**



#### 为什么浮点数运算的时候会有精度丢失的风险？

计算机底层是二进制的，而且计算机在表示一个数字时，宽度是有限的，无限循环的小数存储在计算机时，只能被截断，所以就会导致小数精度发生损失的情况。



#### 如何解决浮点数运算的精度丢失问题？

`BigDecimal` 可以实现对浮点数的运算，不会造成精度丢失。通常情况下，大部分需要浮点数精确运算结果的业务场景（比如涉及到钱的场景）都是通过 `BigDecimal` 来做的。

>《阿里巴巴 Java 开发手册》中提到：*浮点数之间的等值判断，基本数据类型不能用 == 来比较，包装数据类型不能用 equals 来判断。*

> - 创建
>
> 在使用 `BigDecimal` 时，为了防止精度丢失，推荐使用它的`BigDecimal(String val)`构造方法或者 `BigDecimal.valueOf(double val)` 静态方法来创建对象。

>- 加减乘除
>
>`add` 方法用于将两个 `BigDecimal` 对象相加，`subtract` 方法用于将两个 `BigDecimal` 对象相减。`multiply` 方法用于将两个 `BigDecimal` 对象相乘，`divide` 方法用于将两个 `BigDecimal` 对象相除。
>
>```java
>BigDecimal a = new BigDecimal("1.0");
>BigDecimal b = new BigDecimal("0.9");
>System.out.println(a.add(b));// 1.9
>System.out.println(a.subtract(b));// 0.1
>System.out.println(a.multiply(b));// 0.90
>System.out.println(a.divide(b));// 无法除尽，抛出 ArithmeticException 异常
>System.out.println(a.divide(b, 2, RoundingMode.HALF_UP));// 1.11
>
>```
>
>------
>
>这里需要注意的是，在我们使用 `divide` 方法的时候尽量使用 3 个参数版本，并且`RoundingMode` 不要选择 `UNNECESSARY`，否则很可能会遇到 `ArithmeticException`（无法除尽出现无限循环小数的时候），其中 `scale` 表示要保留几位小数，`roundingMode` 代表保留规则。
>
>```java
>public BigDecimal divide(BigDecimal divisor, int scale, RoundingMode roundingMode) {
>    return divide(divisor, scale, roundingMode.oldMode);
>}
>
>```
>
>保留规则
>
>```java
>public enum RoundingMode {
>   // 2.5 -> 3 , 1.6 -> 2
>   // -1.6 -> -2 , -2.5 -> -3
>			 UP(BigDecimal.ROUND_UP),
>   // 2.5 -> 2 , 1.6 -> 1
>   // -1.6 -> -1 , -2.5 -> -2
>			 DOWN(BigDecimal.ROUND_DOWN),
>			 // 2.5 -> 3 , 1.6 -> 2
>   // -1.6 -> -1 , -2.5 -> -2
>			 CEILING(BigDecimal.ROUND_CEILING),
>			 // 2.5 -> 2 , 1.6 -> 1
>   // -1.6 -> -2 , -2.5 -> -3
>			 FLOOR(BigDecimal.ROUND_FLOOR),
>   	// 2.5 -> 3 , 1.6 -> 2
>   // -1.6 -> -2 , -2.5 -> -3
>			 HALF_UP(BigDecimal.ROUND_HALF_UP),
>   //......
>}
>```

> - 大小比较
>
> `a.compareTo(b)` : 返回 -1 表示 `a` 小于 `b`，0 表示 `a` 等于 `b` ， 1 表示 `a` 大于 `b`。
>
> ```java
> BigDecimal a = new BigDecimal("1.0");
> BigDecimal b = new BigDecimal("0.9");
> System.out.println(a.compareTo(b));// 1
> ```

> - 保留几位小数
>
> 通过 `setScale`方法设置保留几位小数以及保留规则。保留规则有挺多种，不需要记，IDEA 会提示。
>
> ```java
> BigDecimal m = new BigDecimal("1.255433");
> BigDecimal n = m.setScale(3,RoundingMode.HALF_DOWN);
> System.out.println(n);// 1.255
> ```



**BigDecimal 等值比较问题**

`BigDecimal` 使用 `equals()` 方法进行等值比较不仅仅会比较值的大小（value）还会比较精度（scale），而 `compareTo()` 方法比较的时候会忽略精度。1.0 的 scale 是 1，1 的 scale 是 0，因此 `a.equals(b)` 的结果是 false。

`compareTo()` 方法可以比较两个 `BigDecimal` 的值，如果相等就返回 0，如果第 1 个数比第 2 个数大则返回 1，反之返回-1。

```java
BigDecimal a = new BigDecimal("1");
BigDecimal b = new BigDecimal("1.0");
System.out.println(a.compareTo(b));//0
```



**BigDecimal工具类**

```java
import java.math.BigDecimal;
import java.math.RoundingMode;

/**
 * 简化BigDecimal计算的小工具类
 */
public class BigDecimalUtil {

    /**
     * 默认除法运算精度
     */
    private static final int DEF_DIV_SCALE = 10;

    private BigDecimalUtil() {
    }

    /**
     * 提供精确的加法运算。
     *
     * @param v1 被加数
     * @param v2 加数
     * @return 两个参数的和
     */
    public static double add(double v1, double v2) {
        BigDecimal b1 = BigDecimal.valueOf(v1);
        BigDecimal b2 = BigDecimal.valueOf(v2);
        return b1.add(b2).doubleValue();
    }

    /**
     * 提供精确的减法运算。
     *
     * @param v1 被减数
     * @param v2 减数
     * @return 两个参数的差
     */
    public static double subtract(double v1, double v2) {
        BigDecimal b1 = BigDecimal.valueOf(v1);
        BigDecimal b2 = BigDecimal.valueOf(v2);
        return b1.subtract(b2).doubleValue();
    }

    /**
     * 提供精确的乘法运算。
     *
     * @param v1 被乘数
     * @param v2 乘数
     * @return 两个参数的积
     */
    public static double multiply(double v1, double v2) {
        BigDecimal b1 = BigDecimal.valueOf(v1);
        BigDecimal b2 = BigDecimal.valueOf(v2);
        return b1.multiply(b2).doubleValue();
    }

    /**
     * 提供（相对）精确的除法运算，当发生除不尽的情况时，精确到
     * 小数点以后10位，以后的数字四舍五入。
     *
     * @param v1 被除数
     * @param v2 除数
     * @return 两个参数的商
     */
    public static double divide(double v1, double v2) {
        return divide(v1, v2, DEF_DIV_SCALE);
    }

    /**
     * 提供（相对）精确的除法运算。当发生除不尽的情况时，由scale参数指
     * 定精度，以后的数字四舍五入。
     *
     * @param v1    被除数
     * @param v2    除数
     * @param scale 表示表示需要精确到小数点以后几位。
     * @return 两个参数的商
     */
    public static double divide(double v1, double v2, int scale) {
        if (scale < 0) {
            throw new IllegalArgumentException(
                    "The scale must be a positive integer or zero");
        }
        BigDecimal b1 = BigDecimal.valueOf(v1);
        BigDecimal b2 = BigDecimal.valueOf(v2);
        return b1.divide(b2, scale, RoundingMode.HALF_UP).doubleValue();
    }

    /**
     * 提供精确的小数位四舍五入处理。
     *
     * @param v     需要四舍五入的数字
     * @param scale 小数点后保留几位
     * @return 四舍五入后的结果
     */
    public static double round(double v, int scale) {
        if (scale < 0) {
            throw new IllegalArgumentException(
                    "The scale must be a positive integer or zero");
        }
        BigDecimal b = BigDecimal.valueOf(v);
        BigDecimal one = new BigDecimal("1");
        return b.divide(one, scale, RoundingMode.HALF_UP).doubleValue();
    }

    /**
     * 提供精确的类型转换(Float)
     *
     * @param v 需要被转换的数字
     * @return 返回转换结果
     */
    public static float convertToFloat(double v) {
        BigDecimal b = new BigDecimal(v);
        return b.floatValue();
    }

    /**
     * 提供精确的类型转换(Int)不进行四舍五入
     *
     * @param v 需要被转换的数字
     * @return 返回转换结果
     */
    public static int convertsToInt(double v) {
        BigDecimal b = new BigDecimal(v);
        return b.intValue();
    }

    /**
     * 提供精确的类型转换(Long)
     *
     * @param v 需要被转换的数字
     * @return 返回转换结果
     */
    public static long convertsToLong(double v) {
        BigDecimal b = new BigDecimal(v);
        return b.longValue();
    }

    /**
     * 返回两个数中大的一个值
     *
     * @param v1 需要被对比的第一个数
     * @param v2 需要被对比的第二个数
     * @return 返回两个数中大的一个值
     */
    public static double returnMax(double v1, double v2) {
        BigDecimal b1 = new BigDecimal(v1);
        BigDecimal b2 = new BigDecimal(v2);
        return b1.max(b2).doubleValue();
    }

    /**
     * 返回两个数中小的一个值
     *
     * @param v1 需要被对比的第一个数
     * @param v2 需要被对比的第二个数
     * @return 返回两个数中小的一个值
     */
    public static double returnMin(double v1, double v2) {
        BigDecimal b1 = new BigDecimal(v1);
        BigDecimal b2 = new BigDecimal(v2);
        return b1.min(b2).doubleValue();
    }

    /**
     * 精确对比两个数字
     *
     * @param v1 需要被对比的第一个数
     * @param v2 需要被对比的第二个数
     * @return 如果两个数一样则返回0，如果第一个数比第二个数大则返回1，反之返回-1
     */
    public static int compareTo(double v1, double v2) {
        BigDecimal b1 = BigDecimal.valueOf(v1);
        BigDecimal b2 = BigDecimal.valueOf(v2);
        return b1.compareTo(b2);
    }

}
```





#### 超过 long 整型的数据应该如何表示？

`BigInteger` 内部使用 `int[]` 数组来存储任意大小的整形数据。

相对于常规整数类型的运算来说，`BigInteger` 运算的效率会相对较低。



### 变量

#### 成员变量与局部变量的区别？

|          | 成员变量                                                     | 局部变量                                                     |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 定义位置 | 属于类（类中方法外）                                         | 代码块、方法内的变量、方法的参数                             |
| 修饰符   | public、private、static、final                               | final                                                        |
| 存储方式 | 静态成员变量（类变量）：方法区<br />非静态成员变量（实例变量）：堆内存 | 栈内存                                                       |
| 生存时间 | 实例变量随着对象的创建而诞生，随着对象的消亡而消亡<br />类变量在程序执行期间一直存在，被所有对象实例共享 | 局部变量的生存时间仅限于它们所在的作用域，当作用域结束时销毁 |
| 默认值   | 从变量是否有默认值来看，成员变量如果没有被赋初始值，则会自动以类型的默认值而赋值（一种情况例外:被 `final` 修饰的成员变量也必须显式地赋值） | 而局部变量则不会自动赋值                                     |



####  静态变量有什么作用？

- 静态变量也就是被 `static` 关键字修饰的变量。它可以被类的所有实例共享，无论一个类创建了多少个对象，它们都共享同一份静态变量。也就是说，静态变量只会被分配一次内存，即使创建多个对象，这样可以节省内存。
- 静态变量是通过类名来访问的（如果被 `private`关键字修饰就无法这样访问了，可以提供暴露的方法进行访问）。
- 通常情况下，静态变量会被 `final` 关键字修饰成为常量。



#### 静态变量和实例变量有什么区别

|              | 静态变量                   | 实例变量                                             |
| ------------ | -------------------------- | ---------------------------------------------------- |
| 定义、初始化 | static修饰、类加载时初始化 | 在类中定义、每个对象有独立的副本，在对象创建时初始化 |
| 存储位置     | 方法区                     | 对象的堆内存中                                       |
| 生命周期     | 和类同步                   | 和对象同步                                           |
| 访问方式     | 通过类名访问               | ，通过对象引用访问                                   |
| 用途         | 与类相关的常量或共享数据   | 存储对象特定的状态信息                               |



####  字符型常量和字符串常量的区别?

- **形式** : 字符常量是单引号引起的一个字符，字符串常量是双引号引起的 0 个或若干个字符。
- **含义** : 字符常量相当于一个整型值( ASCII 值),可以参加表达式运算; 字符串常量代表一个地址值(该字符串在内存中存放位置)。
- **占内存大小**：字符常量只占 2 个字节; 字符串常量占若干个字节。



### 方法

#### 是否可以从一个static方法内部发出对非static方法的调用？

从一个静态方法内部可以调用非静态方法，但前提是必须先创建该类的对象。



#### 什么是方法的返回值?方法有哪几种类型？

**方法的返回值** 是指我们获取到的某个方法体中的代码执行后产生的结果！（前提是方法返回某个值）

按照方法的返回值和参数类型将方法分为下面这几种：

1. 无参数无返回值的方法
2. 有参数无返回值的方法
3. 无参数有返回值的方法
4. 有参数有返回值的方法



#### 静态方法为什么不能调用非静态成员?

1. 静态方法是属于类的，在类加载的时候就会分配内存，可以通过类名直接访问。而非静态成员属于实例对象，只有在对象实例化之后才存在，需要通过类的实例对象去访问。
2. 在类的非静态成员不存在的时候静态方法就已经存在了，此时调用在内存中还不存在的非静态成员，属于非法操作。



#### 静态方法和实例方法有何不同？

1. 调用方式
   - 在外部调用静态方法时，可以使用 `类名.方法名` 的方式，也可以使用 `对象.方法名` 的方式，而实例方法只有后面这种方式。也就是说，**调用静态方法可以无需创建对象** 。
   - 不过，需要注意的是一般不建议使用 `对象.方法名` 的方式来调用静态方法。这种方式非常容易造成混淆，静态方法不属于类的某个对象而是属于这个类。
   - 因此，一般建议使用 `类名.方法名` 的方式来调用静态方法。

2. 访问类成员是否存在限制
   - 静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法）。
   - 不允许访问实例成员（即实例成员变量和实例方法），而实例方法不存在这个限制。



####  重载和重写有什么区别？

| 区别点     | 重载方法 | 重写方法                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| 发生范围   | 同一个类 | 子类                                                         |
| 参数列表   | 必须修改 | 一定不能修改                                                 |
| 返回类型   | 可修改   | 子类方法返回值类型应比父类方法返回值类型更小或相等           |
| 异常       | 可修改   | 子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等 |
| 访问修饰符 | 可修改   | 子类的访问修饰符范围大于等于父类                             |
| 发生阶段   | 编译期   | 运行期                                                       |

> 关于 **重写的返回值类型** 这里需要额外多说明一下，上面的表述不太清晰准确：如果方法的返回类型是 void 和基本数据类型，则返回值重写时不可修改。但是如果方法的返回值是引用类型，重写时是可以返回该引用类型的子类的。



#### 什么是可变长参数？

- 从 Java5 开始，Java 支持定义可变长参数，所谓可变长参数就是允许在调用方法时传入不定长度（0个或者多个）的参数。
- 可变参数只能作为函数的最后一个参数，但其前面可以有也可以没有任何其他参数。
- Java 的可变参数编译后实际会被转换成一个数组。

> 遇到方法重载的情况，优先匹配固定参数的方法，因为固定参数的方法匹配度更高。



### 面向对象基础

#### 面向对象和面向过程的区别

- 面向过程把解决问题的过程拆成一个个方法，通过一个个方法的执行解决问题。
- 面向对象会先抽象出对象，然后用对象执行方法的方式解决问题。

另外，面向对象开发的程序一般更易维护、易复用、易扩展。



#### 创建一个对象用什么运算符-对象实体与对象引用有何不同

new 运算符，new 创建对象实例（对象实例在 内存中），对象引用指向对象实例（对象引用存放在栈内存中）。

- 一个对象引用可以指向 0 个或 1 个对象（一根绳子可以不系气球，也可以系一个气球）。
- 一个对象可以有 n 个引用指向它（可以用 n 条绳子系住一个气球）。



#### 对象的相等和引用相等的区别

- 对象的相等一般比较的是内存中存放的内容是否相等。
- 引用相等一般比较的是他们指向的内存地址是否相等。

```java
// 对象相等与引用相等的示例

String str1 = "hello";
String str2 = new String("hello");
String str3 = "hello";
// 使用 == 比较字符串的引用相等
System.out.println(str1 == str2);
System.out.println(str1 == str3);
// 使用 equals 方法比较字符串的相等
System.out.println(str1.equals(str2));
System.out.println(str1.equals(str3));
```

```
false  // str1指向字符串常量池，str2指向堆内存
true   // str1 str2 都指向了字符串常量池
true   // String.equals比较的是内容
true   // String.equals比较的是内容
```



####  如果一个类没有声明构造方法，该程序能正确执行吗?

构造方法是一种特殊的方法，主要作用是完成对象的初始化工作。

如果一个类没有声明构造方法，也可以执行！因为一个类即使没有声明构造方法也会有默认的不带参数的构造方法。如果我们自己添加了类的构造方法（无论是否有参），Java 就不会添加默认的无参数的构造方法了。



#### 构造方法有哪些特点？是否可被 override?

构造方法特点如下：

- 名字与类名相同。
- 没有返回值，但不能用 void 声明构造函数。
- 创建类的对象时自动执行，无需调用。

构造方法不能被 override（重写）,但是可以 overload（重载），所以你可以看到一个类中有多个构造函数的情况。



#### 面向对象三大特征

- 封装、继承、多态

  > 封装：封装是指把一个对象的状态信息，即属性，隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性。（合理隐藏、合理暴露）

  > 继承：不同类型的对象，相互之间经常有一定数量的共同点。继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类，可以提高代码的重用性，程序的可维护性，节省大量创建新类的时间 ，提高开发效率。
  >
  > ------
  >
  > 继承的注意点：
  >
  > 1. 子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问，**只是拥有**。
  > 2. 子类可以拥有自己属性和方法，即子类可以对父类进行扩展。
  > 3. 子类可以用自己的方式实现父类的方法。
  
  > 多态：表示一个对象具有多种的状态，具体表现为父类的引用指向子类的实例。
  >
  > ------
  >
  > 多态的特点：
  >
  > 1. 对象类型和引用类型之间具有继承（类）/实现（接口）的关系。
  > 2. 引用类型变量发出的方法调用的到底是哪个类中的方法，必须在程序运行期间才能确定。
  > 3. 多态不能调用“只在子类存在但在父类不存在”的方法。
  > 4. 如果子类重写了父类的方法，真正执行的是子类覆盖的方法，如果子类没有覆盖父类的方法，执行的是父类的方法。
  
  

#### 接口和抽象类有什么共同点和区别？

共同点：

- 都不能被实例化。
- 都可以包含抽象方法（接口的方法都是抽象的、抽象类的方法可以包含抽象也可以包含实例方法）。
- 都可以有默认实现的方法（Java 8 可以用 `default` 关键字在接口中定义默认方法）。

区别：

- 接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为。抽象类主要用于代码复用，强调的是所属关系。
- 一个类只能继承一个类，但是可以实现多个接口。
- 接口中的成员变量只能是 `public static final` 类型的，不能被修改且必须有初始值，而抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。



#### 深拷贝和浅拷贝区别了解吗？什么是引用拷贝？

- **浅拷贝**：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。
- **深拷贝**：深拷贝会完全复制整个对象，包括这个对象所包含的内部对象。
- **引用拷贝**：引用拷贝就是两个不同的引用指向同一个对象。

![浅拷贝、深拷贝、引用拷贝示意图](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307101047625.png)



### Object

#### Object 类的常见方法有哪些？

Object 类是一个特殊的类，是所有类的父类。它主要提供了以下 11 个方法：

```java
/**
 * native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
 */
public final native Class<?> getClass();
/**
 * native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
 */
public native int hashCode();
/**
 * 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
 */
public boolean equals(Object obj) { };
/**
 * native 方法，用于创建并返回当前对象的一份拷贝。
 */
protected native Object clone() throws CloneNotSupportedException;
/**
 * 返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
 */
public String toString() { }
/**
 * native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
 */
public final native void notify();
/**
 * native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
 */
public final native void notifyAll();
/**
 * native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
 */
public final native void wait(long timeout) throws InterruptedException;
/**
 * 多了 nanos 参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 毫秒。。
 */
public final void wait(long timeout, int nanos) throws InterruptedException { }
/**
 * 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
 */
public final void wait() throws InterruptedException { }
/**
 * 实例被垃圾回收器回收的时候触发的操作
 */
protected void finalize() throws Throwable { }
```

#### == 和 equals() 的区别

**`==`** 对于基本类型和引用类型的作用效果是不同的：

- 对于基本数据类型来说，`==` 比较的是值。

- 对于引用数据类型来说，`==` 比较的是对象的内存地址。

  > 因为 Java 只有值传递，所以，对于 == 来说，不管是比较基本数据类型，还是引用数据类型的变量，其本质比较的都是值，只是引用类型变量存的值是对象的地址。

  

**`equals()`** 不能用于判断基本数据类型的变量，只能用来判断两个对象是否相等。`equals()`方法存在于`Object`类中，而`Object`类是所有类的直接或间接父类，因此所有的类都有`equals()`方法。

`equals()` 方法存在两种使用情况：

**类没有重写 `equals()`方法**：通过`equals()`比较该类的两个对象时，等价于通过“==”比较这两个对象，使用的默认是 `Object`类`equals()`方法。

**类重写了 `equals()`方法**：一般我们都重写 `equals()`方法来比较两个对象中的属性是否相等；若它们的属性相等，则返回 true(即，认为这两个对象相等)。



`String` 中的 `equals` 方法是被重写过的，因为 `Object` 的 `equals` 方法是比较的对象的内存地址，而 `String` 的 `equals` 方法比较的是对象的值。

当创建 `String` 类型的对象时，虚拟机会在常量池中查找有没有已经存在的值和要创建的值相同的对象，如果有就把它赋给当前引用。如果没有就在常量池中重新创建一个 `String` 对象。



#### hashCode() 有什么用？

`hashCode()` 的作用是获取哈希码（`int` 整数），也称散列码。哈希码的作用是确定该对象在哈希表中的索引位置。

`hashCode()` 定义在 JDK 的 `Object` 类中，这就意味着 Java 中的任何类都包含有 `hashCode()` 函数。另外需要注意的是：`Object` 的 `hashCode()` 方法是本地方法（**native**），也就是用 C 语言或 C++ 实现的。

散列表存储的是键值对(key-value)，它的特点是：**能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！（可以快速找到所需要的对象**



#### 为什么要有 hashCode？

> 以 “`HashSet` 如何检查重复” 为例子来说明为什么要有 `hashCode`？
>
> 
>
> 当你把对象加入 `HashSet` 时，`HashSet` 会先计算对象的 `hashCode` 值来判断对象加入的位置，同时也会与其他已经加入的对象的 `hashCode` 值作比较，如果没有相符的 `hashCode`，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 `hashCode` 值的对象（发生**“哈希碰撞”**），这时会调用 `equals()` 方法来检查 `hashCode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。这样我们就大大减少了 `equals` 的次数，相应就大大提高了执行速度。

其实， `hashCode()` 和 `equals()`都是用于比较两个对象是否相等。

这是因为在一些容器（比如 `HashMap`、`HashSet`）中，有了 `hashCode()` 之后，判断元素是否在对应容器中的效率会更高（参考添加元素进`HashSet`的过程）！

我们在前面也提到了添加元素进`HashSet`的过程，如果 `HashSet` 在对比的时候，同样的 `hashCode` 有多个对象，它会继续使用 `equals()` 来判断是否真的相同。也就是说 `hashCode` 帮助我们大大缩小了查找成本。

**那为什么不只提供 `hashCode()` 方法呢？**

这是因为两个对象的`hashCode` 值相等并不代表两个对象就相等。

**那为什么两个对象有相同的 `hashCode` 值，它们也不一定是相等的？**

因为 `hashCode()` 所使用的哈希算法也许刚好会让多个对象传回相同的哈希值。越糟糕的哈希算法越容易碰撞，但这也与数据值域分布的特性有关（所谓哈希碰撞也就是指的是不同的对象得到相同的 `hashCode` )。

总结下来就是：

- 如果两个对象的`hashCode` 值相等，那这两个对象不一定相等（哈希碰撞）。
- 如果两个对象的`hashCode` 值相等并且`equals()`方法也返回 `true`，我们才认为这两个对象相等。
- 如果两个对象的`hashCode` 值不相等，我们就可以直接认为这两个对象不相等。



#### 为什么重写 equals() 时必须重写 hashCode() 方法？

因为两个相等的对象的 `hashCode` 值必须是相等。也就是说如果 `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。

如果重写 `equals()` 时没有重写 `hashCode()` 方法的话就可能会导致 `equals` 方法判断是相等的两个对象，`hashCode` 值却不相等。

**总结**：

- `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。
- 两个对象有相同的 `hashCode` 值，他们也不一定是相等的（哈希碰撞）。



### String

#### String、StringBuffer、StringBuilder 的区别？

|            |                           String                            |                        StringBuilder                         |                         StringBuffer                         |
| ---------- | :---------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 可变性     |                           不可变                            | `StringBuilder` 与 `StringBuffer` 都继承自 `AbstractStringBuilder` ，在 `AbstractStringBuilder` 中也是使用字符数组保存字符串，不过没有使用 `final` 和 `private` 关键字修饰。`AbstractStringBuilder` 类还提供了很多修改字符串的方法比如 `append` 方法。 |                              /                               |
| 线程安全性 | `String` 中的对象是不可变的，也就可以理解为常量，线程安全。 | `StringBuilder` 并没有对方法进行加同步锁，所以是非线程安全的。 | `StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。 |
| 性能       |                             差                              |               比StringBuffer略高（甚至可忽略）               |                             优秀                             |

**对于三者使用的总结：**

1. 操作少量的数据: 适用 `String`
2. 单线程操作字符串缓冲区下操作大量数据: 适用 `StringBuilder`
3. 多线程操作字符串缓冲区下操作大量数据: 适用 `StringBuffer`



#### String 为什么是不可变的?

`String` 类中使用 `final` 关键字修饰字符数组来保存字符串。

我们知道被 `final` 关键字修饰的类不能被继承、修饰的方法不能被重写、修饰的变量是基本数据类型则值不能改变、修饰的变量是引用类型则不能再指向其他对象。

因此，`final` 关键字修饰的数组保存字符串并不是 `String` 不可变的根本原因，因为这个数组保存的字符串是可变的（`final` 修饰引用类型变量的情况）。

`String` 真正不可变有下面几点原因：

1. 保存字符串的数组被 `final` 修饰且为私有的，并且`String` 类没有提供/暴露修改这个字符串的方法。
2. `String` 类被 `final` 修饰导致其不能被继承，进而避免了子类破坏 `String` 不可变。

> 在 Java 9 之后，`String`、`StringBuilder` 与 `StringBuffer` 的实现改用 `byte` 数组存储字符串。
>
> ------
>
> **Java 9 为何要将 `String` 的底层实现由 `char[]` 改成了 `byte[]` ?**
>
> 新版的 String 其实支持两个编码方案：Latin-1 和 UTF-16。如果字符串中包含的汉字没有超过 Latin-1 可表示范围内的字符，那就会使用 Latin-1 作为编码方案。Latin-1 编码方案下，`byte` 占一个字节(8 位)，`char` 占用 2 个字节（16），`byte` 相较 `char` 节省一半的内存空间。
>
> JDK 官方就说了绝大部分字符串对象只包含 Latin-1 可表示的字符。
>
> 如果字符串中包含的汉字超过 Latin-1 可表示范围内的字符，`byte` 和 `char` 所占用的空间是一样的。



#### 字符串拼接用“+” 还是 StringBuilder?

- Java 语言本身并不支持运算符重载，“+”和“+=”是专门为 String 类重载过的运算符，也是 Java 中仅有的两个重载过的运算符。

- 字符串对象通过“+”的字符串拼接方式，实际上是通过 `StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个 `String` 对象 。

- 不过，在循环内使用“+”进行字符串的拼接的话，存在比较明显的缺陷：**编译器不会创建单个 `StringBuilder` 以复用，会导致创建过多的 `StringBuilder` 对象**。

- `StringBuilder` 对象是在循环内部被创建的，这意味着每循环一次就会创建一个 `StringBuilder` 对象。

- 如果直接使用 `StringBuilder` 对象进行字符串拼接的话，就不会存在这个问题了。

  > 不过，使用 “+” 进行字符串拼接会产生大量的临时对象的问题在 JDK9 中得到了解决。在 JDK9 当中，字符串相加 “+” 改为了用动态方法 `makeConcatWithConstants()` 来实现，而不是大量的 `StringBuilder` 了。



#### String#equals() 和 Object#equals() 有何区别？

- `String` 中的 `equals` 方法是被重写过的，比较的是 String 字符串的值是否相等。 

- `Object` 的 `equals` 方法是比较的对象的内存地址。



#### 字符串常量池的作用了解吗？

**字符串常量池** 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。



#### String s1 = new String("abc");这句话创建了几个字符串对象？

会创建 1 或 2 个字符串对象。

1. 如果字符串常量池中不存在字符串对象“abc”的引用，那么它将首先在字符串常量池中创建，然后在堆空间中创建，因此将创建总共 2 个字符串对象。
2. 如果字符串常量池中已存在字符串对象“abc”的引用，则只会在堆中创建 1 个字符串对象“abc”。



#### String#intern 方法有什么作用?

`String.intern()` 是一个 native（本地）方法，其作用是将指定的字符串对象的引用保存在字符串常量池中，可以简单分为两种情况：

- 如果字符串常量池中保存了对应的字符串对象的引用，就直接返回该引用。
- 如果字符串常量池中没有保存了对应的字符串对象的引用，那就在常量池中创建一个指向该字符串对象的引用并返回

```java
// 在堆中创建字符串对象”Java“
// 将字符串对象”Java“的引用保存在字符串常量池中
String s1 = "Java";
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s2 = s1.intern();
// 会在堆中在单独创建一个字符串对象
String s3 = new String("Java");
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s4 = s3.intern();
// s1 和 s2 指向的是堆中的同一个对象
System.out.println(s1 == s2); // true
// s3 和 s4 指向的是堆中不同的对象
System.out.println(s3 == s4); // false
// s1 和 s4 指向的是堆中的同一个对象
System.out.println(s1 == s4); //true
```



#### String 类型的变量和常量做“+”运算时发生了什么？

**对于编译期可以确定值的字符串，也就是常量字符串 ，jvm 会将其存入字符串常量池。并且，字符串常量拼接得到的字符串常量在编译阶段就已经被存放字符串常量池，这个得益于编译器的优化。**

在编译过程中，Javac 编译器（下文中统称为编译器）会进行一个叫做 **常量折叠(Constant Folding)** 的代码优化。

> 常量折叠会把常量表达式的值求出来作为常量嵌在最终生成的代码中，这是 Javac 编译器会对源代码做的极少量优化措施之一(代码优化几乎都在即时编译器中进行)。

并不是所有的常量都会进行折叠，只有编译器在程序编译期就可以确定值的常量才可以：

- 基本数据类型( `byte`、`boolean`、`short`、`char`、`int`、`float`、`long`、`double`)以及字符串常量。
- `final` 修饰的基本数据类型和字符串变量。
- 字符串通过 “+”拼接得到的字符串、基本数据类型之间算数运算（加减乘除）、基本数据类型的位运算（<<、>>、>>> ）。

**引用的值在程序编译期是无法确定的，编译器无法对其进行优化。**

> 对象引用和“+”的字符串拼接方式，实际上是通过 `StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个 `String` 对象 。
>
> ------
>
> 我们在平时写代码的时候，尽量避免多个字符串对象拼接，因为这样会重新创建对象。如果需要改变字符串的话，可以使用 `StringBuilder` 或者 `StringBuffer`。
>
> 不过，字符串使用 `final` 关键字声明之后，可以让编译器当做常量来处理。
>
> 被 `final` 关键字修改之后的 `String` 会被编译器当做常量来处理，编译器在程序编译期就可以确定它的值，其效果就相当于访问常量。
>
> 如果 ，编译器在运行时才能知道其确切值的话，就无法对其优化。



### 异常

#### Exception 和 Error 有什么区别？

在 Java 中，所有的异常都有一个共同的祖先 `java.lang` 包中的 `Throwable` 类。`Throwable` 类有两个重要的子类:

- **`Exception`** ：程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 Checked Exception (受检查异常，必须处理) 和 Unchecked Exception (不受检查异常，可以不处理)。
- **`Error`**：`Error` 属于程序无法处理的错误 ，无法通过 `catch` 来进行捕获不建议通过`catch`捕获 。例如 Java 虚拟机运行错误（`Virtual MachineError`）、虚拟机内存不够错误(`OutOfMemoryError`)、类定义错误（`NoClassDefFoundError`）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。

![types-of-exceptions-in-java](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307110853113.png)

#### Checked Exception 和 Unchecked Exception 有什么区别？

**Checked Exception** 即 受检查异常 ，Java 代码在编译过程中，如果受检查异常没有被 `catch`或者`throws` 关键字处理的话，就没办法通过编译。

除了`RuntimeException`及其子类以外，其他的`Exception`类及其子类都属于受检查异常 。常见的受检查异常有：IO 相关的异常、`ClassNotFoundException`、`SQLException`等。



**Unchecked Exception** 即 **不受检查异常** ，Java 代码在编译过程中 ，不处理不受检查异常也可以正常通过编译。

`RuntimeException` 及其子类都统称为非受检查异常，常见的有：

- `NullPointerException`(空指针错误)
- `IllegalArgumentException`(参数错误比如方法入参类型错误)
- `NumberFormatException`（字符串转换为数字格式错误）
- `ArrayIndexOutOfBoundsException`（数组越界错误）
- `ClassCastException`（类型转换错误）
- `ArithmeticException`（算术错误）
- `SecurityException` （安全错误比如权限不够）
- `UnsupportedOperationException`(不支持的操作错误比如重复创建同一用户)



#### Throwable 类常用方法有哪些？

- `String getMessage()`: 返回异常发生时的简要描述
- `String toString()`: 返回异常发生时的详细信息
- `String getLocalizedMessage()`: 返回异常对象的本地化信息。使用 `Throwable` 的子类覆盖这个方法，可以生成本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与 `getMessage()`返回的结果相同
- `void printStackTrace()`: 在控制台上打印 `Throwable` 对象封装的异常信息



#### try-catch-finally 如何使用？

`try`块：用于捕获异常。其后可接零个或多个 `catch` 块，如果没有 `catch` 块，则必须跟一个 `finally` 块。

`catch`块：用于处理 try 捕获到的异常。

`finally` 块：无论是否捕获或处理异常，`finally` 块里的语句都会被执行。

>  当在 `try` 块或 `catch` 块中遇到 `return` 语句时，`finally` 语句块将在方法返回之前被执行。
>
> **注意：不要在 finally 语句块中使用 return!** 当 try 语句和 finally 语句中都有 return 语句时，try 语句块中的 return 语句会被忽略。这是因为 try 语句中的 return 返回值会先被暂存在一个本地变量中，当执行到 finally 语句中的 return 之后，这个本地变量的值就变为了 finally 语句中的 return 返回值。



#### finally 中的代码一定会执行吗？

不一定的！在某些情况下，finally 中的代码不会被执行。

```java
// 在finally语句块之前JVM终止

try {
    System.out.println("Try to do something");
    throw new RuntimeException("RuntimeException");
} catch (Exception e) {
    System.out.println("Catch Exception -> " + e.getMessage());
    // 终止当前正在运行的Java虚拟机
    System.exit(1);
} finally {
    System.out.println("Finally");
}
```

```
Try to do something
Catch Exception -> RuntimeException
```

在以下 3 种特殊情况下，`finally` 块的代码不会被执行：

1. 程序所在的线程死亡。
2. 关闭 CPU。
3. finally语句块之前虚拟机被终止。



#### 如何使用 `try-with-resources` 代替`try-catch-finally`？

**适用范围（资源的定义）：** 任何实现 `java.lang.AutoCloseable`或者 `java.io.Closeable` 的对象

**关闭资源和 finally 块的执行顺序：** 在 `try-with-resources` 语句中，任何 catch 或 finally 块在声明的资源关闭后运行

> 把`资源对象`完整的提取到`try()块`的`()`内，多个资源对象使用逗号`,`分隔，最后一个资源对象使用分号`;`结束



#### 异常使用有哪些需要注意的地方？

- 不要把异常定义为静态变量，因为这样会导致异常栈信息错乱。每次手动抛出异常，我们都需要手动 new 一个异常对象抛出。

- 抛出的异常信息一定要有意义。

- 建议抛出更加具体的异常比如字符串转换为数字格式错误的时候应该抛出`NumberFormatException`而不是其父类`IllegalArgumentException`。

- 使用日志打印异常之后就不要再抛出异常了（两者不要同时存在一段代码逻辑中）。



### 泛型

#### 什么是泛型？有什么作用？

- **Java 泛型（Generics）** 是 JDK 5 中引入的一个新特性。使用泛型参数，可以增强代码的可读性以及稳定性。
- 编译器可以对泛型参数进行检测，并且通过泛型参数可以指定传入的对象类型。



#### 泛型的使用方式有哪几种？

泛型一般有三种使用方式:**泛型类**、**泛型接口**、**泛型方法**。

1. 泛型类

   ```java
   //此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
   //在实例化泛型类时，必须指定T的具体类型
   public class Generic<T>{
   
       private T key;
   
       public Generic(T key) {
           this.key = key;
       }
   
       public T getKey(){
           return key;
       }
   }
   ```

   如何实例化泛型类

   ```java
   Generic<Integer> genericInteger = new Generic<Integer>(123456);
   ```

2. 泛型接口

   ```java
   public interface Generator<T> {
       public T method();
   }
   ```

   实现泛型接口，不指定类型：

   ```java
   class GeneratorImpl<T> implements Generator<T>{
       @Override
       public T method() {
           return null;
       }
   }
   ```

   实现泛型接口，指定类型：

   ```java
   class GeneratorImpl<T> implements Generator<String>{
       @Override
       public String method() {
           return "hello";
       }
   }
   ```

3. 泛型方法

   ```java
      public static < E > void printArray( E[] inputArray )
      {
            for ( E element : inputArray ){
               System.out.printf( "%s ", element );
            }
            System.out.println();
       }
   
   ```

   使用：

   ```java
   // 创建不同类型数组：Integer, Double 和 Character
   Integer[] intArray = { 1, 2, 3 };
   String[] stringArray = { "Hello", "World" };
   printArray( intArray  );
   printArray( stringArray  );
   ```

   > 注意: `public static < E > void printArray( E[] inputArray )` 一般被称为静态泛型方法;在 java 中泛型只是一个占位符，必须在传递类型后才能使用。类在实例化时才能真正的传递类型参数，由于静态方法的加载先于类的实例化，也就是说类中的泛型还没有传递真正的类型参数，静态的方法的加载就已经完成了，所以静态泛型方法是没有办法使用类上声明的泛型的。只能使用自己声明的 `<E>`



#### 什么是泛型擦除？为什么要擦除

**Java 的泛型是伪泛型，这是因为 Java 在编译期间，所有的泛型信息都会被擦掉，这也就是通常所说类型擦除 。**

编译器会在编译期间会动态地将泛型 T 擦除为 Object 或将 T extends xxx 擦除为其限定类型 xxx 。

因此，泛型本质上其实还是编译器的行为，为了保证引入泛型机制但不创建新的类型，减少虚拟机的运行开销，编译器通过擦除将泛型类转化为一般类。



举例说明：

```java
List<Integer> list = new ArrayList<>();

list.add(12);
//1.编译期间直接添加会报错
list.add("a");
Class<? extends List> clazz = list.getClass();
Method add = clazz.getDeclaredMethod("add", Object.class);
//2.运行期间通过反射添加，是可以的
add.invoke(list, "kl");

System.out.println(list)
```

```java
//由于泛型擦除的问题，下面的方法重载会报错。
public void print(List<String> list)  { }
public void print(List<Integer> list) { }
```

既然编译器要把泛型擦除，那为什么还要用泛型呢？用 Object 代替不行吗？ 

> 1. 使用泛型可在编译期间进行类型检测。 
> 2. 使用 Object 类型需要手动添加强制类型转换，降低代码可读性，提高出错概率。
> 3. 泛型可以使用自限定类型如 T extends Comparable 。



#### 什么是桥方法

桥方法(Bridge Method) 用于继承泛型类时保证多态。

```java
class Node<T> {
    public T data;
    
    public Node(T data) { 
        this.data = data; 
    }
    
    public void setData(T data) {
        System.out.println("Node.setData");
        this.data = data;
    }
}

class MyNode extends Node<Integer> {
    public MyNode(Integer data) { 
        super(data); 
    }

  	// Node<T> 泛型擦除后为 setData(Object data)，而子类 MyNode 中并没有重写该方法，所以编译器会加入该桥方法保证多态
   	public void setData(Object data) {
        setData((Integer) data);
    }

    public void setData(Integer data) {
        System.out.println("MyNode.setData");
        super.setData(data);
    }
}
```

⚠️注意 ：桥方法为编译器自动生成，非手写。


泛型有哪些限制？为什么？

泛型的限制一般是由泛型擦除机制导致的。擦除为 Object 后无法进行类型判断

- 只能声明不能实例化 T 类型变量。
- 泛型参数不能是基本类型。因为基本类型不是 Object 子类，应该用基本类型对应的包装类型代替。
- 不能实例化泛型参数的数组。擦除后为 Object 后无法进行类型判断。
- 不能实例化泛型数组。
- 泛型无法使用 Instance of 和 getClass() 进行类型判断。
- 不能实现两个不同泛型参数的同一接口，擦除后多个父类的桥方法将冲突
- 不能使用 static 修饰泛型变量



以下代码是否能通过编译，为什么？

```java
public final class Algorithm {
    public static <T> T max(T x, T y) {
        return x > y ? x : y;
    }
}
```

无法编译，因为 x 和 y 都会被擦除为` Object` 类型，` Object `无法使用` > `进行比较

```java
public class Singleton<T> {

    public static T getInstance() {
        if (instance == null)
            instance = new Singleton<T>();

        return instance;
    }

    private static T instance = null;
}
```

无法编译，因为不能使用 `static `修饰泛型` T` 。



#### 什么是通配符？有什么用？

泛型类型是固定的，某些场景下使用起来不太灵活，通配符可以允许类型参数变化，用来解决泛型无法协变的问题。

```java
// 限制类型为 Person 的子类
<? extends Person>
// 限制类型为 Manager 的父类
<? super Manager>
```



#### 通配符 ？和常用的泛型 T 之间有什么区别？

- T 可以用于声明变量或常量而 ? 不行。
- T 一般用于声明泛型类或方法，通配符 ? 一般用于泛型方法的调用代码和形参。
- T 在编译期会被擦除为限定类型或 Object，通配符用于捕获具体类型。



#### 什么是无界通配符

无界通配符可以接收任何泛型类型数据，用于实现不依赖于具体类型参数的简单方法，可以捕获参数类型并交由泛型方法进行处理。

```java
void testMethod(Person<?> p) {  
    // 泛型方法自行处理
}
```



#### List<?> 和 List 有区别吗？

- List<?> list 表示 list 是持有某种特定类型的 List，但是不知道具体是哪种类型。因此，我们添加元素进去的时候会报错。

- List list 表示 list 是持有的元素的类型是 Object，因此可以添加任何类型的对象，只不过编译器会有警告信息。

  ```java
  List<?> list = new ArrayList<>();
  list.add("sss");//报错
  List list2 = new ArrayList<>();
  list2.add("sss");//警告信息
  ```



#### 什么是上边界通配符、下边界通配符？

在使用泛型的时候，我们还可以为传入的泛型类型实参进行上下边界的限制，如：**类型实参只准传入某种类型的父类或某种类型的子类。**

**上边界通配符 extends** 可以实现泛型的向上转型即传入的类型实参必须是指定类型的子类型。



举个例子：

```java
// 限制必须是 Person 类的子类
<? extends Person>
```

类型边界可以设置多个，还可以对 T 类型进行限制。

```java
<T extends T1 & T2>
<T extends XXX>
```

**下边界通配符 super** 与上边界通配符 extends刚好相反，它可以实现泛型的向下转型即传入的类型实参必须是指定类型的父类型。

举个例子：

```java
//  限制必须是 Employee 类的父类
List<? super Employee>
```

**`? extends xxx `和` ? super xxx `有什么区别?**

两者接收参数的范围不同。并且，使用 ? extends xxx 声明的泛型参数只能调用 get() 方法返回 xxx 类型，调用 set() 报错。使用 ? super xxx 声明的泛型参数只能调用 set() 方法接收 xxx 类型，调用 get() 报错。



**`T extends xxx` 和` ? extends xxx `又有什么区别？**

T extends xxx 用于定义泛型类和方法，擦除后为 xxx 类型， ? extends xxx 用于声明方法形参，接收 xxx 和其子类型。



**`Class<?>` 和` Class `的区别？**

直接使用 Class 的话会有一个类型警告，使用 Class<?> 则没有，因为 Class 是一个泛型类，接收原生类型会产生警告



以下代码是否能编译，为什么？

```java
class Shape { /* ... */ }
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }

class Node<T> { /* ... */ }

Node<Circle> nc = new Node<>();
Node<Shape>  ns = nc;
```

不能，因为`Node<Circle> `不是 `Node<Shape>` 的子类

```java
class Shape { /* ... */ }
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }

class Node<T> { /* ... */ }
class ChildNode<T> extends Node<T>{

}
ChildNode<Circle> nc = new ChildNode<>();
Node<Circle>  ns = nc;
```

可以编译，`ChildNode<Circle>` 是 `Node<Circle>` 的子类

```java
public static void print(List<? extends Number> list) {
    for (Number n : list)
        System.out.print(n + " ");
    System.out.println();
}
```

可以编译，`List<? extends Number> `可以往外取元素，但是无法调用` add() `添加元素。



#### 当泛型遇到重载

```java
public class GenericTypes {

    public static void method(List<String> list) {
        System.out.println("invoke method(List<String> list)");
    }

    public static void method(List<Integer> list) {
        System.out.println("invoke method(List<Integer> list)");
    }
}
```

上面这段代码，有两个重载的函数，因为他们的参数类型不同，一个是`List<String>`另一个是`List<Integer>` ，但是，这段代码是编译通不过的。参数`List<Integer>`和`List<String>`编译之后都被擦除了，变成了一样的原生类型 List，擦除动作导致这两个方法的特征签名变得一模一样。



#### 当泛型遇到 catch

泛型的类型参数不能用在 Java 异常处理的 catch 语句中。因为异常处理是由 JVM 在运行时刻来进行的。由于类型信息被擦除，JVM 是无法区分两个异常类型`MyException<String>`和`MyException<Integer>`的



#### 当泛型内包含静态变量

```java
public class StaticTest{
    public static void main(String[] args){
        GT<Integer> gti = new GT<Integer>();
        gti.var=1;
        GT<String> gts = new GT<String>();
        gts.var=2;
        System.out.println(gti.var);  // 输出 2 
    }
}
class GT<T>{
    public static int var=0;
    public void nothing(T x){}
}
```

由于经过类型擦除，所有的泛型类实例都关联到同一份字节码上，泛型类的所有静态变量是共享的。



#### 项目中哪里用到了泛型？

- 自定义接口通用返回结果 `CommonResult<T>` 通过参数 `T` 可根据具体的返回类型动态指定结果的数据类型。
- 定义 `Excel` 处理类 `ExcelUtil<T>` 用于动态指定 `Excel` 导出的数据类型。
- 构建集合工具类（参考 `Collections` 中的 `sort`, `binarySearch` 方法）。



### 反射

#### 何谓反射？

通过反射你可以获取任意一个类的所有属性和方法，还可以调用这些方法和属性。



#### 反射的优缺点？

- 反射可以让我们的代码更加灵活、为各种框架提供开箱即用的功能提供了便利。

- 不过，反射让我们在运行时有了分析操作类的能力的同时，也增加了安全问题，比如可以无视泛型参数的安全检查（泛型参数的安全检查发生在编译时）。另外，反射的性能也要稍差点，只是，对于框架来说实际是影响不大的。



#### 反射的应用场景？

- **框架中也大量使用了动态代理，而动态代理的实现依赖反射**

- **注解** 的实现也用到了反射



#### 获取Class对象的四种方式

1. 知道具体类的情况下可以使用

   ```java
   Class alunbarClass = TargetObject.class;
   ```

2. 通过`Class.forName()`传入类的全路径获取

   ```java
   Class alunbarClass1 = Class.forName("cn.javaguide.TargetObject");
   ```

3. 通过对象实例`instance.getClass()`获取：

   ```java
   TargetObject o = new TargetObject();
   Class alunbarClass2 = o.getClass();
   ```

4. 通过类加载器`xxxClassLoader.loadClass()`传入类路径获取

   ```java
   ClassLoader.getSystemClassLoader().loadClass("cn.javaguide.TargetObject");
   ```

   > 通过类加载器获取 Class 对象不会进行初始化，意味着不进行包括初始化等一系列步骤，静态代码块和静态对象不会得到执行



#### 反射的一些基本操作

1. 创建一个我们要使用反射操作的类 `TargetObject`。

   ```java
   public class TargetObject {
       private String value;
   
       public TargetObject() {
           value = "JavaGuide";
       }
   
       public void publicMethod(String s) {
           System.out.println("I love " + s);
       }
   
       private void privateMethod() {
           System.out.println("value is " + value);
       }
   }
   ```

2. 使用反射操作这个类的方法以及参数。

   ```java
   public class Main {
       public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InstantiationException, InvocationTargetException, NoSuchFieldException {
           /**
            * 获取 TargetObject 类的 Class 对象并且创建 TargetObject 类实例
            */
           Class<?> targetClass = Class.forName("com.zxj.TargetObject");
           TargetObject targetObject = (TargetObject) targetClass.newInstance();
           /**
            * 获取 TargetObject 类中定义的所有方法
            */
           Method[] methods = targetClass.getDeclaredMethods();
           for (Method method : methods) {
               System.out.println(method.getName());
           }
   
           /**
            * 获取指定方法并调用
            */
           Method publicMethod = targetClass.getDeclaredMethod("publicMethod",
                   String.class);
   
           publicMethod.invoke(targetObject, "zxj");
   
           /**
            * 获取指定参数并对参数进行修改
            */
           Field field = targetClass.getDeclaredField("value");
           //为了对类中的参数进行修改我们取消安全检查
           field.setAccessible(true);
           field.set(targetObject, "zxj");
   
           /**
            * 调用 private 方法
            */
           Method privateMethod = targetClass.getDeclaredMethod("privateMethod");
           //为了调用private方法我们取消安全检查
           privateMethod.setAccessible(true);
           privateMethod.invoke(targetObject);
       }
   }
   ```

   ```
   publicMethod
   privateMethod
   I love JavaGuide
   value is JavaGuide
   ```
   
   



### 注解

#### 何谓注解？

`Annotation` （注解） 是 Java5 开始引入的新特性，可以看作是一种特殊的注释，主要用于修饰类、方法或者变量，提供某些信息供程序在编译或者运行时使用。

注解本质是一个继承了`Annotation` 的特殊接口：



#### 注解的解析方法有哪几种？

注解只有被解析之后才会生效，常见的解析方法有两种：

- **编译期直接扫描**：编译器在编译 Java 代码的时候扫描对应的注解并处理，比如某个方法使用`@Override` 注解，编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法。
- **运行期通过反射处理**：像框架中自带的注解(比如 Spring 框架的 `@Value`、`@Component`)都是通过反射来进行处理的。



### SPI

#### 何谓 SPI?

SPI 即 Service Provider Interface ，字面意思就是：“服务提供者的接口”，我的理解是：专门提供给服务提供者或者扩展框架功能的开发者去使用的一个接口。

SPI 将服务接口和具体的服务实现分离开来，将服务调用方和服务实现者解耦，能够提升程序的扩展性、可维护性。修改或者替换服务实现并不需要修改调用方。

很多框架都使用了 Java 的 SPI 机制，比如：Spring 框架、数据库加载驱动、日志接口、以及 Dubbo 的扩展实现等等。



#### SPI 和 API 有什么区别？

一般模块之间都是通过接口进行通讯，那我们在服务调用方和服务实现方（也称服务提供者）之间引入一个“接口”。

当实现方提供了接口和实现，可以通过调用实现方的接口从而拥有实现方提供的能力，这就是 API ，这种接口和实现都是放在实现方的。

当接口存在于调用方这边时，就是 SPI ，由接口调用方确定接口规则，然后由不同的厂商去根据这个规则对这个接口进行实现，从而提供服务。

<img src="https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307111531283.png" alt="SPIAPI" style="zoom:50%;" />



#### SPI 的优缺点？

通过 SPI 机制能够大大地提高接口设计的灵活性，但是 SPI 机制也存在一些缺点，比如：

- 需要遍历加载所有的实现类，不能做到按需加载，这样效率还是相对较低的。
- 当多个 `ServiceLoader` 同时 `load` 时，会有并发问题。



#### 实战演示

SLF4J （Simple Logging Facade for Java）是 Java 的一个日志门面（接口），其具体实现有几种，比如：Logback、Log4j、Log4j2 等等，而且还可以切换，在切换日志具体实现的时候我们是不需要更改项目代码的，只需要在 Maven 依赖里面修改一些 pom 依赖就好了。

![image-20220723213306039-165858318917813](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307152101209.png)

这就是依赖 SPI 机制实现的，那我们接下来就实现一个简易版本的日志框架。

#### Service Provider Interface

新建一个 Java 项目 `service-provider-interface` 目录结构如下：（注意直接新建 Java 项目就好了，不用新建 Maven 项目，Maven 项目会涉及到一些编译配置，如果有私服的话，直接 deploy 会比较方便，但是没有的话，在过程中可能会遇到一些奇怪的问题。）

```text
│  service-provider-interface.iml
│
├─.idea
│  │  .gitignore
│  │  misc.xml
│  │  modules.xml
│  └─ workspace.xml
│
└─src
    └─edu
        └─jiangxuan
            └─up
                └─spi
                        Logger.java
                        LoggerService.java
                        Main.class
```

新建 `Logger` 接口，这个就是 SPI ， 服务提供者接口，后面的服务提供者就要针对这个接口进行实现。

```java
package edu.jiangxuan.up.spi;

public interface Logger {
    void info(String msg);
    void debug(String msg);
}
```

接下来就是 `LoggerService` 类，这个主要是为服务使用者（调用方）提供特定功能的。这个类也是实现 Java SPI 机制的关键所在，如果存在疑惑的话可以先往后面继续看。

```java
package edu.jiangxuan.up.spi;

import java.util.ArrayList;
import java.util.List;
import java.util.ServiceLoader;

public class LoggerService {
    private static final LoggerService SERVICE = new LoggerService();

    private final Logger logger;

    private final List<Logger> loggerList;

    private LoggerService() {
        ServiceLoader<Logger> loader = ServiceLoader.load(Logger.class);
        List<Logger> list = new ArrayList<>();
        for (Logger log : loader) {
            list.add(log);
        }
        // LoggerList 是所有 ServiceProvider
        loggerList = list;
        if (!list.isEmpty()) {
            // Logger 只取一个
            logger = list.get(0);
        } else {
            logger = null;
        }
    }

    public static LoggerService getService() {
        return SERVICE;
    }

    public void info(String msg) {
        if (logger == null) {
            System.out.println("info 中没有发现 Logger 服务提供者");
        } else {
            logger.info(msg);
        }
    }

    public void debug(String msg) {
        if (loggerList.isEmpty()) {
            System.out.println("debug 中没有发现 Logger 服务提供者");
        }
        loggerList.forEach(log -> log.debug(msg));
    }
}
```

新建 `Main` 类（服务使用者，调用方），启动程序查看结果。

```java
package org.spi.service;

public class Main {
    public static void main(String[] args) {
        LoggerService service = LoggerService.getService();

        service.info("Hello SPI");
        service.debug("Hello SPI");
    }
}
```

程序结果：

> info 中没有发现 Logger 服务提供者 debug 中没有发现 Logger 服务提供者

此时我们只是空有接口，并没有为 `Logger` 接口提供任何的实现，所以输出结果中没有按照预期打印相应的结果。

你可以使用命令或者直接使用 IDEA 将整个程序直接打包成 jar 包。

#### Service Provider

接下来新建一个项目用来实现 `Logger` 接口

新建项目 `service-provider` 目录结构如下：

```text
│  service-provider.iml
│
├─.idea
│  │  .gitignore
│  │  misc.xml
│  │  modules.xml
│  └─ workspace.xml
│
├─lib
│      service-provider-interface.jar
|
└─src
    ├─edu
    │  └─jiangxuan
    │      └─up
    │          └─spi
    │              └─service
    │                      Logback.java
    │
    └─META-INF
        └─services
                edu.jiangxuan.up.spi.Logger
```

新建 `Logback` 类

```java
package edu.jiangxuan.up.spi.service;

import edu.jiangxuan.up.spi.Logger;

public class Logback implements Logger {
    @Override
    public void info(String s) {
        System.out.println("Logback info 打印日志：" + s);
    }

    @Override
    public void debug(String s) {
        System.out.println("Logback debug 打印日志：" + s);
    }
}
```

将 `service-provider-interface` 的 jar 导入项目中。

新建 lib 目录，然后将 jar 包拷贝过来，再添加到项目中。

对jar右键`Add as Library -- ok`



接下来就可以在项目中导入 jar 包里面的一些类和方法了，就像 JDK 工具类导包一样的。

实现 `Logger` 接口，在 `src` 目录下新建 `META-INF/services` 文件夹，然后新建文件 `edu.jiangxuan.up.spi.Logger` （SPI 的全类名），文件里面的内容是：`edu.jiangxuan.up.spi.service.Logback` （Logback 的全类名，即 SPI 的实现类的包名 + 类名）。

**这是 JDK SPI 机制 ServiceLoader 约定好的标准。**

这里先大概解释一下：Java 中的 SPI 机制就是在每次类加载的时候会先去找到 class 相对目录下的 `META-INF` 文件夹下的 services 文件夹下的文件，将这个文件夹下面的所有文件先加载到内存中，然后根据这些文件的文件名和里面的文件内容找到相应接口的具体实现类，找到实现类后就可以通过反射去生成对应的对象，保存在一个 list 列表里面，所以可以通过迭代或者遍历的方式拿到对应的实例对象，生成不同的实现。

所以会提出一些规范要求：文件名一定要是接口的全类名，然后里面的内容一定要是实现类的全类名，实现类可以有多个，直接换行就好了，多个实现类的时候，会一个一个的迭代加载。

接下来同样将 `service-provider` 项目打包成 jar 包，这个 jar 包就是服务提供方的实现。通常我们导入 maven 的 pom 依赖就有点类似这种，只不过我们现在没有将这个 jar 包发布到 maven 公共仓库中，所以在需要使用的地方只能手动的添加到项目中。



新建 Main 方法测试：

```java
public class TestJavaSPI {
    public static void main(String[] args) {
        LoggerService loggerService = LoggerService.getService();
        loggerService.info("你好");
        loggerService.debug("测试Java SPI 机制");
    }
}
```

运行结果如下：

> Logback info 打印日志：你好 Logback debug 打印日志：测试 Java SPI 机制

说明导入 jar 包中的实现类生效了。

如果我们不导入具体的实现类的 jar 包，那么此时程序运行的结果就会是：

> info 中没有发现 Logger 服务提供者 debug 中没有发现 Logger 服务提供者

通过使用 SPI 机制，可以看出服务（`LoggerService`）和 服务提供者两者之间的耦合度非常低，如果说我们想要换一种实现，那么其实只需要修改 `service-provider` 项目中针对 `Logger` 接口的具体实现就可以了，只需要换一个 jar 包即可，也可以有在一个项目里面有多个实现，这不就是 SLF4J 原理吗？

如果某一天需求变更了，此时需要将日志输出到消息队列，或者做一些别的操作，这个时候完全不需要更改 Logback 的实现，只需要新增一个服务实现（service-provider）可以通过在本项目里面新增实现也可以从外部引入新的服务实现 jar 包。我们可以在服务(LoggerService)中选择一个具体的 服务实现(service-provider) 来完成我们需要的操作。

那么接下来我们具体来说说 Java SPI 工作的重点原理—— **ServiceLoader** 。



#### ServiceLoader 具体实现

想要使用 Java 的 SPI 机制是需要依赖 `ServiceLoader` 来实现的，那么我们接下来看看 `ServiceLoader` 具体是怎么做的：

`ServiceLoader` 是 JDK 提供的一个工具类， 位于`package java.util;`包下。

```text
A facility to load implementations of a service.
```

这是 JDK 官方给的注释：**一种加载服务实现的工具。**

再往下看，我们发现这个类是一个 `final` 类型的，所以是不可被继承修改，同时它实现了 `Iterable` 接口。之所以实现了迭代器，是为了方便后续我们能够通过迭代的方式得到对应的服务实现。

```java
public final class ServiceLoader<S> implements Iterable<S>{ xxx...}
```

可以看到一个熟悉的常量定义：

```
private static final String PREFIX = "META-INF/services/";
```

下面是 `load` 方法：可以发现 `load` 方法支持两种重载后的入参；

```java
public static <S> ServiceLoader<S> load(Class<S> service) {
    ClassLoader cl = Thread.currentThread().getContextClassLoader();
    return ServiceLoader.load(service, cl);
}

public static <S> ServiceLoader<S> load(Class<S> service,
                                        ClassLoader loader) {
    return new ServiceLoader<>(service, loader);
}

private ServiceLoader(Class<S> svc, ClassLoader cl) {
    service = Objects.requireNonNull(svc, "Service interface cannot be null");
    loader = (cl == null) ? ClassLoader.getSystemClassLoader() : cl;
    acc = (System.getSecurityManager() != null) ? AccessController.getContext() : null;
    reload();
}

public void reload() {
    providers.clear();
    lookupIterator = new LazyIterator(service, loader);
}
```

根据代码的调用顺序，在 `reload()` 方法中是通过一个内部类 `LazyIterator` 实现的。先继续往下面看。

`ServiceLoader` 实现了 `Iterable` 接口的方法后，具有了迭代的能力，在这个 `iterator` 方法被调用时，首先会在 `ServiceLoader` 的 `Provider` 缓存中进行查找，如果缓存中没有命中那么则在 `LazyIterator` 中进行查找。

```java
public Iterator<S> iterator() {
    return new Iterator<S>() {

        Iterator<Map.Entry<String, S>> knownProviders
                = providers.entrySet().iterator();

        public boolean hasNext() {
            if (knownProviders.hasNext())
                return true;
            return lookupIterator.hasNext(); // 调用 LazyIterator
        }

        public S next() {
            if (knownProviders.hasNext())
                return knownProviders.next().getValue();
            return lookupIterator.next(); // 调用 LazyIterator
        }

        public void remove() {
            throw new UnsupportedOperationException();
        }

    };
}
```

在调用 `LazyIterator` 时，具体实现如下：

```java
public boolean hasNext() {
    if (acc == null) {
        return hasNextService();
    } else {
        PrivilegedAction<Boolean> action = new PrivilegedAction<Boolean>() {
            public Boolean run() {
                return hasNextService();
            }
        };
        return AccessController.doPrivileged(action, acc);
    }
}

private boolean hasNextService() {
    if (nextName != null) {
        return true;
    }
    if (configs == null) {
        try {
            //通过PREFIX（META-INF/services/）和类名 获取对应的配置文件，得到具体的实现类
            String fullName = PREFIX + service.getName();
            if (loader == null)
                configs = ClassLoader.getSystemResources(fullName);
            else
                configs = loader.getResources(fullName);
        } catch (IOException x) {
            fail(service, "Error locating configuration files", x);
        }
    }
    while ((pending == null) || !pending.hasNext()) {
        if (!configs.hasMoreElements()) {
            return false;
        }
        pending = parse(service, configs.nextElement());
    }
    nextName = pending.next();
    return true;
}


public S next() {
    if (acc == null) {
        return nextService();
    } else {
        PrivilegedAction<S> action = new PrivilegedAction<S>() {
            public S run() {
                return nextService();
            }
        };
        return AccessController.doPrivileged(action, acc);
    }
}

private S nextService() {
    if (!hasNextService())
        throw new NoSuchElementException();
    String cn = nextName;
    nextName = null;
    Class<?> c = null;
    try {
        c = Class.forName(cn, false, loader);
    } catch (ClassNotFoundException x) {
        fail(service,
                "Provider " + cn + " not found");
    }
    if (!service.isAssignableFrom(c)) {
        fail(service,
                "Provider " + cn + " not a subtype");
    }
    try {
        S p = service.cast(c.newInstance());
        providers.put(cn, p);
        return p;
    } catch (Throwable x) {
        fail(service,
                "Provider " + cn + " could not be instantiated",
                x);
    }
    throw new Error();          // This cannot happen
}
```



#### 实现 ServiceLoader

```java
public class MyServiceLoader<S> {

    // 对应的接口 Class 模板
    private final Class<S> service;

    // 对应实现类的 可以有多个，用 List 进行封装
    private final List<S> providers = new ArrayList<>();

    // 类加载器
    private final ClassLoader classLoader;

    // 暴露给外部使用的方法，通过调用这个方法可以开始加载自己定制的实现流程。
    public static <S> MyServiceLoader<S> load(Class<S> service) {
        return new MyServiceLoader<>(service);
    }

    // 构造方法私有化
    private MyServiceLoader(Class<S> service) {
        this.service = service;
        this.classLoader = Thread.currentThread().getContextClassLoader();
        doLoad();
    }

    // 关键方法，加载具体实现类的逻辑
    private void doLoad() {
        try {
            // 读取所有 jar 包里面 META-INF/services 包下面的文件，这个文件名就是接口名，然后文件里面的内容就是具体的实现类的路径加全类名
            Enumeration<URL> urls = classLoader.getResources("META-INF/services/" + service.getName());
            // 挨个遍历取到的文件
            while (urls.hasMoreElements()) {
                // 取出当前的文件
                URL url = urls.nextElement();
                System.out.println("File = " + url.getPath());
                // 建立链接
                URLConnection urlConnection = url.openConnection();
                urlConnection.setUseCaches(false);
                // 获取文件输入流
                InputStream inputStream = urlConnection.getInputStream();
                // 从文件输入流获取缓存
                BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
                // 从文件内容里面得到实现类的全类名
                String className = bufferedReader.readLine();

                while (className != null) {
                    // 通过反射拿到实现类的实例
                    Class<?> clazz = Class.forName(className, false, classLoader);
                    // 如果声明的接口跟这个具体的实现类是属于同一类型，（可以理解为Java的一种多态，接口跟实现类、父类和子类等等这种关系。）则构造实例
                    if (service.isAssignableFrom(clazz)) {
                        Constructor<? extends S> constructor = (Constructor<? extends S>) clazz.getConstructor();
                        S instance = constructor.newInstance();
                        // 把当前构造的实例对象添加到 Provider的列表里面
                        providers.add(instance);
                    }
                    // 继续读取下一行的实现类，可以有多个实现类，只需要换行就可以了。
                    className = bufferedReader.readLine();
                }
            }
        } catch (Exception e) {
            System.out.println("读取文件异常。。。");
        }
    }

    // 返回spi接口对应的具体实现类列表
    public List<S> getProviders() {
        return providers;
    }
}
```

主要的流程就是：

1. 通过 URL 工具类从 jar 包的 `/META-INF/services` 目录下面找到对应的文件，
2. 读取这个文件的名称找到对应的 spi 接口，
3. 通过 `InputStream` 流将文件里面的具体实现类的全类名读取出来，
4. 根据获取到的全类名，先判断跟 spi 接口是否为同一类型，如果是的，那么就通过反射的机制构造对应的实例对象，
5. 将构造出来的实例对象添加到 `Providers` 的列表中。





### 序列化和反序列化

#### 什么是序列化?什么是反序列化?

简单定义：

- **序列化**：将数据结构或对象转换成二进制字节流的过程
- **反序列化**：将在序列化过程中所生成的二进制字节流转换成数据结构或者对象的过程



维基百科介绍：

> **序列化**（serialization）在计算机科学的数据处理中，是指将数据结构或对象状态转换成可取用格式（例如存成文件，存于缓冲，或经由网络中发送），以留待后续在相同或另一台计算机环境中，能恢复原先状态的过程。依照序列化格式重新获取字节的结果时，可以利用它来产生与原始对象相同语义的副本。对于许多对象，像是使用大量引用的复杂对象，这种序列化重建的过程并不容易。面向对象中的对象序列化，并不概括之前原始对象所关系的函数。这种过程也称为对象编组（marshalling）。从一系列字节提取数据结构的反向操作，是**反序列化**（也称为解编组、deserialization、unmarshalling）。
>
> 综上：**序列化的主要目的是通过网络传输对象或者说是将对象存储到文件系统、数据库、内存中。**



#### 序列化和反序列化常见应用场景：

- 对象在进行网络传输（比如远程方法调用 RPC 的时候）之前需要先被序列化，接收到序列化的对象之后需要再进行反序列化；
- 将对象存储到文件之前需要进行序列化，将对象从文件中读取出来需要进行反序列化；
- 将对象存储到数据库（如 Redis）之前需要用到序列化，将对象从缓存数据库中读取出来需要反序列化；
- 将对象存储到内存之前需要进行序列化，从内存中读取出来之后需要进行反序列化。



#### 序列化协议对应于 TCP/IP 4 层模型的哪一层？

![tcp-ip-4-model](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307111527555.png)

如上图所示，OSI 七层协议模型中，表示层做的事情主要就是对应用层的用户数据进行处理转换为二进制流。反过来的话，就是将二进制流转换成应用层的用户数据。这不就对应的是序列化和反序列化么？

因为，OSI 七层协议模型中的应用层、表示层和会话层对应的都是 TCP/IP 四层模型中的应用层，所以序列化协议属于 TCP/IP 协议应用层的一部分。



#### serialVersionUID 有什么作用？

序列化号 `serialVersionUID` 属于版本控制的作用。反序列化时，会检查 `serialVersionUID` 是否和当前类的 `serialVersionUID` 一致。如果 `serialVersionUID` 不一致则会抛出 `InvalidClassException` 异常

> 强烈推荐每个序列化类都手动指定其 `serialVersionUID`，如果不手动指定，那么编译器会动态生成默认的 `serialVersionUID`。

**serialVersionUID 不是被 static 变量修饰了吗？为什么还会被“序列化”？**

`static` 修饰的变量是静态变量，位于方法区，本身是不会被序列化的。 `static` 变量是属于类的而不是对象。反序列化之后，`static` 变量的值就像是默认赋予给了对象一样，看着就像是 `static` 变量被序列化，实际只是假象罢了。

官方说明如下：

> 如果想显式指定 `serialVersionUID` ，则需要在类中使用 `static` 和 `final` 关键字来修饰一个 `long` 类型的变量，变量名字必须为 `"serialVersionUID"` 。

也就是说，`serialVersionUID` 只是用来被 JVM 识别，实际并没有被序列化。



#### 如果有些字段不想进行序列化怎么办？

对于不想进行序列化的变量，使用 `transient` 关键字修饰。

`transient`：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 `transient` 修饰的变量值不会被持久化和恢复。

关于 `transient` 还有几点注意：

- `transient` 只能修饰变量，不能修饰类和方法。
- `transient` 修饰的变量，在反序列化后变量值将会被置成类型的默认值。例如，如果是修饰 `int` 类型，那么反序列后结果就是 `0`。
- `static` 变量因为不属于任何对象(Object)，所以无论有没有 `transient` 关键字修饰，均不会被序列化。



#### 常见序列化协议有哪些？

JDK 自带的序列化方式一般不会用 ，因为序列化效率低并且存在安全问题。比较常用的序列化协议有 *Hessian、Kryo、Protobuf、ProtoStuff*，这些都是基于二进制的序列化协议。

像 `JSON` 和 `XML` 这种属于文本类序列化方式。虽然可读性比较好，但是性能较差，一般不会选择。



#### 为什么不推荐使用 JDK 自带的序列化？

**不支持跨语言调用** : 如果调用的是其他语言开发的服务的时候就不支持了。

**性能差**：相比于其他序列化框架性能更低，主要原因是序列化之后的字节数组体积较大，导致传输成本加大。

**存在安全问题**：序列化和反序列化本身并不存在问题。但当输入的反序列化的数据可被用户控制，那么攻击者即可通过构造恶意输入，让反序列化产生非预期的对象，在此过程中执行构造的任意代码。



> 总结：
>
> Kryo 是专门针对 Java 语言序列化方式并且性能非常好，如果你的应用是专门针对 Java 语言的话可以考虑使用，并且 Dubbo 官网的一篇文章中提到说推荐使用 Kryo 作为生产环境的序列化方式。
>
> 像 Protobuf、 ProtoStuff、hessian 这类都是跨语言的序列化方式，如果有跨语言需求的话可以考虑使用。



### I/O

#### Java IO 流了解吗？

IO 即 `Input/Output`，输入和输出。数据输入到计算机内存的过程即输入，反之输出到外部存储（比如数据库，文件，远程主机）的过程即输出。数据传输过程类似于水流，因此称为 IO 流。IO 流在 Java 中分为输入流和输出流，而根据数据的处理方式又分为字节流和字符流。

Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。

- `InputStream`/`Reader`: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- `OutputStream`/`Writer`: 所有输出流的基类，前者是字节输出流，后者是字符输出流。



#### I/O 流为什么要分为字节流和字符流呢?

- 字符流是由 Java 虚拟机将字节转换得到的，这个过程还算是比较耗时。
- 如果我们不知道编码类型的话，使用字节流的过程中很容易出现乱码问题。



#### Java IO 中的设计模式有哪些？

*详见后续IO专题*



#### BIO、NIO 和 AIO 的区别？

*详见后续IO专题*



### 语法糖

#### 什么是语法糖？

**语法糖（Syntactic sugar）** 代指的是编程语言为了方便程序员开发程序而设计的一种特殊语法，这种语法对编程语言的功能并没有影响。实现相同的功能，基于语法糖写出来的代码往往更简单简洁且更易阅读。



举个例子，Java 中的 `for-each` 就是一个常用的语法糖，其原理其实就是基于普通的 for 循环和迭代器。

```java
String[] strs = {"Java", "MySQL", "Redis"};
for (String s : strs) {
  	System.out.println(s);
}
```

不过，JVM 其实并不能识别语法糖，Java 语法糖要想被正确执行，需要先通过编译器进行解糖，也就是在程序编译阶段将其转换成 JVM 认识的基本语法。这也侧面说明，Java 中真正支持语法糖的是 Java 编译器而不是 JVM。如果你去看`com.sun.tools.javac.main.JavaCompiler`的源码，你会发现在`compile()`中有一个步骤就是调用`desugar()`，这个方法就是负责解语法糖的实现的。

> Java 语言中，`javac`命令可以将后缀名为`.java`的源文件编译为后缀名为`.class`的可以运行于 Java 虚拟机的字节码。然而就在`com.sun.tools.javac.main.JavaCompiler`的源码中，`compile()`中有一个步骤就是调用`desugar()`，这个方法就是负责解语法糖的实现的。



#### Java 中有哪些常见的语法糖？

Java 中最常用的语法糖主要有**泛型、自动拆装箱、变长参数、枚举、内部类、增强 for 循环、try-with-resources 语法、lambda 表达式**等。



#### switch支持String与枚举

Java 中的`switch`自身原本就支持基本类型。比如`int`、`char`等。对于`int`类型，直接进行数值的比较。对于`char`类型则是比较其 ascii 码。所以，对于编译器来说，`switch`中其实只能使用整型，任何类型的比较都要转换成整型。比如`byte`。`short`，`char`(ascii 码是整型)以及`int`。



switch对String的支持示例代码：

```java
public class switchDemoString {
    public static void main(String[] args) {
        String str = "world";
        switch (str) {
        case "hello":
            System.out.println("hello");
            break;
        case "world":
            System.out.println("world");
            break;
        default:
            break;
        }
    }
}
```

反编译后内容如下：

```java
public class switchDemoString
{
    public switchDemoString()
    {
    }
    public static void main(String args[])
    {
        String str = "world";
        String s;
        switch((s = str).hashCode())
        {
        default:
            break;
        case 99162322:
            if(s.equals("hello"))
                System.out.println("hello");
            break;
        case 113318802:
            if(s.equals("world"))
                System.out.println("world");
            break;
        }
    }
}

```

通过上述代码可知 **字符串的 switch 是通过`equals()`和`hashCode()`方法来实现的。** 还好`hashCode()`方法返回的是`int`，而不是`long`。

进行`switch`的实际是哈希值，然后通过使用`equals`方法比较进行安全检查，这个检查是必要的，因为哈希可能会发生碰撞。因此它的性能是不如使用枚举进行 `switch` 或者使用纯整数常量，但这也不是很差。



#### 泛型

很多语言都是支持泛型的，但是不同的编译器对于泛型的处理方式是不同的，通常情况下，一个编译器处理泛型有两种方式：`Code specialization`和`Code sharing`。

C++和 C#是使用`Code specialization`的处理机制，而 Java 使用的是`Code sharing`的机制。

> Code sharing 方式为每个泛型类型创建唯一的字节码表示，并且将该泛型类型的实例都映射到这个唯一的字节码表示上。将多种泛型类形实例映射到唯一的字节码表示是通过类型擦除（`type erasue`）实现的。

也就是说，**对于 Java 虚拟机来说，根本不认识`Map<String, String> map`这样的语法。需要在编译阶段通过类型擦除的方式进行解语法糖。**

类型擦除的主要过程如下：

1. 将所有的泛型参数用其最左边界（最顶级的父类型）类型替换。 
2. 移除所有的类型参数。

以下代码：

```java
Map<String, String> map = new HashMap<String, String>();
map.put("name", "hollis");
map.put("wechat", "Hollis");
map.put("blog", "www.hollischuang.com");
```

解语法糖之后会变成：

```java
Map map = new HashMap();
map.put("name", "hollis");
map.put("wechat", "Hollis");
map.put("blog", "www.hollischuang.com");
```

以下代码：

```java
public static <A extends Comparable<A>> A max(Collection<A> xs) {
    Iterator<A> xi = xs.iterator();
    A w = xi.next();
    while (xi.hasNext()) {
        A x = xi.next();
        if (w.compareTo(x) < 0)
            w = x;
    }
    return w;
}
```

类型擦除后会变成：

```java
 public static Comparable max(Collection xs){
    Iterator xi = xs.iterator();
    Comparable w = (Comparable)xi.next();
    while(xi.hasNext())
    {
        Comparable x = (Comparable)xi.next();
        if(w.compareTo(x) < 0)
            w = x;
    }
    return w;
}
```

**虚拟机中没有泛型，只有普通类和普通方法，所有泛型类的类型参数在编译时都会被擦除，泛型类并没有自己独有的`Class`类对象。比如并不存在`List<String>.class`或是`List<Integer>.class`，而只有`List.class`。**



#### 自动装箱与拆箱

自动装箱就是 Java 自动将**原始类型值**转换成**对应的对象**.

比如将 int 的变量转换成 Integer 对象，这个过程叫做**装箱**，反之将 Integer 对象转换成 int 类型值，这个过程叫做**拆箱**。

因为这里的装箱和拆箱是自动进行的非人为转换，所以就称作为自动装箱和拆箱。

原始类型 byte, short, char, int, long, float, double 和 boolean 对应的封装类为 Byte, Short, Character, Integer, Long, Float, Double, Boolean。

自动装箱的代码：

```java
 public static void main(String[] args) {
    int i = 10;
    Integer n = i;
}
```

反编译后代码如下:

```java
public static void main(String args[])
{
    int i = 10;
    Integer n = Integer.valueOf(i);
}
```

自动拆箱的代码：

```java
public static void main(String[] args) {

    Integer i = 10;
    int n = i;
}
```

反编译后代码如下：

```java
public static void main(String args[])
{
    Integer i = Integer.valueOf(10);
    int n = i.intValue();
}
```

从反编译得到内容可以看出，在装箱的时候自动调用的是`Integer`的`valueOf(int)`方法。而在拆箱的时候自动调用的是`Integer`的`intValue`方法。

所以，**装箱过程是通过调用包装器的 valueOf 方法实现的，而拆箱过程是通过调用包装器的 xxxValue 方法实现的。**



**自动拆装箱遇上对象相等比较**

```java
public static void main(String[] args) {
    Integer a = 1000;
    Integer b = 1000;
    Integer c = 100;
    Integer d = 100;
    System.out.println("a == b is " + (a == b));
    System.out.println(("c == d is " + (c == d)));
}
```

```text
a == b is false
c == d is true
```

在 Java 5 中，在 Integer 的操作上引入了一个新功能来节省内存和提高性能。整型对象通过使用相同的对象引用实现了缓存和重用。

> 适用于整数值区间-128 至 +127。
>
> 只适用于自动装箱。使用构造函数创建对象不适用。



#### 可变长参数

可变参数(`variable arguments`)是在 Java 1.5 中引入的一个特性。它允许一个方法把任意数量的值作为参数。

看下以下可变参数代码，其中 `print` 方法接收可变参数：

```java
public static void main(String[] args) {
        print("Java", "xxxx", "Javaxxx", "hhhh");
    }

public static void print(String... strs) {
    for (int i = 0; i < strs.length; i++) {
        System.out.println(strs[i]);
    }
}
```

反编译后代码：

```java
public static void main(String[] var0) {
  print(new String[]{"Java", "xxxx", "Javaxxx", "hhhh"});
}

public static void print(String ... var0) {
  for(int var1 = 0; var1 < var0.length; ++var1) {
     System.out.println(var0[var1]);
  }

}
```

从反编译后代码可以看出，可变参数在被使用的时候，他首先会创建一个数组，数组的长度就是调用该方法是传递的实参的个数，然后再把参数值全部放到这个数组当中，然后再把这个数组作为参数传递到被调用的方法中



#### 枚举

关键字`enum`可以将一组具名的值的有限集合创建为一种新的类型，而这些具名的值可以作为常规的程序组件使用，这是一种非常有用的功能。

`enum`就和`class`一样，只是一个关键字，并不是一个类。

枚举示例：

```java
public enum t {
    SPRING,SUMMER;
}
```

反编译后代码内容如下：

```java
public final class T extends Enum
{
    private T(String s, int i)
    {
        super(s, i);
    }
    public static T[] values()
    {
        T at[];
        int i;
        T at1[];
        System.arraycopy(at = ENUM$VALUES, 0, at1 = new T[i = at.length], 0, i);
        return at1;
    }

    public static T valueOf(String s)
    {
        return (T)Enum.valueOf(demo/T, s);
    }

    public static final T SPRING;
    public static final T SUMMER;
    private static final T ENUM$VALUES[];
    static
    {
        SPRING = new T("SPRING", 0);
        SUMMER = new T("SUMMER", 1);
        ENUM$VALUES = (new T[] {
            SPRING, SUMMER
        });
    }
}
```

通过反编译后代码我们可以看到，`public final class T extends Enum`，说明，该类是继承了`Enum`类的，同时`final`关键字告诉我们，这个类也是不能被继承的。

**当使用`enum`来定义一个枚举类型的时候，编译器会自动帮我们创建一个`final`类型的类继承`Enum`类，所以枚举类型不能被继承。**



####  内部类

内部类又称为嵌套类，可以把内部类理解为外部类的一个普通成员。

**内部类之所以也是语法糖，是因为它仅仅是一个编译时的概念，`outer.java`里面定义了一个内部类`inner`，一旦编译成功，就会生成两个完全不同的`.class`文件了，分别是`outer.class`和`outer$inner.class`。所以内部类的名字完全可以和它的外部类名字相同。**

```java
public class OutterClass {
    private String userName;

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public static void main(String[] args) {

    }

    class InnerClass{
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
}
```

以上代码编译后会生成两个 class 文件：`OutterClass$InnerClass.class`、`OutterClass.class` 。尝试对`OutterClass.class`文件进行反编译，命令行会打印以下内容：`Parsing OutterClass.class...Parsing inner class OutterClass$InnerClass.class... Generating OutterClass.jad` 。他会把两个文件全部进行反编译，然后一起生成一个`OutterClass.jad`文件。文件内容如下：

```java
public class OutterClass {
    class InnerClass {
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        private String name;
        final OutterClass this$0;

        InnerClass() {
            this.this$0 = OutterClass.this;
            super();
        }
    }

    public OutterClass() {
    }
    public String getUserName() {
        return userName;
    }
    public void setUserName(String userName){
        this.userName = userName;
    }
    public static void main(String args1[]) {
    }
    private String userName;
}
```



#### 条件编译

—般情况下，程序中的每一行代码都要参加编译。但有时候出于对程序代码优化的考虑，希望只对其中一部分内容进行编译，此时就需要在程序中加上条件，让编译器只对满足条件的代码进行编译，将不满足条件的代码舍弃，这就是条件编译。

其实在 Java 中也可实现条件编译。我们先来看一段代码：

```java
public class ConditionalCompilation {
    public static void main(String[] args) {
        final boolean DEBUG = true;
        if(DEBUG) {
            System.out.println("Hello, DEBUG!");
        }

        final boolean ONLINE = false;

        if(ONLINE){
            System.out.println("Hello, ONLINE!");
        }
    }
}
```

反编译后代码如下：

```java
public class ConditionalCompilation
{

    public ConditionalCompilation()
    {
    }

    public static void main(String args[])
    {
        boolean DEBUG = true;
        System.out.println("Hello, DEBUG!");
        boolean ONLINE = false;
    }
}
```

首先，我们发现，在反编译后的代码中没有`System.out.println("Hello, ONLINE!");`，这其实就是条件编译。当`if(ONLINE)`为 false 的时候，编译器就没有对其内的代码进行编译。

所以，**Java 语法的条件编译，是通过判断条件为常量的 if 语句实现的。其原理也是 Java 语言的语法糖。根据 if 判断条件的真假，编译器直接把分支为 false 的代码块消除。通过该方式实现的条件编译，必须在方法体内实现，而无法在正整个 Java 类的结构或者类的属性上进行条件编译，这与 C/C++的条件编译相比，确实更有局限性。在 Java 语言设计之初并没有引入条件编译的功能，虽有局限，但是总比没有更强。**



#### 断言

Java 在执行的时候默认是不启动断言检查的（这个时候，所有的断言语句都将忽略！），如果要开启断言检查，则需要用开关`-enableassertions`或`-ea`来开启。

看一段包含断言的代码：

```java
public class AssertTest {
    public static void main(String args[]) {
        int a = 1;
        int b = 1;
        assert a == b;
        System.out.println("aaa");
        assert a != b : "bbb";
        System.out.println("ccc");
    }
}
```

反编译后代码如下：

```java
public class AssertTest {
   public AssertTest() { }
    public static void main(String args[]) {
    int a = 1;
    int b = 1;
    if(!$assertionsDisabled && a != b) throw new AssertionError();
    System.out.println("aaa");
    if(!$assertionsDisabled && a == b) throw new AssertionError("bbb");
    else {System.out.println("ccc"); return;}
}

static final boolean $assertionsDisabled = !com/hollis/suguar/AssertTest.desiredAssertionStatus();

}
```

很明显，反编译之后的代码要比我们自己的代码复杂的多。所以，使用了 assert 这个语法糖我们节省了很多代码。**其实断言的底层实现就是 if 语言，如果断言结果为 true，则什么都不做，程序继续执行，如果断言结果为 false，则程序抛出 AssertError 来打断程序的执行。**`-enableassertions`会设置$assertionsDisabled 字段的值。



#### 数值字面量

数值字面量，不管是整数还是浮点数，都允许在数字之间插入任意多个下划线。这些下划线不会对字面量的数值产生影响，目的就是方便阅读。

比如：

```java
public class Test {
    public static void main(String... args) {
        int i = 10_000;
        System.out.println(i);
    }
}
```

反编译后：

```java
public class Test
{
  public static void main(String[] args)
  {
    int i = 10000;
    System.out.println(i);
  }
}
```

反编译后就是把`_`删除了。也就是说 **编译器并不认识在数字字面量中的`_`，需要在编译阶段把他去掉。**



#### for-each

```java
public static void main(String... args) {
    String[] strs = {"aaa", "bbb", "ccc"};
    for (String s : strs) {
        System.out.println(s);
    }
    List<String> strList = ImmutableList.of("aaa", "bbb", "ccc");
    for (String s : strList) {
        System.out.println(s);
    }
}
```

反编译后代码如下：

```java
public static transient void main(String args[]) {
    String strs[] = {
        "aaa", "bbb", "ccc"
    };
    String args1[] = strs;
    int i = args1.length;
    for(int j = 0; j < i; j++) {
        String s = args1[j];
        System.out.println(s);
    }

    List strList = ImmutableList.of("aaa", "bbb", "ccc");
    String s;
    for(Iterator iterator = strList.iterator(); iterator.hasNext(); System.out.println(s))
        s = (String)iterator.next();
}
```

**for-each 的实现原理其实就是使用了普通的 for 循环和迭代器。**

*Iterator*迭代器删除元素时引发的问题

```java
for (Student stu : students) {
    if (stu.getId() == 2)
        students.remove(stu);
}
```

会抛出`ConcurrentModificationException`异常。

Iterator 是工作在一个独立的线程中，并且拥有一个 mutex 锁。 Iterator 被创建之后会建立一个指向原来对象的单链索引表，当原来的对象数量发生变化时，这个索引表的内容不会同步改变，所以当索引指针往后移动的时候就找不到要迭代的对象，所以按照 fail-fast 原则 Iterator 会马上抛出`java.util.ConcurrentModificationException`异常。

所以 `Iterator` 在工作的时候是不允许被迭代的对象被改变的。但你可以使用 `Iterator` 本身的方法`remove()`来删除对象，`Iterator.remove()` 方法会在删除当前迭代对象的同时维护索引的一致性。



#### try-with-resource

Java 里，对于文件操作 IO 流、数据库连接等开销非常昂贵的资源，用完之后必须及时通过 close 方法将其关闭，否则资源会一直处于打开状态，可能会导致内存泄露等问题。

```java
public static void main(String... args) {
    try (BufferedReader br = new BufferedReader(new FileReader("aaa.xml"))) {
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
    } catch (IOException e) {
        // handle exception
    }
}
```

看，这简直是一大福音啊，虽然我之前一般使用`IOUtils`去关闭流，并不会使用在`finally`中写很多代码的方式，但是这种新的语法糖看上去好像优雅很多呢。看下他的背后：

```java
public static transient void main(String args[])
    {
        BufferedReader br;
        Throwable throwable;
        br = new BufferedReader(new FileReader("d:\\ hollischuang.xml"));
        throwable = null;
        String line;
        try
        {
            while((line = br.readLine()) != null)
                System.out.println(line);
        }
        catch(Throwable throwable2)
        {
            throwable = throwable2;
            throw throwable2;
        }
        if(br != null)
            if(throwable != null)
                try
                {
                    br.close();
                }
                catch(Throwable throwable1)
                {
                    throwable.addSuppressed(throwable1);
                }
            else
                br.close();
            break MISSING_BLOCK_LABEL_113;
            Exception exception;
            exception;
            if(br != null)
                if(throwable != null)
                    try
                    {
                        br.close();
                    }
                    catch(Throwable throwable3)
                      {
                        throwable.addSuppressed(throwable3);
                    }
                else
                    br.close();
        throw exception;
        IOException ioexception;
        ioexception;
    }
}
```

**其实背后的原理也很简单，那些我们没有做的关闭资源的操作，编译器都帮我们做了。所以，再次印证了，语法糖的作用就是方便程序员的使用，但最终还是要转成编译器认识的语言。**



#### Lambda 表达式

**Lambda 表达式不是匿名内部类的语法糖，但是他也是一个语法糖。实现方式其实是依赖了几个 JVM 底层提供的 lambda 相关 api。**

```java
public static void main(String... args) {
    List<String> strList = ImmutableList.of("aaa", "bbb", "ccc");

    strList.forEach( s -> { System.out.println(s); } );
}
```

为啥说他并不是内部类的语法糖呢，前面讲内部类我们说过，内部类在编译之后会有两个 class 文件，但是，包含 lambda 表达式的类编译后只有一个文件。

反编译后代码如下:

```java
public static /* varargs */ void main(String ... args) {
    ImmutableList strList = ImmutableList.of((Object)"Hollis", (Object)"\u516c\u4f17\u53f7\uff1aHollis", (Object)"\u535a\u5ba2\uff1awww.hollischuang.com");
    strList.forEach((Consumer<String>)LambdaMetafactory.metafactory(null, null, null, (Ljava/lang/Object;)V, lambda$main$0(java.lang.String ), (Ljava/lang/String;)V)());
}

private static /* synthetic */ void lambda$main$0(String s) {
    System.out.println(s);
}
```

可以看到，在`forEach`方法中，其实是调用了`java.lang.invoke.LambdaMetafactory#metafactory`方法，该方法的第四个参数 `implMethod` 指定了方法实现。可以看到这里其实是调用了一个`lambda$main$0`方法进行了输出。

再来看一个稍微复杂一点的，先对 List 进行过滤，然后再输出：

```java
public static void main(String... args) {
    List<String> strList = ImmutableList.of("aaa", "bbb", "ccc");

    List HollisList = strList.stream().filter(string -> string.contains("aaa")).collect(Collectors.toList());

    HollisList.forEach( s -> { System.out.println(s); } );
}
```

反编译后代码如下：

```java
public static /* varargs */ void main(String ... args) {
    ImmutableList strList = ImmutableList.of((Object)"aaa", (Object)"bbb", (Object)"ccc");
    List<Object> HollisList = strList.stream().filter((Predicate<String>)LambdaMetafactory.metafactory(null, null, null, (Ljava/lang/Object;)Z, lambda$main$0(java.lang.String ), (Ljava/lang/String;)Z)()).collect(Collectors.toList());
    HollisList.forEach((Consumer<Object>)LambdaMetafactory.metafactory(null, null, null, (Ljava/lang/Object;)V, lambda$main$1(java.lang.Object ), (Ljava/lang/Object;)V)());
}

private static /* synthetic */ void lambda$main$1(Object s) {
    System.out.println(s);
}

private static /* synthetic */ boolean lambda$main$0(String string) {
    return string.contains("Hollis");
}
```

两个 lambda 表达式分别调用了`lambda$main$1`和`lambda$main$0`两个方法。

**所以，lambda 表达式的实现其实是依赖了一些底层的 api，在编译阶段，编译器会把 lambda 表达式进行解糖，转换成调用内部 api 的方式。**



### 值传递详解

> Java 中将实参传递给方法（或函数）的方式是 **值传递**：
>
> - 如果参数是基本类型的话，很简单，传递的就是基本类型的字面量值的拷贝，会创建副本。
> - 如果参数是引用类型，传递的就是实参所引用的对象在堆中地址值的拷贝，同样也会创建副本。

#### 形参&实参

方法的定义可能会用到 **参数**（有参的方法），参数在程序语言中分为：

- **实参（实际参数，Arguments）**：用于传递给函数/方法的参数，必须有确定的值。
- **形参（形式参数，Parameters）**：用于定义函数/方法，接收实参，不需要有确定的值。



#### 值传递&引用传递

程序设计语言将实参传递给方法（或函数）的方式分为两种：

- **值传递**：方法接收的是实参值的拷贝，会创建副本。
- **引用传递**：方法接收的直接是实参所引用的对象在堆中的地址，不会创建副本，对形参的修改将影响到实参。



#### 为什么Java只有值传递

- 案例1：传递基本类型参数

  ```java
  public static void main(String[] args) {
      int num1 = 10;
      int num2 = 20;
      swap(num1, num2);
      System.out.println("num1 = " + num1);
      System.out.println("num2 = " + num2);
  }
  
  public static void swap(int a, int b) {
      int temp = a;
      a = b;
      b = temp;
      System.out.println("a = " + a);
      System.out.println("b = " + b);
  }
  ```

  ```
  a = 20
  b = 10
  num1 = 10
  num2 = 20
  ```
  
  > 在`swap()`方法中，`a`、`b`的值进行交换，并不会影响到 `num1`、`num2`。因为，`a`、`b` 的值，只是从 `num1`、`num2` 的复制过来的。也就是说，a、b 相当于 `num1`、`num2` 的副本，副本的内容无论怎么修改，都不会影响到原件本身。（一个方法不能修改一个基本数据类型的参数）

- 案例2：传递引用类型参数

  ```java
  public static void main(String[] args) {
    int[] arr = { 1, 2, 3, 4, 5 };
    System.out.println(arr[0]);  // 1
    change(arr);
    System.out.println(arr[0]);  // 0
  }
  
  public static void change(int[] array) {
    // 将数组的第一个元素变为0
    array[0] = 0;
  }
  ```

  > 实际上这里传递的还是值，不过，这个值是实参的地址罢了！
  >
  > 也就是说 `change` 方法的参数拷贝的是 `arr` （实参）的地址，因此，它和 `arr` 指向的是同一个数组对象。这也就说明了为什么方法内部对形参的修改会影响到实参。
  
- 案例3：传递引用类型参数

  ```java
  @Data
  @NoArgsConstructor
  @AllArgsConstructor
  public class Person {
      private String name;
  }
  
  public static void main(String[] args) {
      Person xiaoZhang = new Person("小张");
      Person xiaoLi = new Person("小李");
      swap(xiaoZhang, xiaoLi);
      System.out.println("xiaoZhang:" + xiaoZhang.getName());
      System.out.println("xiaoLi:" + xiaoLi.getName());
  }
  
  public static void swap(Person person1, Person person2) {
      Person temp = person1;
      person1 = person2;
      person2 = temp;
      System.out.println("person1:" + person1.getName());
      System.out.println("person2:" + person2.getName());
  }
  ```

  ```
  person1:小李
  person2:小张
  xiaoZhang:小张
  xiaoLi:小李
  ```
  
  > `swap` 方法的参数 `person1` 和 `person2` 只是拷贝的实参 `xiaoZhang` 和 `xiaoLi` 的地址。因此， `person1` 和 `person2` 的互换只是拷贝的两个地址的互换罢了，并不会影响到实参 `xiaoZhang` 和 `xiaoLi` 。



#### 什么是引用传递

```c++
#include <iostream>

void incr(int& num)
{
    std::cout << "incr before: " << num << "\n";
    num++;
    std::cout << "incr after: " << num << "\n";
}

int main()
{
    int age = 10;
    std::cout << "invoke before: " << age << "\n";
    incr(age);
    std::cout << "invoke after: " << age << "\n";
}
```

```
invoke before: 10
incr before: 10
incr after: 11
invoke after: 11
```

> 在 `incr` 函数中对形参的修改，可以影响到实参的值。要注意：这里的 `incr` 形参的数据类型用的是 `int&` 才为引用传递，如果是用 `int` 的话还是值传递



#### 为什么 Java 不引入引用传递呢？

- 出于安全考虑，方法内部对值进行的操作，对于调用者都是未知的（把方法定义为接口，调用方不关心具体实现）。
- Java 之父 James Gosling 在设计之初就看到了 C、C++ 的许多弊端，所以才想着去设计一门新的语言 Java。在他设计 Java 的时候就遵循了简单易用的原则，摒弃了许多开发者一不留意就会造成问题的“特性”，语言本身的东西少了，开发者要学习的东西也少了。



### 代理模式详解

#### 代理模式

代理模式是一种比较好理解的设计模式。简单来说就是 **我们使用代理对象来代替对真实对象(real object)的访问，这样就可以在不修改原目标对象的前提下，提供额外的功能操作，扩展目标对象的功能。**

**代理模式的主要作用是扩展目标对象的功能，比如说在目标对象的某个方法执行前后你可以增加一些自定义的操作。**

举个例子：新娘找来了自己的姨妈来代替自己处理新郎的提问，新娘收到的提问都是经过姨妈处理过滤之后的。姨妈在这里就可以看作是代理你的代理对象，代理的行为（方法）是接收和回复新郎的提问。

代理模式有静态代理和动态代理两种实现方式

![1_DjWCgTFm-xqbhbNQVsaWQw](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307142257976.png)



#### 静态代理

- 从实现和应用角度来说

  静态代理中，我们对目标对象的每个方法的增强都是手动完成的,非常不灵活（比如接口一旦新增加方法，目标对象和代理对象都要进行修改）且麻烦(需要对每个目标类都单独写一个代理类）。 实际应用场景非常非常少，日常开发几乎看不到使用静态代理的场景。

- 从 JVM 层面来说

   静态代理在编译时就将接口、实现类、代理类这些都变成了一个个实际的 class 文件。

静态代理实现步骤:

1. 定义一个接口及其实现类；
2. 创建一个代理类同样实现这个接口
3. 将目标对象注入进代理类，然后在代理类的对应方法调用目标类中的对应方法。这样的话，我们就可以通过代理类屏蔽对目标对象的访问，并且可以在目标方法执行前后做一些自己想做的事情。

**1.定义发送短信的接口**

```java
public interface SmsService {
    String send(String message);
}
```

**2.实现发送短信的接口**

```java
public class SmsServiceImpl implements SmsService {
    public String send(String message) {
        System.out.println("send message:" + message);
        return message;
    }
}
```

**3.创建代理类并同样实现发送短信的接口**

```java
public class SmsProxy implements SmsService {

    private final SmsService smsService;

    public SmsProxy(SmsService smsService) {
        this.smsService = smsService;
    }

    @Override
    public String send(String message) {
        //调用方法之前，我们可以添加自己的操作
        System.out.println("before method send()");
        smsService.send(message);
        //调用方法之后，我们同样可以添加自己的操作
        System.out.println("after method send()");
        return null;
    }
}
```

**4.实际使用**

```java
public class Main {
    public static void main(String[] args) {
        SmsService smsService = new SmsServiceImpl();
        SmsProxy smsProxy = new SmsProxy(smsService);
        smsProxy.send("java");
    }
}
```

运行上述代码之后，控制台打印出：

```bash
before method send()
send message:java
after method send()
```

可以输出结果看出，我们已经增加了 `SmsServiceImpl` 的`send()`方法。



#### 动态代理

相比于静态代理来说，动态代理更加灵活。不需要针对每个目标类都单独创建一个代理类，并且也不需要我们必须实现接口，我们可以直接代理实现类( *CGLIB 动态代理机制*)。

**从 JVM 角度来说，动态代理是在运行时动态生成类字节码，并加载到 JVM 中的。**

Spring AOP、RPC 框架的实现都依赖了动态代理。

就 Java 来说，动态代理的实现方式有很多种，如 **JDK 动态代理**、**CGLIB 动态代理**。



##### JDK 动态代理机制

**在 Java 动态代理机制中 `InvocationHandler` 接口和 `Proxy` 类是核心。**

`Proxy` 类中使用频率最高的方法是：`newProxyInstance()` ，这个方法主要用来生成一个代理对象。

```java
public static Object newProxyInstance(ClassLoader loader,
                                      Class<?>[] interfaces,
                                      InvocationHandler h)
    throws IllegalArgumentException
{
    ......
}
```



这个方法一共有 3 个参数：

1. **loader** :类加载器，用于加载代理对象。
2. **interfaces** : 被代理类实现的一些接口；
3. **h** : 实现了 `InvocationHandler` 接口的对象；

要实现动态代理的话，还必须实现`InvocationHandler` 来自定义处理逻辑。 当我们的动态代理对象调用一个方法时，这个方法的调用就会被转发到实现`InvocationHandler` 接口类的 `invoke` 方法来调用。

```java
public interface InvocationHandler {

    /**
     * 当你使用代理对象调用方法的时候实际会调用到这个方法
     */
    public Object invoke(Object proxy, Method method, Object[] args)
        throws Throwable;
}
```

`invoke()` 方法有下面三个参数：

1. **proxy** :动态生成的代理类
2. **method** : 与代理类对象调用的方法相对应
3. **args** : 当前 method 方法的参数

也就是说：**通过`Proxy` 类的 `newProxyInstance()` 创建的代理对象在调用方法的时候，实际会调用到实现`InvocationHandler` 接口的类的 `invoke()`方法。** 你可以在 `invoke()` 方法中自定义处理逻辑，比如在方法执行前后做什么事情。



##### JDK 动态代理类使用步骤

1. 定义一个接口及其实现类；
2. 自定义 `InvocationHandler` 并重写`invoke`方法，在 `invoke` 方法中我们会调用原生方法（被代理类的方法）并自定义一些处理逻辑；
3. 通过 `Proxy.newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h)` 方法创建代理对象；



这样说可能会有点空洞和难以理解，我上个例子，大家感受一下吧！

**1.定义发送短信的接口**

```java
public interface SmsService {
    String send(String message);
}
```

**2.实现发送短信的接口**

```java
public class SmsServiceImpl implements SmsService {
    public String send(String message) {
        System.out.println("send message:" + message);
        return message;
    }
}
```

**3.定义一个 JDK 动态代理类**

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
 * @author shuang.kou
 * @createTime 2020年05月11日 11:23:00
 */
public class DebugInvocationHandler implements InvocationHandler {
    /**
     * 代理类中的真实对象
     */
    private final Object target;

    public DebugInvocationHandler(Object target) {
        this.target = target;
    }


    public Object invoke(Object proxy, Method method, Object[] args) throws InvocationTargetException, IllegalAccessException {
        //调用方法之前，我们可以添加自己的操作
        System.out.println("before method " + method.getName());
        Object result = method.invoke(target, args);
        //调用方法之后，我们同样可以添加自己的操作
        System.out.println("after method " + method.getName());
        return result;
    }
}
```

`invoke()` 方法: 当动态代理对象调用原生方法的时候，最终实际上调用到的是 `invoke()` 方法，然后 `invoke()` 方法调用了被代理对象的原生方法。

**4.获取代理对象的工厂类**

```java
public class JdkProxyFactory {
    public static Object getProxy(Object target) {
        return Proxy.newProxyInstance(
                target.getClass().getClassLoader(), // 目标类的类加载
                target.getClass().getInterfaces(),  // 代理需要实现的接口，可指定多个
                new DebugInvocationHandler(target)   // 代理对象对应的自定义 InvocationHandler
        );
    }
}
```

`getProxy()`：主要通过`Proxy.newProxyInstance（）`方法获取某个类的代理对象

**5.实际使用**

```java
SmsService smsService = (SmsService) JdkProxyFactory.getProxy(new SmsServiceImpl());
smsService.send("java");
```

```text
before method send
send message:java
after method send
```



#### CGLIB 动态代理机制

**JDK 动态代理有一个最致命的问题是其只能代理实现了接口的类。**

**为了解决这个问题，可以用 CGLIB 动态代理机制来避免。**

CGLIB是一个基于ASM的字节码生成库，它允许我们在运行时对字节码进行修改和动态生成。CGLIB 通过继承方式实现代理。很多知名的开源框架都使用到了CGLIB， 例如 Spring 中的 AOP 模块中：如果目标对象实现了接口，则默认采用 JDK 动态代理，否则采用 CGLIB 动态代理。

**在 CGLIB 动态代理机制中 `MethodInterceptor` 接口和 `Enhancer` 类是核心。**

需要自定义 `MethodInterceptor` 并重写 `intercept` 方法，`intercept` 用于拦截增强被代理类的方法。

```java
public interface MethodInterceptor
extends Callback{
    // 拦截被代理类中的方法
    public Object intercept(Object obj, java.lang.reflect.Method method, Object[] args,MethodProxy proxy) throws Throwable;
}
```

1. **obj** : 被代理的对象（需要增强的对象）
2. **method** : 被拦截的方法（需要增强的方法）
3. **args** : 方法入参
4. **proxy** : 用于调用原始方法

你可以通过 `Enhancer`类来动态获取被代理类，当代理类调用方法的时候，实际调用的是 `MethodInterceptor` 中的 `intercept` 方法。



##### CGLIB 动态代理类使用步骤

1. 定义一个类；
2. 自定义 `MethodInterceptor` 并重写 `intercept` 方法，`intercept` 用于拦截增强被代理类的方法，和 JDK 动态代理中的 `invoke` 方法类似；
3. 通过 `Enhancer` 类的 `create()`创建代理类；



不同于 JDK 动态代理不需要额外的依赖。CGLIB 实际是属于一个开源项目，如果你要使用它的话，需要手动添加相关依赖。

```xml
<dependency>
  <groupId>cglib</groupId>
  <artifactId>cglib</artifactId>
  <version>3.3.0</version>
</dependency>
```

**1.实现一个使用阿里云发送短信的类**

```java
package github.javaguide.dynamicProxy.cglibDynamicProxy;

public class AliSmsService {
    public String send(String message) {
        System.out.println("send message:" + message);
        return message;
    }
}
```

**2.自定义 `MethodInterceptor`（方法拦截器）**

```java
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

/**
 * 自定义MethodInterceptor
 */
public class DebugMethodInterceptor implements MethodInterceptor {


    /**
     * @param o           被代理的对象（需要增强的对象）
     * @param method      被拦截的方法（需要增强的方法）
     * @param args        方法入参
     * @param methodProxy 用于调用原始方法
     */
    @Override
    public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        //调用方法之前，我们可以添加自己的操作
        System.out.println("before method " + method.getName());
        Object object = methodProxy.invokeSuper(o, args);
        //调用方法之后，我们同样可以添加自己的操作
        System.out.println("after method " + method.getName());
        return object;
    }

}
```

**3.获取代理类**

```java
import net.sf.cglib.proxy.Enhancer;

public class CglibProxyFactory {

    public static Object getProxy(Class<?> clazz) {
        // 创建动态代理增强类
        Enhancer enhancer = new Enhancer();
        // 设置类加载器
        enhancer.setClassLoader(clazz.getClassLoader());
        // 设置被代理类
        enhancer.setSuperclass(clazz);
        // 设置方法拦截器
        enhancer.setCallback(new DebugMethodInterceptor());
        // 创建代理类
        return enhancer.create();
    }
}
```

**4.实际使用**

```java
AliSmsService aliSmsService = (AliSmsService) CglibProxyFactory.getProxy(AliSmsService.class);
aliSmsService.send("java");
```

运行上述代码之后，控制台打印出：

```bash
before method send
send message:java
after method send
```

#### JDK 动态代理和 CGLIB 动态代理对比

1. **JDK 动态代理只能代理实现了接口的类或者直接代理接口，而 CGLIB 可以代理未实现任何接口的类。** 另外， CGLIB 动态代理是通过生成一个被代理类的子类来拦截被代理类的方法调用，因此不能代理声明为 final 类型的类和方法。
2. 就二者的效率来说，大部分情况都是 JDK 动态代理更优秀，随着 JDK 版本的升级，这个优势更加明显。



#### 静态代理和动态代理的对比

1. **灵活性**：动态代理更加灵活，不需要必须实现接口，可以直接代理实现类，并且可以不需要针对每个目标类都创建一个代理类。另外，静态代理中，接口一旦新增加方法，目标对象和代理对象都要进行修改，这是非常麻烦的！
2. **JVM 层面**：静态代理在编译时就将接口、实现类、代理类这些都变成了一个个实际的 class 文件。而动态代理是在运行时动态生成类字节码，并加载到 JVM 中的。



### Java 魔法类 Unsafe 详解

#### Unsafe 介绍

`Unsafe` 是位于 `sun.misc` 包下的一个类，主要提供一些用于执行低级别、不安全操作的方法，如直接访问系统内存资源、自主管理内存资源等，这些方法在提升 Java 运行效率、增强 Java 语言底层资源操作能力方面起到了很大的作用。但由于 `Unsafe` 类使 Java 语言拥有了类似 C 语言指针一样操作内存空间的能力，这无疑也增加了程序发生相关指针问题的风险。在程序中过度、不正确使用 `Unsafe` 类会使得程序出错的概率变大，使得 Java 这种安全的语言变得不再“安全”，因此对 `Unsafe` 的使用一定要慎重。

另外，`Unsafe` 提供的这些功能的实现需要依赖本地方法（Native Method）。你可以将本地方法看作是 Java 中使用其他编程语言编写的方法。本地方法使用 **`native`** 关键字修饰，Java 代码中只是声明方法头，具体的实现则交给 **本地代码**。



**为什么要使用本地方法呢？**

1. 需要用到 Java 中不具备的依赖于操作系统的特性，Java 在实现跨平台的同时要实现对底层的控制，需要借助其他语言发挥作用。
2. 对于其他语言已经完成的一些现成功能，可以使用 Java 直接调用。
3. 程序对时间敏感或对性能要求非常高时，有必要使用更加底层的语言，例如 C/C++甚至是汇编。

在 JUC 包的很多并发工具类在实现并发机制时，都调用了本地方法，通过它们打破了 Java 运行时的界限，能够接触到操作系统底层的某些功能。对于同一本地方法，不同的操作系统可能会通过不同的方式来实现，但是对于使用者来说是透明的，最终都会得到相同的结果。



#### Unsafe 创建

`sun.misc.Unsafe` 部分源码如下：

```java
public final class Unsafe {
  // 单例对象
  private static final Unsafe theUnsafe;
  ......
  private Unsafe() {
  }
  @CallerSensitive
  public static Unsafe getUnsafe() {
    Class var0 = Reflection.getCallerClass();
    // 仅在引导类加载器`BootstrapClassLoader`加载时才合法
    if(!VM.isSystemDomainLoader(var0.getClassLoader())) {
      throw new SecurityException("Unsafe");
    } else {
      return theUnsafe;
    }
  }
}
```

`Unsafe` 类为一单例实现，提供静态方法 `getUnsafe` 获取 `Unsafe`实例。这个看上去貌似可以用来获取 `Unsafe` 实例。但是，当我们直接调用这个静态方法的时候，会抛出 `SecurityException` 异常：

```bash
Exception in thread "main" java.lang.SecurityException: Unsafe
 at sun.misc.Unsafe.getUnsafe(Unsafe.java:90)
 at com.cn.test.GetUnsafeTest.main(GetUnsafeTest.java:12)
```



为什么 `public static` 方法无法被直接调用呢？

这是因为在`getUnsafe`方法中，会对调用者的`classLoader`进行检查，判断当前类是否由`Bootstrap classLoader`加载，如果不是的话那么就会抛出一个`SecurityException`异常。也就是说，只有启动类加载器加载的类才能够调用 Unsafe 类中的方法，来防止这些方法在不可信的代码中被调用。



为什么要对 Unsafe 类进行这么谨慎的使用限制呢?

`Unsafe` 提供的功能过于底层（如直接访问系统内存资源、自主管理内存资源等），安全隐患也比较大，使用不当的话，很容易出现很严重的问题。



如若想使用 `Unsafe` 这个类的话，应该如何获取其实例呢？

1、利用反射获得 Unsafe 类中已经实例化完成的单例对象 `theUnsafe` 。

```java
private static Unsafe reflectGetUnsafe() {
    try {
      Field field = Unsafe.class.getDeclaredField("theUnsafe");
      field.setAccessible(true);
      return (Unsafe) field.get(null);
    } catch (Exception e) {
      log.error(e.getMessage(), e);
      return null;
    }
}
```

2、从`getUnsafe`方法的使用限制条件出发，通过 Java 命令行命令`-Xbootclasspath/a`把调用 Unsafe 相关方法的类 A 所在 jar 包路径追加到默认的 bootstrap 路径中，使得 A 被引导类加载器加载，从而通过`Unsafe.getUnsafe`方法安全的获取 Unsafe 实例。

```bash
java -Xbootclasspath/a: ${path}   // 其中path为调用Unsafe相关方法的类所在jar包路径
```

#### Unsafe 功能

概括的来说，`Unsafe` 类实现功能可以被分为下面 8 类：

1. 内存操作
2. 内存屏障
3. 对象操作
4. 数据操作
5. CAS 操作
6. 线程调度
7. Class 操作
8. 系统信息



#### 内存操作

Java 中是不允许直接对内存进行操作的，对象内存的分配和回收都是由 JVM 自己实现的。但是在 `Unsafe` 中，提供的下列接口可以直接进行内存操作：

```java
// 分配新的本地空间
public native long allocateMemory(long bytes);
// 重新调整内存空间的大小
public native long reallocateMemory(long address, long bytes);
// 将内存设置为指定值
public native void setMemory(Object o, long offset, long bytes, byte value);
// 内存拷贝
public native void copyMemory(Object srcBase, long srcOffset,Object destBase, long destOffset,long bytes);
// 清除内存
public native void freeMemory(long address);
```

使用下面的代码进行测试：

```java
private void memoryTest() {
    int size = 4;
    long addr = unsafe.allocateMemory(size);
    long addr3 = unsafe.reallocateMemory(addr, size * 2);
    System.out.println("addr: "+addr);
    System.out.println("addr3: "+addr3);
    try {
        unsafe.setMemory(null,addr ,size,(byte)1);
        for (int i = 0; i < 2; i++) {
            unsafe.copyMemory(null,addr,null,addr3+size*i,4);
        }
        System.out.println(unsafe.getInt(addr));
        System.out.println(unsafe.getLong(addr3));
    }finally {
        unsafe.freeMemory(addr);
        unsafe.freeMemory(addr3);
    }
}
```

```text
addr: 2433733895744
addr3: 2433733894944
16843009
72340172838076673
```

首先使用`allocateMemory`方法申请 4 字节长度的内存空间，调用`setMemory`方法向每个字节写入内容为`byte`类型的 1，当使用 Unsafe 调用`getInt`方法时，因为一个`int`型变量占 4 个字节，会一次性读取 4 个字节，组成一个`int`的值，对应的十进制结果为 16843009。

![image-20220717144344005](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307151659862.png)



在代码中调用`reallocateMemory`方法重新分配了一块 8 字节长度的内存空间，通过比较`addr`和`addr3`可以看到和之前申请的内存地址是不同的。在代码中的第二个 for 循环里，调用`copyMemory`方法进行了两次内存的拷贝，每次拷贝内存地址`addr`开始的 4 个字节，分别拷贝到以`addr3`和`addr3+4`开始的内存空间上：

![image-20220717144354582](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307151729043.png)

拷贝完成后，使用`getLong`方法一次性读取 8 个字节，得到`long`类型的值为 72340172838076673。

需要注意，通过这种方式分配的内存属于 堆外内存 ，是无法进行垃圾回收的，需要我们把这些内存当做一种资源去手动调用`freeMemory`方法进行释放，否则会产生内存泄漏。通用的操作内存方式是在`try`中执行对内存的操作，最终在`finally`块中进行内存的释放。



为什么要使用堆外内存？

- 对垃圾回收停顿的改善。由于堆外内存是直接受操作系统管理而不是 JVM，所以当我们使用堆外内存时，即可保持较小的堆内内存规模。从而在 GC 时减少回收停顿对于应用的影响。
- 提升程序 I/O 操作的性能。通常在 I/O 通信过程中，会存在堆内内存到堆外内存的数据拷贝操作，对于需要频繁进行内存间数据拷贝且生命周期较短的暂存数据，都建议存储到堆外内存。



`DirectByteBuffer` 是 Java 用于实现堆外内存的一个重要类，通常用在通信过程中做缓冲池，如在 Netty、MINA 等 NIO 框架中应用广泛。`DirectByteBuffer` 对于堆外内存的创建、使用、销毁等逻辑均由 Unsafe 提供的堆外内存 API 来实现。

下图为 `DirectByteBuffer` 构造函数，创建 `DirectByteBuffer` 的时候，通过 `Unsafe.allocateMemory` 分配内存、`Unsafe.setMemory` 进行内存初始化，而后构建 `Cleaner` 对象用于跟踪 `DirectByteBuffer` 对象的垃圾回收，以实现当 `DirectByteBuffer` 被垃圾回收时，分配的堆外内存一起被释放。

```java
DirectByteBuffer(int cap) {                   // package-private

    super(-1, 0, cap, cap);
    boolean pa = VM.isDirectMemoryPageAligned();
    int ps = Bits.pageSize();
    long size = Math.max(1L, (long)cap + (pa ? ps : 0));
    Bits.reserveMemory(size, cap);

    long base = 0;
    try {
        // 分配内存并返回基地址
        base = unsafe.allocateMemory(size);
    } catch (OutOfMemoryError x) {
        Bits.unreserveMemory(size, cap);
        throw x;
    }
    // 内存初始化
    unsafe.setMemory(base, size, (byte) 0);
    if (pa && (base % ps != 0)) {
        // Round up to page boundary
        address = base + ps - (base & (ps - 1));
    } else {
        address = base;
    }
    // 跟踪 DirectByteBuffer 对象的垃圾回收，以实现堆外内存释放
    cleaner = Cleaner.create(this, new Deallocator(base, size, cap));
    att = null;
}
```

#### 内存屏障

在介绍内存屏障前，需要知道编译器和 CPU 会在保证程序输出结果一致的情况下，会对代码进行重排序，从指令优化角度提升性能。而指令重排序可能会带来一个不好的结果，导致 CPU 的高速缓存和内存中数据的不一致，而内存屏障（`Memory Barrier`）就是通过阻止屏障两边的指令重排序从而避免编译器和硬件的不正确优化情况。

在硬件层面上，内存屏障是 CPU 为了防止代码进行重排序而提供的指令，不同的硬件平台上实现内存屏障的方法可能并不相同。在 Java8 中，引入了 3 个内存屏障的函数，它屏蔽了操作系统底层的差异，允许在代码中定义、并统一由 JVM 来生成内存屏障指令，来实现内存屏障的功能。

`Unsafe` 中提供了下面三个内存屏障相关方法：

```java
//内存屏障，禁止load操作重排序。屏障前的load操作不能被重排序到屏障后，屏障后的load操作不能被重排序到屏障前
public native void loadFence();
//内存屏障，禁止store操作重排序。屏障前的store操作不能被重排序到屏障后，屏障后的store操作不能被重排序到屏障前
public native void storeFence();
//内存屏障，禁止load、store操作重排序
public native void fullFence();
```

内存屏障可以看做对内存随机访问的操作中的一个同步点，使得此点之前的所有读写操作都执行后才可以开始执行此点之后的操作。以`loadFence`方法为例，它会禁止读操作重排序，保证在这个屏障之前的所有读操作都已经完成，并且将缓存数据设为无效，重新从主存中进行加载。

看到这估计很多小伙伴们会想到`volatile`关键字了，如果在字段上添加了`volatile`关键字，就能够实现字段在多线程下的可见性。基于读内存屏障，我们也能实现相同的功能。下面定义一个线程方法，在线程中去修改`flag`标志位，注意这里的`flag`是没有被`volatile`修饰的：

```java
@Getter
class ChangeThread implements Runnable{
    /**volatile**/ boolean flag=false;
    @Override
    public void run() {
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("subThread change flag to:" + flag);
        flag = true;
    }
}
```

在主线程的`while`循环中，加入内存屏障，测试是否能够感知到`flag`的修改变化：

```java
public static void main(String[] args){
    ChangeThread changeThread = new ChangeThread();
    new Thread(changeThread).start();
    while (true) {
        boolean flag = changeThread.isFlag();
        unsafe.loadFence(); //加入读内存屏障
        if (flag){
            System.out.println("detected flag changed");
            break;
        }
    }
    System.out.println("main thread end");
}
```

运行结果：

```text
subThread change flag to:false
detected flag changed
main thread end
```

而如果删掉上面代码中的`loadFence`方法，那么主线程将无法感知到`flag`发生的变化，会一直在`while`中循环。可以用图来表示上面的过程：

![image-20220717144703446](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307151801358.png)



了解 Java 内存模型（`JMM`）的小伙伴们应该清楚，运行中的线程不是直接读取主内存中的变量的，只能操作自己工作内存中的变量，然后同步到主内存中，并且线程的工作内存是不能共享的。上面的图中的流程就是子线程借助于主内存，将修改后的结果同步给了主线程，进而修改主线程中的工作空间，跳出循环。



在 Java 8 中引入了一种锁的新机制——`StampedLock`，它可以看成是读写锁的一个改进版本。`StampedLock` 提供了一种乐观读锁的实现，这种乐观读锁类似于无锁的操作，完全不会阻塞写线程获取写锁，从而缓解读多写少时写线程“饥饿”现象。由于 `StampedLock` 提供的乐观读锁不阻塞写线程获取读锁，当线程共享变量从主内存 load 到线程工作内存时，会存在数据不一致问题。

为了解决这个问题，`StampedLock` 的 `validate` 方法会通过 `Unsafe` 的 `loadFence` 方法加入一个 `load` 内存屏障。

```java
public boolean validate(long stamp) {
   U.loadFence();
   return (stamp & SBITS) == (state & SBITS);
}
```

#### 对象操作

```java
import sun.misc.Unsafe;
import java.lang.reflect.Field;

public class Main {

    private int value;

    public static void main(String[] args) throws Exception{
        Unsafe unsafe = reflectGetUnsafe();
        assert unsafe != null;
        long offset = unsafe.objectFieldOffset(Main.class.getDeclaredField("value"));
        Main main = new Main();
        System.out.println("value before putInt: " + main.value);
        unsafe.putInt(main, offset, 42);
        System.out.println("value after putInt: " + main.value);
	System.out.println("value after putInt: " + unsafe.getInt(main, offset));
    }

    private static Unsafe reflectGetUnsafe() {
        try {
            Field field = Unsafe.class.getDeclaredField("theUnsafe");
            field.setAccessible(true);
            return (Unsafe) field.get(null);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

}
```

```text
value before putInt: 0
value after putInt: 42
value after putInt: 42
```

**对象属性**

对象成员属性的内存偏移量获取，以及字段属性值的修改，在上面的例子中我们已经测试过了。除了前面的`putInt`、`getInt`方法外，Unsafe 提供了全部 8 种基础数据类型以及`Object`的`put`和`get`方法，并且所有的`put`方法都可以越过访问权限，直接修改内存中的数据。阅读 openJDK 源码中的注释发现，基础数据类型和`Object`的读写稍有不同，基础数据类型是直接操作的属性值（`value`），而`Object`的操作则是基于引用值（`reference value`）。下面是`Object`的读写方法：

```java
//在对象的指定偏移地址获取一个对象引用
public native Object getObject(Object o, long offset);
//在对象指定偏移地址写入一个对象引用
public native void putObject(Object o, long offset, Object x);
```

除了对象属性的普通读写外，`Unsafe` 还提供了 **volatile 读写**和**有序写入**方法。`volatile`读写方法的覆盖范围与普通读写相同，包含了全部基础数据类型和`Object`类型，以`int`类型为例：

```java
//在对象的指定偏移地址处读取一个int值，支持volatile load语义
public native int getIntVolatile(Object o, long offset);
//在对象指定偏移地址处写入一个int，支持volatile store语义
public native void putIntVolatile(Object o, long offset, int x);
```

相对于普通读写来说，`volatile`读写具有更高的成本，因为它需要保证可见性和有序性。在执行`get`操作时，会强制从主存中获取属性值，在使用`put`方法设置属性值时，会强制将值更新到主存中，从而保证这些变更对其他线程是可见的。

有序写入的方法有以下三个：

```java
public native void putOrderedObject(Object o, long offset, Object x);
public native void putOrderedInt(Object o, long offset, int x);
public native void putOrderedLong(Object o, long offset, long x);
```

有序写入的成本相对`volatile`较低，因为它只保证写入时的有序性，而不保证可见性，也就是一个线程写入的值不能保证其他线程立即可见。为了解决这里的差异性，需要对内存屏障的知识点再进一步进行补充，首先需要了解两个指令的概念：

- `Load`：将主内存中的数据拷贝到处理器的缓存中
- `Store`：将处理器缓存的数据刷新到主内存中

顺序写入与`volatile`写入的差别在于，在顺序写时加入的内存屏障类型为`StoreStore`类型，而在`volatile`写入时加入的内存屏障是`StoreLoad`类型，如下图所示：

![image-20220717144834132](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307151802638.png)

在有序写入方法中，使用的是`StoreStore`屏障，该屏障确保`Store1`立刻刷新数据到内存，这一操作先于`Store2`以及后续的存储指令操作。而在`volatile`写入中，使用的是`StoreLoad`屏障，该屏障确保`Store1`立刻刷新数据到内存，这一操作先于`Load2`及后续的装载指令，并且，`StoreLoad`屏障会使该屏障之前的所有内存访问指令，包括存储指令和访问指令全部完成之后，才执行该屏障之后的内存访问指令。

综上所述，在上面的三类写入方法中，在写入效率方面，按照`put`、`putOrder`、`putVolatile`的顺序效率逐渐降低。

**对象实例化**

使用 `Unsafe` 的 `allocateInstance` 方法，允许我们使用非常规的方式进行对象的实例化，首先定义一个实体类，并且在构造函数中对其成员变量进行赋值操作：

```java
@Data
public class A {
    private int b;
    public A(){
        this.b =1;
    }
}
```

分别基于构造函数、反射以及 `Unsafe` 方法的不同方式创建对象进行比较：

```java
public void objTest() throws Exception{
    A a1=new A();
    System.out.println(a1.getB());
    A a2 = A.class.newInstance();
    System.out.println(a2.getB());
    A a3= (A) unsafe.allocateInstance(A.class);
    System.out.println(a3.getB());
}
```

打印结果分别为 1、1、0，说明通过`allocateInstance`方法创建对象过程中，不会调用类的构造方法。使用这种方式创建对象时，只用到了`Class`对象，所以说如果想要跳过对象的初始化阶段或者跳过构造器的安全检查，就可以使用这种方法。在上面的例子中，如果将 A 类的构造函数改为`private`类型，将无法通过构造函数和反射创建对象，但`allocateInstance`方法仍然有效。



- **常规对象实例化方式**：我们通常所用到的创建对象的方式，从本质上来讲，都是通过 new 机制来实现对象的创建。但是，new 机制有个特点就是当类只提供有参的构造函数且无显示声明无参构造函数时，则必须使用有参构造函数进行对象构造，而使用有参构造函数时，必须传递相应个数的参数才能完成对象实例化。
- **非常规的实例化方式**：而 Unsafe 中提供 allocateInstance 方法，仅通过 Class 对象就可以创建此类的实例对象，而且不需要调用其构造函数、初始化代码、JVM 安全检查等。它抑制修饰符检测，也就是即使构造器是 private 修饰的也能通过此方法实例化，只需提类对象即可创建相应的对象。由于这种特性，allocateInstance 在 java.lang.invoke、Objenesis（提供绕过类构造器的对象生成方式）、Gson（反序列化时用到）中都有相应的应用。



#### 数组操作

`arrayBaseOffset` 与 `arrayIndexScale` 这两个方法配合起来使用，即可定位数组中每个元素在内存中的位置。

```java
//返回数组中第一个元素的偏移地址
public native int arrayBaseOffset(Class<?> arrayClass);
//返回数组中一个元素占用的大小
public native int arrayIndexScale(Class<?> arrayClass);
```

这两个与数据操作相关的方法，在 `java.util.concurrent.atomic` 包下的 `AtomicIntegerArray`（可以实现对 `Integer` 数组中每个元素的原子性操作）中有典型的应用，如下图 `AtomicIntegerArray` 源码所示，通过 `Unsafe` 的 `arrayBaseOffset`、`arrayIndexScale` 分别获取数组首元素的偏移地址 `base` 及单个元素大小因子 `scale` 。后续相关原子性操作，均依赖于这两个值进行数组中元素的定位，如下图二所示的 `getAndAdd` 方法即通过 `checkedByteOffset` 方法获取某数组元素的偏移地址，而后通过 CAS 实现原子性操作。

![image-20220717144927257](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307151809677.png)



#### CAS 操作

这部分主要为 CAS 相关操作的方法。

```java
/**
	*  CAS
  * @param o         包含要修改field的对象
  * @param offset    对象中某field的偏移量
  * @param expected  期望值
  * @param update    更新值
  * @return          true | false
  */
public final native boolean compareAndSwapObject(Object o, long offset,  Object expected, Object update);

public final native boolean compareAndSwapInt(Object o, long offset, int expected,int update);

public final native boolean compareAndSwapLong(Object o, long offset, long expected, long update);
```

**什么是 CAS?** CAS 即比较并替换（Compare And Swap)，是实现并发算法时常用到的一种技术。CAS 操作包含三个操作数——内存位置、预期原值及新值。执行 CAS 操作的时候，将内存位置的值与预期原值比较，如果相匹配，那么处理器会自动将该位置值更新为新值，否则，处理器不做任何操作。我们都知道，CAS 是一条 CPU 的原子指令（cmpxchg 指令），不会造成所谓的数据不一致问题，`Unsafe` 提供的 CAS 方法（如 `compareAndSwapXXX`）底层实现即为 CPU 指令 `cmpxchg` 。



在 JUC 包的并发工具类中大量地使用了 CAS 操作，像在前面介绍`synchronized`和`AQS`的文章中也多次提到了 CAS，其作为乐观锁在并发工具类中广泛发挥了作用。在 `Unsafe` 类中，提供了`compareAndSwapObject`、`compareAndSwapInt`、`compareAndSwapLong`方法来实现的对`Object`、`int`、`long`类型的 CAS 操作。以`compareAndSwapInt`方法为例：

```java
public final native boolean compareAndSwapInt(Object o, long offset,int expected,int x);
```

参数中`o`为需要更新的对象，`offset`是对象`o`中整形字段的偏移量，如果这个字段的值与`expected`相同，则将字段的值设为`x`这个新值，并且此更新是不可被中断的，也就是一个原子操作。下面是一个使用`compareAndSwapInt`的例子：

```java
private volatile int a;
public static void main(String[] args){
    CasTest casTest=new CasTest();
    new Thread(()->{
        for (int i = 1; i < 5; i++) {
            casTest.increment(i);
            System.out.print(casTest.a+" ");
        }
    }).start();
    new Thread(()->{
        for (int i = 5 ; i <10 ; i++) {
            casTest.increment(i);
            System.out.print(casTest.a+" ");
        }
    }).start();
}

private void increment(int x){
    while (true){
        try {
            long fieldOffset = unsafe.objectFieldOffset(CasTest.class.getDeclaredField("a"));
            if (unsafe.compareAndSwapInt(this,fieldOffset,x-1,x))
                break;
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        }
    }
}
```

```text
1 2 3 4 5 6 7 8 9
```

在上面的例子中，使用两个线程去修改`int`型属性`a`的值，并且只有在`a`的值等于传入的参数`x`减一时，才会将`a`的值变为`x`，也就是实现对`a`的加一的操作。流程如下所示：

![image-20220717144939826](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307151810940.png)



#### 线程调度

`Unsafe` 类中提供了`park`、`unpark`、`monitorEnter`、`monitorExit`、`tryMonitorEnter`方法进行线程调度。

```java
//取消阻塞线程
public native void unpark(Object thread);
//阻塞线程
public native void park(boolean isAbsolute, long time);
//获得对象锁（可重入锁）
@Deprecated
public native void monitorEnter(Object o);
//释放对象锁
@Deprecated
public native void monitorExit(Object o);
//尝试获取对象锁
@Deprecated
public native boolean tryMonitorEnter(Object o);
```

方法 `park`、`unpark` 即可实现线程的挂起与恢复，将一个线程进行挂起是通过 `park` 方法实现的，调用 `park` 方法后，线程将一直阻塞直到超时或者中断等条件出现；`unpark` 可以终止一个挂起的线程，使其恢复正常。

此外，`Unsafe` 源码中`monitor`相关的三个方法已经被标记为`deprecated`，不建议被使用：

```java
//获得对象锁
@Deprecated
public native void monitorEnter(Object var1);
//释放对象锁
@Deprecated
public native void monitorExit(Object var1);
//尝试获得对象锁
@Deprecated
public native boolean tryMonitorEnter(Object var1);
```

`monitorEnter`方法用于获得对象锁，`monitorExit`用于释放对象锁，如果对一个没有被`monitorEnter`加锁的对象执行此方法，会抛出`IllegalMonitorStateException`异常。`tryMonitorEnter`方法尝试获取对象锁，如果成功则返回`true`，反之返回`false`。



Java 锁和同步器框架的核心类 `AbstractQueuedSynchronizer` (AQS)，就是通过调用`LockSupport.park()`和`LockSupport.unpark()`实现线程的阻塞和唤醒的，而 `LockSupport` 的 `park`、`unpark` 方法实际是调用 `Unsafe` 的 `park`、`unpark` 方式实现的。

```java
public static void park(Object blocker) {
    Thread t = Thread.currentThread();
    setBlocker(t, blocker);
    UNSAFE.park(false, 0L);
    setBlocker(t, null);
}
public static void unpark(Thread thread) {
    if (thread != null)
        UNSAFE.unpark(thread);
}
```

`LockSupport` 的`park`方法调用了 `Unsafe` 的`park`方法来阻塞当前线程，此方法将线程阻塞后就不会继续往后执行，直到有其他线程调用`unpark`方法唤醒当前线程。下面的例子对 `Unsafe` 的这两个方法进行测试：

```java
public static void main(String[] args) {
    Thread mainThread = Thread.currentThread();
    new Thread(()->{
        try {
            TimeUnit.SECONDS.sleep(5);
            System.out.println("subThread try to unpark mainThread");
            unsafe.unpark(mainThread);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }).start();

    System.out.println("park main mainThread");
    unsafe.park(false,0L);
    System.out.println("unpark mainThread success");
}
```

```text
park main mainThread
subThread try to unpark mainThread
unpark mainThread success
```

程序运行的流程也比较容易看懂，子线程开始运行后先进行睡眠，确保主线程能够调用`park`方法阻塞自己，子线程在睡眠 5 秒后，调用`unpark`方法唤醒主线程，使主线程能继续向下执行。整个流程如下图所示

![image-20220717144950116](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307151813924.png)



#### Class 操作

`Unsafe` 对`Class`的相关操作主要包括类加载和静态变量的操作方法。

**静态属性读取相关的方法**

```java
//获取静态属性的偏移量
public native long staticFieldOffset(Field f);
//获取静态属性的对象指针
public native Object staticFieldBase(Field f);
//判断类是否需要初始化（用于获取类的静态属性前进行检测）
public native boolean shouldBeInitialized(Class<?> c);
```

创建一个包含静态属性的类，进行测试：

```java
@Data
public class User {
    public static String name="Hydra";
    int age;
}
private void staticTest() throws Exception {
    User user=new User();
    // 也可以用下面的语句触发类初始化
    // 1.
    // unsafe.ensureClassInitialized(User.class);
    // 2.
    // System.out.println(User.name);
    System.out.println(unsafe.shouldBeInitialized(User.class));
    Field sexField = User.class.getDeclaredField("name");
    long fieldOffset = unsafe.staticFieldOffset(sexField);
    Object fieldBase = unsafe.staticFieldBase(sexField);
    Object object = unsafe.getObject(fieldBase, fieldOffset);
    System.out.println(object);
}
```

运行结果：

```text
falseHydra
```

在 `Unsafe` 的对象操作中，我们学习了通过`objectFieldOffset`方法获取对象属性偏移量并基于它对变量的值进行存取，但是它不适用于类中的静态属性，这时候就需要使用`staticFieldOffset`方法。在上面的代码中，只有在获取`Field`对象的过程中依赖到了`Class`，而获取静态变量的属性时不再依赖于`Class`。

在上面的代码中首先创建一个`User`对象，这是因为如果一个类没有被初始化，那么它的静态属性也不会被初始化，最后获取的字段属性将是`null`。所以在获取静态属性前，需要调用`shouldBeInitialized`方法，判断在获取前是否需要初始化这个类。如果删除创建 User 对象的语句，运行结果会变为：

```text
truenull
```

**使用`defineClass`方法允许程序在运行时动态地创建一个类**

```java
public native Class<?> defineClass(String name, byte[] b, int off, int len, ClassLoader loader,ProtectionDomain protectionDomain);
```

在实际使用过程中，可以只传入字节数组、起始字节的下标以及读取的字节长度，默认情况下，类加载器（`ClassLoader`）和保护域（`ProtectionDomain`）来源于调用此方法的实例。下面的例子中实现了反编译生成后的 class 文件的功能：

```java
private static void defineTest() {
    String fileName="F:\\workspace\\unsafe-test\\target\\classes\\com\\cn\\model\\User.class";
    File file = new File(fileName);
    try(FileInputStream fis = new FileInputStream(file)) {
        byte[] content=new byte[(int)file.length()];
        fis.read(content);
        Class clazz = unsafe.defineClass(null, content, 0, content.length, null, null);
        Object o = clazz.newInstance();
        Object age = clazz.getMethod("getAge").invoke(o, null);
        System.out.println(age);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

在上面的代码中，首先读取了一个`class`文件并通过文件流将它转化为字节数组，之后使用`defineClass`方法动态的创建了一个类，并在后续完成了它的实例化工作，流程如下图所示，并且通过这种方式创建的类，会跳过 JVM 的所有安全检查。

![image-20220717145000710](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307151816466.png)



除了`defineClass`方法外，Unsafe 还提供了一个`defineAnonymousClass`方法：

```java
public native Class<?> defineAnonymousClass(Class<?> hostClass, byte[] data, Object[] cpPatches);
```

使用该方法可以用来动态的创建一个匿名类，在`Lambda`表达式中就是使用 ASM 动态生成字节码，然后利用该方法定义实现相应的函数式接口的匿名类。在 JDK 15 发布的新特性中，在隐藏类（`Hidden classes`）一条中，指出将在未来的版本中弃用 `Unsafe` 的`defineAnonymousClass`方法。

Lambda 表达式实现需要依赖 `Unsafe` 的 `defineAnonymousClass` 方法定义实现相应的函数式接口的匿名类。



#### 系统信息

这部分包含两个获取系统相关信息的方法。

```java
//返回系统指针的大小。返回值为4（32位系统）或 8（64位系统）。
public native int addressSize();
//内存页的大小，此值为2的幂次方。
public native int pageSize();
```

这两个方法的应用场景比较少，在`java.nio.Bits`类中，在使用`pageCount`计算所需的内存页的数量时，调用了`pageSize`方法获取内存页的大小。另外，在使用`copySwapMemory`方法拷贝内存时，调用了`addressSize`方法，检测 32 位系统的情况。



## 集合

### 集合概述

#### Java 集合概览

Java 集合， 也叫作容器，主要是由两大接口派生而来：一个是 `Collection`接口，主要用于存放单一元素；另一个是 `Map` 接口，主要用于存放键值对。对于`Collection` 接口，下面又有三个主要的子接口：`List`、`Set` 和 `Queue`。

Java 集合框架如下图所示：

![java-collection-hierarchy](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307152120339.png)

注：图中只列举了主要的继承派生关系，并没有列举所有关系。



####  Collection 和 Collections 的区别。

`Collection` 和 `Collections` 是 Java 中的两个不同概念：

1. `Collection`：
   - `Collection` 是 Java 单列集合框架的顶层接口，表示一组对象的集合，用于存储和操作多个对象。
   - 它继承自 `java.util.Iterable` 接口，因此可以使用迭代器来遍历其中的元素。
   - `Collection` 接口的常见实现类包括 `List`、`Set` 和 `Queue` 等。

2. `Collections`：
   - `Collections` 是 Java 提供的一个实用类，包含一些静态方法，用于操作集合对象。
   - 这些静态方法提供了对集合的排序、搜索、替换、同步等操作，方便对集合进行管理和处理。
   - 不要与 `Collection` 混淆，`Collections` 类中的方法不是对集合对象的操作，而是通过传入集合对象来进行相关操作。

总结：`Collection` 是 Java 集合框架的接口，用于表示一组对象的集合；`Collections` 是 Java 提供的实用类，用于操作集合对象。





#### 说说 List, Set, Queue, Map 四者的区别？

- `List`(对付顺序的好帮手): 有序、可重复。
- `Set`(注重独一无二的性质): 无序的、不重复。
- `Queue`(实现排队功能的叫号机): 按特定的排队规则来确定先后顺序，有序、可重复。
- `Map`(用 key 来搜索的专家): 使用键值对（key-value）存储，key 是无序的、不可重复的，value 是无序的、可重复的，每个键最多映射到一个值。



#### 集合框架底层数据结构总结

`Collection` 接口下面的集合。

##### List

- `ArrayList`：`Object[]` 数组
- `Vector`：`Object[]` 数组
- `LinkedList`：双向链表(JDK1.6 之前为循环链表，JDK1.7 取消了循环)

##### Set

- `HashSet`(无序，唯一): 基于 `HashMap` 实现的，底层采用 `HashMap` 来保存元素
- `LinkedHashSet`: `LinkedHashSet` 是 `HashSet` 的子类，并且其内部是通过 `LinkedHashMap` 来实现的。有点类似于我们之前说的 `LinkedHashMap` 其内部是基于 `HashMap` 实现一样，不过还是有一点点区别的
- `TreeSet`(有序，唯一): 红黑树(自平衡的排序二叉树)

##### Queue

- `PriorityQueue`: `Object[]` 数组来实现二叉堆
- `ArrayQueue`: `Object[]` 数组 + 双指针





`Map` 接口下面的集合。

##### Map

- `HashMap`：JDK1.8 之前 `HashMap` 由数组+链表组成的，数组是 `HashMap` 的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）。JDK1.8 以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间
- `LinkedHashMap`：`LinkedHashMap` 继承自 `HashMap`，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，`LinkedHashMap` 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。`Hashtable`：数组+链表组成的，数组是 `Hashtable` 的主体，链表则是主要为了解决哈希冲突而存在的
- `TreeMap`：红黑树（自平衡的排序二叉树）



#### 如何选用集合?

根据集合的特点来选择合适的集合。比如：

- 我们需要根据键值获取到元素值时就选用 `Map` 接口下的集合，需要排序时选择 `TreeMap`,不需要排序时就选择 `HashMap`,需要保证线程安全就选用 `ConcurrentHashMap`。
- 我们只需要存放元素值时，就选择实现`Collection` 接口的集合，需要保证元素唯一时选择实现 `Set` 接口的集合比如 `TreeSet` 或 `HashSet`，不需要就选择实现 `List` 接口的比如 `ArrayList` 或 `LinkedList`，然后再根据实现这些接口的集合的特点来选用。



#### 为什么要使用集合？

当我们需要存储一组类型相同的数据时，数组是最常用且最基本的容器之一。但是，使用数组存储对象存在一些不足之处，因为在实际开发中，存储的数据类型多种多样且数量不确定。这时，Java 集合就派上用场了。与数组相比，Java 集合提供了更灵活、更有效的方法来存储多个数据对象。Java 集合框架中的各种集合类和接口可以存储不同类型和数量的对象，同时还具有多样化的操作方式。相较于数组，Java 集合的优势在于它们的大小可变、支持泛型、具有内建算法等。总的来说，Java 集合提高了数据的存储和处理灵活性，可以更好地适应现代软件开发中多样化的数据需求，并支持高质量的代码编写。



### List

#### ArrayList 和 Array（数组）的区别？

`ArrayList` 内部基于动态数组实现，比 `Array`（静态数组） 使用起来更加灵活：

- `ArrayList`会根据实际存储的元素动态地扩容或缩容，而 `Array` 被创建之后就不能改变它的长度了。
- `ArrayList` 允许使用泛型来确保类型安全，`Array` 则不可以。
- `ArrayList` 中只能存储对象。对于基本类型数据，需要使用其对应的包装类（如 Integer、Double 等）。`Array` 可以直接存储基本类型数据，也可以存储对象。
- `ArrayList` 支持插入、删除、遍历等常见操作，并且提供了丰富的 API 操作方法。`Array` 只是一个固定长度的数组，只能按照下标访问其中的元素，不具备动态添加、删除元素的能力。
- `ArrayList`创建时不需要指定大小，而`Array`创建时必须指定大小。



下面是二者使用的简单对比：

`Array`：

```java
 // 初始化一个 String 类型的数组
 String[] stringArr = new String[]{"hello", "world", "!"};
 // 修改数组元素的值
 stringArr[0] = "goodbye";
 System.out.println(Arrays.toString(stringArr));// [goodbye, world, !]
 // 删除数组中的元素，需要手动移动后面的元素
 for (int i = 0; i < stringArr.length - 1; i++) {
     stringArr[i] = stringArr[i + 1];
 }
 stringArr[stringArr.length - 1] = null;
 System.out.println(Arrays.toString(stringArr));// [world, !, null]
```

`ArrayList` ：

```java
// 初始化一个 String 类型的 ArrayList
 ArrayList<String> stringList = new ArrayList<>(Arrays.asList("hello", "world", "!"));
// 添加元素到 ArrayList 中
 stringList.add("goodbye");
 System.out.println(stringList);// [hello, world, !, goodbye]
 // 修改 ArrayList 中的元素
 stringList.set(0, "hi");
 System.out.println(stringList);// [hi, world, !, goodbye]
 // 删除 ArrayList 中的元素
 stringList.remove(0);
 System.out.println(stringList); // [world, !, goodbye]
```

#### ArrayList 和 Vector 的区别?

- `ArrayList` 是 `List` 的主要实现类，底层使用 `Object[]`存储，适用于频繁的查找工作，线程不安全 。
- `Vector` 是 `List` 的古老实现类，底层使用`Object[]` 存储，线程安全。



#### Vector 和 Stack 的区别?

- `Vector` 和 `Stack` 两者都是线程安全的，都是使用 `synchronized` 关键字进行同步处理。
- `Stack` 继承自 `Vector`，是一个后进先出的栈，而 `Vector` 是一个列表。



随着 Java 并发编程的发展，`Vector` 和 `Stack` 已经被淘汰，推荐使用并发集合类（例如 `ConcurrentHashMap`、`CopyOnWriteArrayList` 等）或者手动实现线程安全的方法来提供安全的多线程操作支持。



#### ArrayList 可以添加 null 值吗？

`ArrayList` 中可以存储任何类型的对象，包括 `null` 值。不过，不建议向`ArrayList` 中添加 `null` 值， `null` 值无意义，会让代码难以维护比如忘记做判空处理就会导致空指针异常。

```java
ArrayList<String> listOfStrings = new ArrayList<>();
listOfStrings.add(null);
listOfStrings.add("java");
System.out.println(listOfStrings);
```

```text
[null, java]
```



#### ArrayList 插入和删除元素的时间复杂度？

对于插入：

- 头部插入：由于需要将所有元素都依次向后移动一个位置，因此时间复杂度是 O(n)。
- 尾部插入：当 `ArrayList` 的容量未达到极限时，往列表末尾插入元素的时间复杂度是 O(1)，因为它只需要在数组末尾添加一个元素即可；当容量已达到极限并且需要扩容时，则需要执行一次 O(n) 的操作将原数组复制到新的更大的数组中，然后再执行 O(1) 的操作添加元素。
- 指定位置插入：需要将目标位置之后的所有元素都向后移动一个位置，然后再把新元素放入指定位置。这个过程需要移动平均 n/2 个元素，因此时间复杂度为 O(n)。



对于删除：

- 头部删除：由于需要将所有元素依次向前移动一个位置，因此时间复杂度是 O(n)。
- 尾部删除：当删除的元素位于列表末尾时，时间复杂度为 O(1)。
- 指定位置删除：需要将目标元素之后的所有元素向前移动一个位置以填补被删除的空白位置，因此需要移动平均 n/2 个元素，时间复杂度为 O(n)。

```java
// ArrayList的底层数组大小为10，此时存储了7个元素
+---+---+---+---+---+---+---+---+---+---+
| 1 | 2 | 3 | 4 | 5 | 6 | 7 |   |   |   |
+---+---+---+---+---+---+---+---+---+---+
  0   1   2   3   4   5   6   7   8   9
// 在索引为1的位置插入一个元素8，该元素后面的所有元素都要向右移动一位
+---+---+---+---+---+---+---+---+---+---+
| 1 | 8 | 2 | 3 | 4 | 5 | 6 | 7 |   |   |
+---+---+---+---+---+---+---+---+---+---+
  0   1   2   3   4   5   6   7   8   9
// 删除索引为1的位置的元素，该元素后面的所有元素都要向左移动一位
+---+---+---+---+---+---+---+---+---+---+
| 1 | 2 | 3 | 4 | 5 | 6 | 7 |   |   |   |
+---+---+---+---+---+---+---+---+---+---+
  0   1   2   3   4   5   6   7   8   9
```



#### LinkedList 插入和删除元素的时间复杂度？

- 头部插入/删除：只需要修改头结点的指针即可完成插入/删除操作，因此时间复杂度为 O(1)。
- 尾部插入/删除：只需要修改尾结点的指针即可完成插入/删除操作，因此时间复杂度为 O(1)。
- 指定位置插入/删除：需要先移动到指定位置，再修改指定节点的指针完成插入/删除，因此需要移动平均 n/2 个元素，时间复杂度为 O(n)。

这里简单列举一个例子：假如我们要删除节点9 的话，需要先遍历链表找到该节点。然后，再执行相应节点指针指向的更改。

![linkedlist-unlink](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307152233882.jpg)





#### LinkedList 为什么不能实现 RandomAccess 接口？

`RandomAccess` 是一个标记接口，用来表明实现该接口的类支持随机访问（即可以通过索引快速访问元素）。由于 `LinkedList` 底层数据结构是链表，内存地址不连续，只能通过指针来定位，不支持随机快速访问，所以不能实现 `RandomAccess` 接口。



#### ArrayList 与 LinkedList 区别?

- **是否保证线程安全：** `ArrayList` 和 `LinkedList` 都是不同步的，也就是不保证线程安全；
- **底层数据结构：** `ArrayList` 底层使用的是 **`Object` 数组**；`LinkedList` 底层使用的是 **双向链表**（JDK1.6 之前为循环链表，JDK1.7 取消了循环。）
- 插入和删除是否受元素位置的影响：
  - `ArrayList` 采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。 比如：执行`add(E e)`方法的时候， `ArrayList` 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是 O(1)。但是如果要在指定位置 i 插入和删除元素的话（`add(int index, E element)`），时间复杂度就为 O(n)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。
  - `LinkedList` 采用链表存储，所以在头尾插入或者删除元素不受元素位置的影响（`add(E e)`、`addFirst(E e)`、`addLast(E e)`、`removeFirst()`、 `removeLast()`），时间复杂度为 O(1)，如果是要在指定位置 `i` 插入和删除元素的话（`add(int index, E element)`，`remove(Object o)`,`remove(int index)`）， 时间复杂度为 O(n) ，因为需要先移动到指定位置再插入和删除。
- **是否支持快速随机访问：** `LinkedList` 不支持高效的随机元素访问，而 `ArrayList`（实现了 `RandomAccess` 接口） 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于`get(int index)`方法)。
- **内存空间占用：** `ArrayList` 的空间浪费主要体现在在 list 列表的结尾会预留一定的容量空间，而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间（因为要存放直接后继和直接前驱以及数据）。



> 我们在项目中一般是不会使用到 `LinkedList` 的，需要用到 `LinkedList` 的场景几乎都可以使用 `ArrayList` 来代替，并且，性能通常会更好！就连 `LinkedList` 的作者约书亚 · 布洛克（Josh Bloch）自己都说从来不会使用 `LinkedList` 。
>
> ------
>
> 另外，不要下意识地认为 `LinkedList` 作为链表就最适合元素增删的场景`LinkedList` 仅仅在头尾插入或者删除元素的时候时间复杂度近似 O(1)，其他情况增删元素的平均时间复杂度都是 O(n) 。



#### 双向链表和双向循环链表

**双向链表：** 包含两个指针，一个 prev 指向前一个节点，一个 next 指向后一个节点。

![bidirectional-linkedlist](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307152239988.png)

**双向循环链表：** 最后一个节点的 next 指向 head，而 head 的 prev 指向最后一个节点，构成一个环。

![bidirectional-circular-linkedlist](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307152239407.png)



#### RandomAccess 接口

```java
public interface RandomAccess {}
```

查看源码我们发现实际上 `RandomAccess` 接口中什么都没有定义。所以，`RandomAccess` 接口不过是一个标识罢了。标识实现这个接口的类具有随机访问功能。

在 `binarySearch（)` 方法中，它要判断传入的 list 是否 `RandomAccess` 的实例，如果是，调用`indexedBinarySearch()`方法，如果不是，那么调用`iteratorBinarySearch()`方法

```java
    public static <T>
    int binarySearch(List<? extends Comparable<? super T>> list, T key) {
        if (list instanceof RandomAccess || list.size()<BINARYSEARCH_THRESHOLD)
            return Collections.indexedBinarySearch(list, key);
        else
            return Collections.iteratorBinarySearch(list, key);
    }
```

`ArrayList` 实现了 `RandomAccess` 接口， 而 `LinkedList` 没有实现。为什么呢？和底层数据结构有关！`ArrayList` 底层是数组，而 `LinkedList` 底层是链表。数组天然支持随机访问，时间复杂度为 O(1)，所以称为快速随机访问。链表需要遍历到特定位置才能访问特定位置的元素，时间复杂度为 O(n)，所以不支持快速随机访问。`ArrayList` 实现了 `RandomAccess` 接口，就表明了他具有快速随机访问功能。 `RandomAccess` 接口只是标识，并不是说 `ArrayList` 实现 `RandomAccess` 接口才具有快速随机访问功能的！



### Set

#### Comparable 和 Comparator 的区别

`Comparable` 接口和 `Comparator` 接口都是 Java 中用于排序的接口

- `Comparable` 接口实际上是出自`java.lang`包，它有一个 `compareTo(Object obj)`方法用来排序
- `Comparator`接口实际上是出自 `java.util` 包，它有一个`compare(Object obj1, Object obj2)`方法用来排序

一般我们需要对一个集合使用自定义排序时，我们就要重写`compareTo()`方法或`compare()`方法，当我们需要对某一个集合实现两种排序方式，比如一个 `song` 对象中的歌名和歌手名分别采用一种排序方法的话，我们可以重写`compareTo()`方法和使用自制的`Comparator`方法或者以两个 `Comparator` 来实现歌名排序和歌星名排序，第二种代表我们只能使用两个参数版的 `Collections.sort()`。



Comparator 定制排序

```java
ArrayList<Integer> arrayList = new ArrayList<Integer>();
arrayList.add(-1);
arrayList.add(3);
arrayList.add(3);
arrayList.add(-5);
arrayList.add(7);
arrayList.add(4);
arrayList.add(-9);
arrayList.add(-7);
System.out.println("原始数组:");
System.out.println(arrayList);
// void reverse(List list)：反转
Collections.reverse(arrayList);
System.out.println("Collections.reverse(arrayList):");
System.out.println(arrayList);

// void sort(List list),按自然排序的升序排序
Collections.sort(arrayList);
System.out.println("Collections.sort(arrayList):");
System.out.println(arrayList);
// 定制排序的用法
Collections.sort(arrayList, new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
        return o2.compareTo(o1);
    }
});
System.out.println("定制排序后：");
System.out.println(arrayList);
```

```text
原始数组:
[-1, 3, 3, -5, 7, 4, -9, -7]
Collections.reverse(arrayList):
[-7, -9, 4, 7, -5, 3, 3, -1]
Collections.sort(arrayList):
[-9, -7, -5, -1, 3, 3, 4, 7]
定制排序后：
[7, 4, 3, 3, -1, -5, -7, -9]
```



重写 compareTo 方法实现按年龄来排序

```java
// person对象没有实现Comparable接口，所以必须实现，这样才不会出错，才可以使treemap中的数据按顺序排列
// 前面一个例子的String类已经默认实现了Comparable接口，详细可以查看String类的API文档，另外其他
// 像Integer类等都已经实现了Comparable接口，所以不需要另外实现了
@Data
@NoArgsConstructor
@AllArgsConstructor
public  class Person implements Comparable<Person> {
    private String name;
    private int age;

    /**
     * T重写compareTo方法实现按年龄来排序
     */
    @Override
    public int compareTo(Person o) {
        if (this.age > o.getAge()) {
            return 1;
        }
        if (this.age < o.getAge()) {
            return -1;
        }
        return 0;
    }
}
    public static void main(String[] args) {
        TreeMap<Person, String> pdata = new TreeMap<Person, String>();
        pdata.put(new Person("张三", 30), "zhangsan");
        pdata.put(new Person("李四", 20), "lisi");
        pdata.put(new Person("王五", 10), "wangwu");
        pdata.put(new Person("小红", 5), "xiaohong");
        // 得到key的值的同时得到key所对应的值
        Set<Person> keys = pdata.keySet();
        for (Person key : keys) {
            System.out.println(key.getAge() + "-" + key.getName());

        }
    }
```

```text
5-小红
10-王五
20-李四
30-张三
```



#### 无序性和不可重复性的含义是什么

- 无序性不等于随机性 ，无序性是指存储的数据在底层数组中并非按照数组索引的顺序添加 ，而是根据数据的哈希值决定的。
- 不可重复性是指添加的元素按照 `equals()` 判断时 ，返回 false，需要同时重写 `equals()` 方法和 `hashCode()` 方法。



#### 比较 HashSet、LinkedHashSet 和 TreeSet 三者的异同

- `HashSet`、`LinkedHashSet` 和 `TreeSet` 都是 `Set` 接口的实现类，都能保证元素唯一，并且都不是线程安全的。
- `HashSet`、`LinkedHashSet` 和 `TreeSet` 的主要区别在于底层数据结构不同。`HashSet` 的底层数据结构是哈希表（基于 `HashMap` 实现）。`LinkedHashSet` 的底层数据结构是链表和哈希表，元素的插入和取出顺序满足 *FIFO*。`TreeSet` 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序。
- 底层数据结构不同又导致这三者的应用场景不同。`HashSet` 用于不需要保证元素插入和取出顺序的场景，`LinkedHashSet` 用于保证元素的插入和取出顺序满足 FIFO 的场景，`TreeSet` 用于支持对元素自定义排序规则的场景。



### Queue

#### Queue 与 Deque 的区别

`Queue` 是单端队列，只能从一端插入元素，另一端删除元素，实现上一般遵循 **先进先出（FIFO）** 规则。

`Queue` 扩展了 `Collection` 的接口，根据 **因为容量问题而导致操作失败后处理方式的不同** 可以分为两类方法: 一种在操作失败后会抛出异常，另一种则会返回特殊值。

| `Queue` 接口 | 抛出异常  | 返回特殊值 |
| ------------ | --------- | ---------- |
| 插入队尾     | add(E e)  | offer(E e) |
| 删除队首     | remove()  | poll()     |
| 查询队首元素 | element() | peek()     |



`Deque` 是双端队列，在队列的两端均可以插入或删除元素。

`Deque` 扩展了 `Queue` 的接口, 增加了在队首和队尾进行插入和删除的方法，同样根据失败后处理方式的不同分为两类：

| `Deque` 接口 | 抛出异常      | 返回特殊值      |
| ------------ | ------------- | --------------- |
| 插入队首     | addFirst(E e) | offerFirst(E e) |
| 插入队尾     | addLast(E e)  | offerLast(E e)  |
| 删除队首     | removeFirst() | pollFirst()     |
| 删除队尾     | removeLast()  | pollLast()      |
| 查询队首元素 | getFirst()    | peekFirst()     |
| 查询队尾元素 | getLast()     | peekLast()      |

事实上，`Deque` 还提供有 `push()` 和 `pop()` 等其他方法，可用于模拟栈。



#### ArrayDeque 与 LinkedList 的区别

`ArrayDeque` 和 `LinkedList` 都实现了 `Deque` 接口，两者都具有队列的功能，但两者有什么区别呢？

- `ArrayDeque` 是基于可变长的数组和双指针来实现，而 `LinkedList` 则通过链表来实现。
- `ArrayDeque` 不支持存储 `NULL` 数据，但 `LinkedList` 支持。
- `ArrayDeque` 是在 JDK1.6 才被引入的，而`LinkedList` 早在 JDK1.2 时就已经存在。
- `ArrayDeque` 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 `LinkedList` 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。

从性能的角度上，选用 `ArrayDeque` 来实现队列要比 `LinkedList` 更好。此外，`ArrayDeque` 也可以用于实现栈。



#### 说一说 PriorityQueue

`PriorityQueue` 是在 JDK1.5 中被引入的, 其与 `Queue` 的区别在于元素出队顺序是与优先级相关的，即总是优先级最高的元素先出队。

- `PriorityQueue` 利用了二叉堆的数据结构来实现的，底层使用可变长的数组来存储数据
- `PriorityQueue` 通过堆元素的上浮和下沉，实现了在 O(logn) 的时间复杂度内插入元素和删除堆顶元素。
- `PriorityQueue` 是非线程安全的，且不支持存储 `NULL` 和 `non-comparable` 的对象。
- `PriorityQueue` 默认是小顶堆，但可以接收一个 `Comparator` 作为构造参数，从而来自定义元素优先级的先后。

> `PriorityQueue` 在面试中可能更多的会出现在手撕算法的时候，典型例题包括堆排序、求第 K 大的数、带权图的遍历等，需要熟练使用。



#### 什么是 BlockingQueue？

`BlockingQueue` （阻塞队列）是一个接口，继承自 `Queue`。`BlockingQueue`阻塞的原因是其支持当队列没有元素时一直阻塞，直到有元素；还支持如果队列已满，一直等到队列可以放入新元素时再放入。

```java
public interface BlockingQueue<E> extends Queue<E> {...}
```

`BlockingQueue` 常用于生产者-消费者模型中，生产者线程会向队列中添加数据，而消费者线程会从队列中取出数据进行处理。

![blocking-queue](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307152304211.png)



#### BlockingQueue 的实现类有哪些？

![blocking-queue-hierarchy](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307152306915.png)

Java 中常用的阻塞队列实现类有以下几种：

1. `ArrayBlockingQueue`：使用数组实现的有界阻塞队列。在创建时需要指定容量大小，并支持公平和非公平两种方式的锁访问机制。
2. `LinkedBlockingQueue`：使用单向链表实现的可选有界阻塞队列。在创建时可以指定容量大小，如果不指定则默认为`Integer.MAX_VALUE`。和`ArrayBlockingQueue`类似， 它也支持公平和非公平的锁访问机制。
3. `PriorityBlockingQueue`：支持优先级排序的无界阻塞队列。元素必须实现`Comparable`接口或者在构造函数中传入`Comparator`对象，并且不能插入 null 元素。
4. `SynchronousQueue`：同步队列，是一种不存储元素的阻塞队列。每个插入操作都必须等待对应的删除操作，反之删除操作也必须等待插入操作。因此，`SynchronousQueue`通常用于线程之间的直接传递数据。
5. `DelayQueue`：延迟队列，其中的元素只有到了其指定的延迟时间，才能够从队列中出队。



#### ArrayBlockingQueue 和 LinkedBlockingQueue 有什么区别？

`ArrayBlockingQueue` 和 `LinkedBlockingQueue` 是 Java 并发包中常用的两种阻塞队列实现，它们都是线程安全的。不过，不过它们之间也存在下面这些区别：

- 底层实现：`ArrayBlockingQueue` 基于数组实现，而 `LinkedBlockingQueue` 基于链表实现。
- 是否有界：`ArrayBlockingQueue` 是有界队列，必须在创建时指定容量大小。`LinkedBlockingQueue` 创建时可以不指定容量大小，默认是`Integer.MAX_VALUE`，也就是无界的。但也可以指定队列大小，从而成为有界的。
- 锁是否分离： `ArrayBlockingQueue`中的锁是没有分离的，即生产和消费用的是同一个锁；`LinkedBlockingQueue`中的锁是分离的，即生产用的是`putLock`，消费是`takeLock`，这样可以防止生产者和消费者线程之间的锁争夺。
- 内存占用：`ArrayBlockingQueue` 需要提前分配数组内存，而 `LinkedBlockingQueue` 则是动态分配链表节点内存。这意味着，`ArrayBlockingQueue` 在创建时就会占用一定的内存空间，且往往申请的内存比实际所用的内存更大，而`LinkedBlockingQueue` 则是根据元素的增加而逐渐占用内存空间。



### Map

#### HashMap 和 Hashtable 的区别

- **线程是否安全：** `HashMap` 是非线程安全的，`Hashtable` 是线程安全的,因为 `Hashtable` 内部的方法基本都经过`synchronized` 修饰。（如果要保证线程安全的话就使用 `ConcurrentHashMap`）；
- **效率：** 因为线程安全的问题，`HashMap` 要比 `Hashtable` 效率高一点。另外，`Hashtable` 基本被淘汰，不要在代码中使用它；
- **对 Null key 和 Null value 的支持：** `HashMap` 可以存储 null 的 key 和 value，但 null 作为键只能有一个，null 作为值可以有多个；Hashtable 不允许有 null 键和 null 值，否则会抛出 `NullPointerException`。
- **初始容量大小和每次扩充容量大小的不同：** 
  - *创建时如果不指定容量初始值*
    - `Hashtable` 默认的初始大小为 11，之后每次扩充，容量变为原来的 2n+1。
    - `HashMap` 默认的初始化大小为 16。之后每次扩充，容量变为原来的 2 倍。
  - *创建时如果给定了容量初始值*
    - `Hashtable` 会直接使用给定的大小。
    - `HashMap` 会将其扩充为 2 的幂次方大小（`HashMap` 中的`tableSizeFor()`方法保证）。也就是说 `HashMap` 总是使用 2 的幂作为哈希表的大小。
- **底层数据结构：** JDK1.8 以后的 `HashMap` 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）时，将链表转化为红黑树（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树），以减少搜索时间。`Hashtable` 没有这样的机制。

**`HashMap` 中带有初始容量的构造函数：**

```java
public HashMap(int initialCapacity, float loadFactor) {
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                           initialCapacity);
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " +
                                           loadFactor);
    this.loadFactor = loadFactor;
    this.threshold = tableSizeFor(initialCapacity);
}
 public HashMap(int initialCapacity) {
    this(initialCapacity, DEFAULT_LOAD_FACTOR);
}
```

```java
/**
 * 下面这个方法保证了 HashMap 总是使用 2 的幂作为哈希表的大小。
 */
static final int tableSizeFor(int cap) {
    int n = cap - 1;
    n |= n >>> 1;
    n |= n >>> 2;
    n |= n >>> 4;
    n |= n >>> 8;
    n |= n >>> 16;
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```



#### HashMap 和 HashSet 区别

如果你看过 `HashSet` 源码的话就应该知道：`HashSet` 底层就是基于 `HashMap` 实现的。（`HashSet` 的源码非常非常少，因为除了 `clone()`、`writeObject()`、`readObject()`是 `HashSet` 自己不得不实现之外，其他方法都是直接调用 `HashMap` 中的方法。

|               `HashMap`                |                          `HashSet`                           |
| :------------------------------------: | :----------------------------------------------------------: |
|           实现了 `Map` 接口            |                       实现 `Set` 接口                        |
|               存储键值对               |                          仅存储对象                          |
|     调用 `put()`向 map 中添加元素      |             调用 `add()`方法向 `Set` 中添加元素              |
| `HashMap` 使用键（Key）计算 `hashcode` | `HashSet` 使用成员对象来计算 `hashcode` 值，对于两个对象来说 `hashcode` 可能相同，所以`equals()`方法用来判断对象的相等性 |



#### HashMap 和 TreeMap 区别

`TreeMap` 和`HashMap` 都继承自`AbstractMap` ，但是需要注意的是`TreeMap`它还实现了`NavigableMap`接口和`SortedMap` 接口。

![treemap_hierarchy](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307160949343.png)



实现 `NavigableMap` 接口让 `TreeMap` 有了对集合内元素的搜索的能力。

实现`SortedMap`接口让 `TreeMap` 有了对集合中的元素根据键排序的能力。默认是按 key 的升序排序，不过我们也可以指定排序的比较器。示例代码如下：

```java
@Data
public class Person {
    private Integer age;

    public static void main(String[] args) {
        TreeMap<Person, String> treeMap = new TreeMap<>(new Comparator<Person>() {
            @Override
            public int compare(Person person1, Person person2) {
                int num = person1.getAge() - person2.getAge();
                return Integer.compare(num, 0);
            }
        });
        treeMap.put(new Person(3), "person1");
        treeMap.put(new Person(18), "person2");
        treeMap.put(new Person(35), "person3");
        treeMap.put(new Person(16), "person4");
        treeMap.entrySet().stream().forEach(personStringEntry -> {
            System.out.println(personStringEntry.getValue());
        });
    }
}
```

```text
person1
person4
person2
person3
```

可以看出，`TreeMap` 中的元素已经是按照 `Person` 的 age 字段的升序来排列了。

通过传入匿名内部类的方式实现排序，可以将代码替换成 Lambda 表达式实现的方式：

```java
TreeMap<Person, String> treeMap = new TreeMap<>((person1, person2) -> {
  int num = person1.getAge() - person2.getAge();
  return Integer.compare(num, 0);
});
```

综上，相比于`HashMap`来说 `TreeMap` 主要多了对集合中的元素*根据键排序*的能力以及对集合内元素的*搜索*的能力。



#### HashSet 如何检查重复?

> 当你把对象加入`HashSet`时，`HashSet` 会先计算对象的`hashcode`值来判断对象加入的位置，同时也会与其他加入的对象的 `hashcode` 值作比较，如果没有相符的 `hashcode`，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 `hashcode` 值的对象，这时会调用`equals()`方法来检查 `hashcode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让加入操作成功。

在 JDK1.8 中，`HashSet`的`add()`方法只是简单的调用了`HashMap`的`put()`方法，并且判断了一下返回值以确保是否有重复元素。直接看一下`HashSet`中的源码：

```java
// 返回值：当 set 中没有包含 add 的元素时返回真
public boolean add(E e) {  return map.put(e, PRESENT)==null;}
```

而在`HashMap`的`putVal()`方法中也能看到如下说明：

```java
// 返回值：如果插入位置没有元素返回null，否则返回上一个元素
final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {...}
```

也就是说，在 JDK1.8 中，实际上无论`HashSet`中是否已经存在了某元素，`HashSet`都会直接插入，只是会在`add()`方法的返回值处告诉我们插入前是否存在相同元素。



#### HashMap 的底层实现

JDK1.8 之前

JDK1.8 之前 `HashMap` 底层是 **数组和链表** 结合在一起使用也就是 **链表散列**。HashMap 通过 key 的 `hashcode` 经过扰动函数处理过后得到 hash 值，然后通过 `(n - 1) & hash` 判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。

所谓扰动函数指的就是 HashMap 的 `hash` 方法。使用 `hash` 方法也就是扰动函数是为了防止一些实现比较差的 `hashCode()` 方法 换句话说使用扰动函数之后可以减少碰撞。

**JDK 1.8 HashMap 的 hash 方法源码:**

JDK 1.8 的 hash 方法 相比于 JDK 1.7 hash 方法更加简化，但是原理不变。

```java
static final int hash(Object key) {
   int h;
   // key.hashCode()：返回散列值也就是hashcode
   // ^：按位异或
   // >>>:无符号右移，忽略符号位，空位都以0补齐
   return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

对比一下 JDK1.7 的 HashMap 的 hash 方法源码.

```java
static int hash(int h) {
    h ^= (h >>> 20) ^ (h >>> 12);
    return h ^ (h >>> 7) ^ (h >>> 4);
}
```

相比于 JDK1.8 的 hash 方法 ，JDK 1.7 的 hash 方法的性能会稍差一点点，因为毕竟扰动了 4 次。

所谓 *拉链法* 就是：将链表和数组相结合。也就是说创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。

![jdk1.7_hashmap](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307161025915.png)



JDK1.8 之后

相比于之前的版本， JDK1.8 之后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树），将链表转化为红黑树，以减少搜索时间。

![jdk1.8_hashmap](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307161025321.png)

> TreeMap、TreeSet 以及 JDK1.8 之后的 HashMap 底层都用到了红黑树。红黑树就是为了解决二叉查找树的缺陷，因为二叉查找树在某些情况下会退化成一个线性结构。

`HashMap` 链表到红黑树的转换。

**1、 `putVal` 方法中执行链表转红黑树的判断逻辑。**

链表的长度大于 8 的时候，就执行 `treeifyBin` （转换红黑树）的逻辑。

```java
// 遍历链表
for (int binCount = 0; ; ++binCount) {
    // 遍历到链表最后一个节点
    if ((e = p.next) == null) {
        p.next = newNode(hash, key, value, null);
        // 如果链表元素个数大于等于TREEIFY_THRESHOLD（8）
        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
            // 红黑树转换（并不会直接转换成红黑树）
            treeifyBin(tab, hash);
        break;
    }
    if (e.hash == hash &&
        ((k = e.key) == key || (key != null && key.equals(k))))
        break;
    p = e;
}
```

**2、`treeifyBin` 方法中判断是否真的转换为红黑树。**

```java
final void treeifyBin(Node<K,V>[] tab, int hash) {
    int n, index; Node<K,V> e;
    // 判断当前数组的长度是否小于 64
    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
        // 如果当前数组的长度小于 64，那么会选择先进行数组扩容
        resize();
    else if ((e = tab[index = (n - 1) & hash]) != null) {
        // 否则才将列表转换为红黑树

        TreeNode<K,V> hd = null, tl = null;
        do {
            TreeNode<K,V> p = replacementTreeNode(e, null);
            if (tl == null)
                hd = p;
            else {
                p.prev = tl;
                tl.next = p;
            }
            tl = p;
        } while ((e = e.next) != null);
        if ((tab[index] = hd) != null)
            hd.treeify(tab);
    }
}
```

将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树。



#### HashMap 的长度为什么是 2 的幂次方

为了能让 HashMap 存取高效，尽量较少碰撞，也就是要尽量把数据分配均匀。Hash 值的范围值-2147483648 到 2147483647，前后加起来大概 40 亿的映射空间，只要哈希函数映射得比较均匀松散，一般应用是很难出现碰撞的。但问题是一个 40 亿长度的数组，内存是放不下的。所以这个散列值是不能直接拿来用的。用之前还要先做对数组的长度取模运算，得到的余数才能用来要存放的位置也就是对应的数组下标。这个数组下标的计算方法是“ `(n - 1) & hash`”。（n 代表数组长度）。这也就解释了 HashMap 的长度为什么是 2 的幂次方。

**这个算法应该如何设计呢？**

“取余(%)操作中如果除数是 2 的幂次则等价于与其除数减一的与(&)操作（也就是说 *hash%length==hash&(length-1)*的前提是 length 是 2 的 n 次方；）。”** 并且采用二进制位操作 &，相对于%能够提高运算效率，这就解释了 HashMap 的长度为什么是 2 的幂次方。



#### HashMap 多线程操作导致死循环问题

JDK1.7 及之前版本的 `HashMap` 在多线程环境下扩容操作可能存在死循环问题，这是由于当一个桶位中有多个元素需要进行扩容时，多个线程同时对链表进行操作，头插法可能会导致链表中的节点指向错误的位置，从而形成一个环形链表，进而使得查询元素的操作陷入死循环无法结束。

为了解决这个问题，JDK1.8 版本的 HashMap 采用了尾插法而不是头插法来避免链表倒置，使得插入的节点永远都是放在链表的末尾，避免了链表中的环形结构。但是还是不建议在多线程下使用 `HashMap`，因为多线程下使用 `HashMap` 还是会存在数据覆盖的问题。并发环境下，推荐使用 `ConcurrentHashMap` 。



#### HashMap 为什么线程不安全？

JDK1.7 及之前版本，在多线程环境下，`HashMap` 扩容时会造成死循环和数据丢失的问题。

数据丢失这个在 JDK1.7 和 JDK 1.8 中都存在，这里以 JDK 1.8 为例进行介绍。

JDK 1.8 后，在 `HashMap` 中，多个键值对可能会被分配到同一个桶（bucket），并以链表或红黑树的形式存储。多个线程对 `HashMap` 的 `put` 操作会导致线程不安全，具体来说会有数据覆盖的风险。

举个例子：

- 两个线程 1,2 同时进行 put 操作，并且发生了哈希冲突（hash 函数计算出的插入下标是相同的）。
- 不同的线程可能在不同的时间片获得 CPU 执行的机会，当前线程 1 执行完哈希冲突判断后，由于时间片耗尽挂起。线程 2 先完成了插入操作。
- 随后，线程 1 获得时间片，由于之前已经进行过 hash 碰撞的判断，所有此时会直接进行插入，这就导致线程 2 插入的数据被线程 1 覆盖了。

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    // ...
    // 判断是否出现 hash 碰撞
    // (n - 1) & hash 确定元素存放在哪个桶中，桶为空，新生成结点放入桶中(此时，这个结点是放在数组中)
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    // 桶中已经存在元素（处理hash冲突）
    else {
    // ...
}
```

还有一种情况是这两个线程同时 `put` 操作导致 `size` 的值不正确，进而导致数据覆盖的问题：

1. 线程 1 执行 `if(++size > threshold)` 判断时，假设获得 `size` 的值为 10，由于时间片耗尽挂起。
2. 线程 2 也执行 `if(++size > threshold)` 判断，获得 `size` 的值也为 10，并将元素插入到该桶位中，并将 `size` 的值更新为 11。
3. 随后，线程 1 获得时间片，它也将元素放入桶位中，并将 size 的值更新为 11。
4. 线程 1、2 都执行了一次 `put` 操作，但是 `size` 的值只增加了 1，也就导致实际上只有一个元素被添加到了 `HashMap` 中。

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    // ...
    // 实际大小大于阈值则扩容
    if (++size > threshold)
        resize();
    // 插入后回调
    afterNodeInsertion(evict);
    return null;
}
```



#### HashMap 常见的遍历方式?

HashMap **遍历从大的方向来说，可分为以下 4 类**：

1. 迭代器（Iterator）方式遍历；
2. For Each 方式遍历；
3. Lambda 表达式遍历（JDK 1.8+）;
4. Streams API 遍历（JDK 1.8+）。

但每种类型下又有不同的实现方式，因此具体的遍历方式又可以分为以下 7 种：

1. 使用迭代器（Iterator）EntrySet 的方式进行遍历；
2. 使用迭代器（Iterator）KeySet 的方式进行遍历；
3. 使用 For Each EntrySet 的方式进行遍历；
4. 使用 For Each KeySet 的方式进行遍历；
5. 使用 Lambda 表达式的方式进行遍历；
6. 使用 Streams API 单线程的方式进行遍历；
7. 使用 Streams API 多线程的方式进行遍历。



##### Iterator EntrySet

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        Iterator<Map.Entry<Integer, String>> iterator = map.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<Integer, String> entry = iterator.next();
            System.out.print(entry.getKey());
            System.out.println("\t" + entry.getValue());
        }
    }
}
```

##### Iterator KeySet

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        Iterator<Integer> iterator = map.keySet().iterator();
        while (iterator.hasNext()) {
            Integer key = iterator.next();
            System.out.print(key);
            System.out.println("\t" + map.get(key));
        }
    }
}
```

##### ForEach EntrySet

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        for (Map.Entry<Integer, String> entry : map.entrySet()) {
            System.out.print(entry.getKey());
            System.out.println("\t" + entry.getValue());
        }
    }
}
```

##### ForEach KeySet

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        for (Integer key : map.keySet()) {
            System.out.print(key);
            System.out.println("\t" + map.get(key));
        }
    }
}
```

##### Lambda

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        map.forEach((key, value) -> {
            System.out.print(key);
            System.out.println("\t"+ value);
        });
    }
}
```

##### Streams API 单线程

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        map.entrySet().stream().forEach((entry) -> {
            System.out.print(entry.getKey());
            System.out.println("\t" + entry.getValue());
        });
    }
}
```

##### Streams API 多线程

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        map.entrySet().parallelStream().forEach((entry) -> {
            System.out.print(entry.getKey());
            System.out.println("\t" + entry.getValue());
        });
    }
}
```

> 使用 Oracle 官方提供的性能测试工具 JMH（Java Microbenchmark Harness，JAVA 微基准测试套件）来测试一下这 7 种循环的性能。结论是`entrySet` 的性能比 `keySet` 的性能高出了一倍之多，因此我们应该尽量使用 `entrySet` 来实现 Map 集合的遍历。
>
> ------
>
> 不能在遍历中使用集合 `map.remove()` 来删除数据，这是非安全的操作方式，但我们可以使用迭代器的 `iterator.remove()` 的方法来删除数据，这是安全的删除集合的方式。同样的我们也可以使用 Lambda 中的 `removeIf` 来提前删除数据，或者是使用 Stream 中的 `filter` 过滤掉要删除的数据进行循环，这样都是安全的，当然我们也可以在 `for` 循环前删除数据在遍历也是线程安全的。

> **存在阻塞时 parallelStream 性能最高, 非阻塞时 parallelStream 性能最低** 。

当遍历不存在阻塞时, parallelStream 的性能是最低的：

```text
Benchmark               Mode  Cnt     Score      Error  Units
Test.entrySet           avgt    5   288.651 ±   10.536  ns/op
Test.keySet             avgt    5   584.594 ±   21.431  ns/op
Test.lambda             avgt    5   221.791 ±   10.198  ns/op
Test.parallelStream     avgt    5  6919.163 ± 1116.139  ns/op
```

加入阻塞代码`Thread.sleep(10)`后, parallelStream 的性能才是最高的:

```text
Benchmark               Mode  Cnt           Score          Error  Units
Test.entrySet           avgt    5  1554828440.000 ± 23657748.653  ns/op
Test.keySet             avgt    5  1550612500.000 ±  6474562.858  ns/op
Test.lambda             avgt    5  1551065180.000 ± 19164407.426  ns/op
Test.parallelStream     avgt    5   186345456.667 ±  3210435.590  ns/op
```



#### ConcurrentHashMap 和 Hashtable 的区别

`ConcurrentHashMap` 和 `Hashtable` 的区别主要体现在实现线程安全的方式上不同。

- **底层数据结构：** JDK1.7 的 `ConcurrentHashMap` 底层采用 **分段的数组+链表** 实现，JDK1.8 采用的数据结构跟 `HashMap1.8` 的结构一样，数组+链表/红黑二叉树。`Hashtable` 和 JDK1.8 之前的 `HashMap` 的底层数据结构类似都是采用 **数组+链表** 的形式，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的；
- 实现线程安全的方式（重要）：
  - 在 JDK1.7 的时候，`ConcurrentHashMap` 对整个桶数组进行了分割分段(`Segment`，分段锁)，每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。
  - 到了 JDK1.8 的时候，`ConcurrentHashMap` 已经摒弃了 `Segment` 的概念，而是直接用 `Node` 数组+链表+红黑树的数据结构来实现，并发控制使用 `synchronized` 和 CAS 来操作。（JDK1.6 以后 `synchronized` 锁做了很多优化） 整个看起来就像是优化过且线程安全的 `HashMap`，虽然在 JDK1.8 中还能看到 `Segment` 的数据结构，但是已经简化了属性，只是为了兼容旧版本；
  - **`Hashtable`(同一把锁)** :使用 `synchronized` 来保证线程安全，效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用 put 添加元素，另一个线程不能使用 put 添加元素，也不能使用 get，竞争会越来越激烈效率越低。

**Hashtable** :

![jdk1.7_hashmap (1)](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307161105910.png)

**JDK1.7 的 ConcurrentHashMap**：

![java7_concurrenthashmap](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307161105216.png)



`ConcurrentHashMap` 是由 `Segment` 数组结构和 `HashEntry` 数组结构组成。

`Segment` 数组中的每个元素包含一个 `HashEntry` 数组，每个 `HashEntry` 数组属于链表结构。

**JDK1.8 的 ConcurrentHashMap**：

![java8_concurrenthashmap](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307161106626.png)



JDK1.8 的 `ConcurrentHashMap` 不再是 **Segment 数组 + HashEntry 数组 + 链表**，而是 **Node 数组 + 链表 / 红黑树**。不过，Node 只能用于链表的情况，红黑树的情况需要使用 **`TreeNode`**。当冲突链表达到一定长度时，链表会转换成红黑树。

`TreeNode`是存储红黑树节点，被`TreeBin`包装。`TreeBin`通过`root`属性维护红黑树的根结点，因为红黑树在旋转的时候，根结点可能会被它原来的子节点替换掉，在这个时间点，如果有其他线程要写这棵红黑树就会发生线程不安全问题，所以在 `ConcurrentHashMap` 中`TreeBin`通过`waiter`属性维护当前使用这棵红黑树的线程，来防止其他线程的进入。

```java
static final class TreeBin<K,V> extends Node<K,V> {
        TreeNode<K,V> root;
        volatile TreeNode<K,V> first;
        volatile Thread waiter;
        volatile int lockState;
        // values for lockState
        static final int WRITER = 1; // set while holding write lock
        static final int WAITER = 2; // set when waiting for write lock
        static final int READER = 4; // increment value for setting read lock
...
}
```



#### ConcurrentHashMap 线程安全的实现方式&底层实现

##### JDK1.8 之前

![java7_concurrenthashmap](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307161110747.png)

首先将数据分为一段一段（这个“段”就是 `Segment`）的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。

**`ConcurrentHashMap` 是由 `Segment` 数组结构和 `HashEntry` 数组结构组成**。

`Segment` 继承了 `ReentrantLock`,所以 `Segment` 是一种可重入锁，扮演锁的角色。`HashEntry` 用于存储键值对数据。

```java
static class Segment<K,V> extends ReentrantLock implements Serializable {
}
```

一个 `ConcurrentHashMap` 里包含一个 `Segment` 数组，`Segment` 的个数一旦**初始化就不能改变**。 `Segment` 数组的大小默认是 16，也就是说默认可以同时支持 16 个线程并发写。

`Segment` 的结构和 `HashMap` 类似，是一种数组和链表结构，一个 `Segment` 包含一个 `HashEntry` 数组，每个 `HashEntry` 是一个链表结构的元素，每个 `Segment` 守护着一个 `HashEntry` 数组里的元素，当对 `HashEntry` 数组的数据进行修改时，必须首先获得对应的 `Segment` 的锁。也就是说，对同一 `Segment` 的并发写入会被阻塞，不同 `Segment` 的写入是可以并发执行的。



##### JDK1.8 之后

![java8_concurrenthashmap](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307161111335.png)



Java 8 几乎完全重写了 `ConcurrentHashMap`，代码量从原来 Java 7 中的 1000 多行，变成了现在的 6000 多行。

`ConcurrentHashMap` 取消了 `Segment` 分段锁，采用 `Node + CAS + synchronized` 来保证并发安全。数据结构跟 `HashMap` 1.8 的结构类似，数组+链表/红黑二叉树。Java 8 在链表长度超过一定阈值（8）时将链表（寻址时间复杂度为 O(N)）转换为红黑树（寻址时间复杂度为 O(log(N))）。

Java 8 中，锁粒度更细，`synchronized` 只锁定当前链表或红黑二叉树的首节点，这样只要 hash 不冲突，就不会产生并发，就不会影响其他 Node 的读写，效率大幅提升。



##### JDK 1.7 和 JDK 1.8 的 ConcurrentHashMap 实现有什么不同？

- **线程安全实现方式**：JDK 1.7 采用 `Segment` 分段锁来保证安全， `Segment` 是继承自 `ReentrantLock`。JDK1.8 放弃了 `Segment` 分段锁的设计，采用 `Node + CAS + synchronized` 保证线程安全，锁粒度更细，`synchronized` 只锁定当前链表或红黑二叉树的首节点。
- **Hash 碰撞解决方法** : JDK 1.7 采用拉链法，JDK1.8 采用拉链法结合红黑树（链表长度超过一定阈值时，将链表转换为红黑树）。
- **并发度**：JDK 1.7 最大并发度是 Segment 的个数，默认是 16。JDK 1.8 最大并发度是 Node 数组的大小，并发度更大。



### Collections

**`Collections` 工具类常用方法**:

- 排序
- 查找,替换操作
- 同步控制(不推荐，需要线程安全的集合类型时请考虑使用 JUC 包下的并发集合)

#### 排序操作

```java
void reverse(List list)//反转
void shuffle(List list)//随机排序
void sort(List list)//按自然排序的升序排序
void sort(List list, Comparator c)//定制排序，由Comparator控制排序逻辑
void swap(List list, int i , int j)//交换两个索引位置的元素
void rotate(List list, int distance)//旋转。当distance为正数时，将list后distance个元素整体移到前面。当distance为负数时，将 list的前distance个元素整体移到后面
```

#### 查找替换操作

```java
int binarySearch(List list, Object key)//对List进行二分查找，返回索引，注意List必须是有序的
int max(Collection coll)//根据元素的自然顺序，返回最大的元素。 类比int min(Collection coll)
int max(Collection coll, Comparator c)//根据定制排序，返回最大元素，排序规则由Comparatator类控制。类比int min(Collection coll, Comparator c)
void fill(List list, Object obj)//用指定的元素代替指定list中的所有元素
int frequency(Collection c, Object o)//统计元素出现次数
int indexOfSubList(List list, List target)//统计target在list中第一次出现的索引，找不到则返回-1，类比int lastIndexOfSubList(List source, list target)
boolean replaceAll(List list, Object oldVal, Object newVal)//用新元素替换旧元素
```

#### 同步控制

`Collections` 提供了多个`synchronizedXxx()`方法·，该方法可以将指定集合包装成线程同步的集合，从而解决多线程并发访问集合时的线程安全问题。

我们知道 `HashSet`，`TreeSet`，`ArrayList`,`LinkedList`,`HashMap`,`TreeMap` 都是线程不安全的。`Collections` 提供了多个静态方法可以把他们包装成线程同步的集合。

**最好不要用下面这些方法，效率非常低，需要线程安全的集合类型时请考虑使用 JUC 包下的并发集合。**

```java
synchronizedCollection(Collection<T>  c) //返回指定 collection 支持的同步（线程安全的）collection。
synchronizedList(List<T> list)//返回指定列表支持的同步（线程安全的）List。
synchronizedMap(Map<K,V> m) //返回由指定映射支持的同步（线程安全的）Map。
synchronizedSet(Set<T> s) //返回指定 set 支持的同步（线程安全的）set。
```

### 注意事项

#### 集合判空

《阿里巴巴 Java 开发手册》的描述如下：

> **判断所有集合内部的元素是否为空，使用 `isEmpty()` 方法，而不是 `size()==0` 的方式。**

这是因为 `isEmpty()` 方法的可读性更好，并且时间复杂度为 O(1)。

绝大部分使用集合的 `size()` 方法的时间复杂度也是 O(1)，不过，也有很多复杂度不是 O(1) 的，比如 `java.util.concurrent` 包下的某些集合（`ConcurrentLinkedQueue`、`ConcurrentHashMap`）。

下面是 `ConcurrentHashMap` 的 `size()` 方法和 `isEmpty()` 方法的源码。

```java
public int size() {
    long n = sumCount();
    return ((n < 0L) ? 0 :
            (n > (long)Integer.MAX_VALUE) ? Integer.MAX_VALUE :
            (int)n);
}
final long sumCount() {
    CounterCell[] as = counterCells; CounterCell a;
    long sum = baseCount;
    if (as != null) {
        for (int i = 0; i < as.length; ++i) {
            if ((a = as[i]) != null)
                sum += a.value;
        }
    }
    return sum;
}
public boolean isEmpty() {
    return sumCount() <= 0L; // ignore transient negative values
}
```



#### 集合转Map

《阿里巴巴 Java 开发手册》的描述如下：

> **在使用 `java.util.stream.Collectors` 类的 `toMap()` 方法转为 `Map` 集合时，一定要注意当 value 为 null 时会抛 NPE 异常。**

```java
class Person {
    private String name;
    private String phoneNumber;
     // getters and setters
}

List<Person> bookList = new ArrayList<>();
bookList.add(new Person("jack","18163138123"));
bookList.add(new Person("martin",null));
// 空指针异常
bookList.stream().collect(Collectors.toMap(Person::getName, Person::getPhoneNumber));
```

`java.util.stream.Collectors` 类的 `toMap()` 方法 ，其内部调用了 `Map` 接口的 `merge()` 方法。

```java
public static <T, K, U, M extends Map<K, U>>
Collector<T, ?, M> toMap(Function<? super T, ? extends K> keyMapper,
                            Function<? super T, ? extends U> valueMapper,
                            BinaryOperator<U> mergeFunction,
                            Supplier<M> mapSupplier) {
    BiConsumer<M, T> accumulator
            = (map, element) -> map.merge(keyMapper.apply(element),
                                          valueMapper.apply(element), mergeFunction);
    return new CollectorImpl<>(mapSupplier, accumulator, mapMerger(mergeFunction), CH_ID);
}
```

`Map` 接口的 `merge()` 方法如下，这个方法是接口中的默认实现。

```java
default V merge(K key, V value,
        BiFunction<? super V, ? super V, ? extends V> remappingFunction) {
    Objects.requireNonNull(remappingFunction);
    Objects.requireNonNull(value);
    V oldValue = get(key);
    V newValue = (oldValue == null) ? value :
               remappingFunction.apply(oldValue, value);
    if(newValue == null) {
        remove(key);
    } else {
        put(key, newValue);
    }
    return newValue;
}
```

`merge()` 方法会先调用 `Objects.requireNonNull()` 方法判断 value 是否为空。

```java
public static <T> T requireNonNull(T obj) {
    if (obj == null)
        throw new NullPointerException();
    return obj;
}
```



#### 集合遍历

《阿里巴巴 Java 开发手册》的描述如下：

> **不要在 foreach 循环里进行元素的 `remove/add` 操作。remove 元素请使用 `Iterator` 方式，如果并发操作，需要对 `Iterator` 对象加锁。**

通过反编译发现 foreach 语法底层其实还是依赖 `Iterator` 。不过， `remove/add` 操作直接调用的是集合自己的方法，而不是 `Iterator` 的 `remove/add`方法

这就导致 `Iterator` 莫名其妙地发现自己有元素被 `remove/add` ，然后，它就会抛出一个 `ConcurrentModificationException` 来提示用户发生了并发修改异常。这就是单线程状态下产生的 **fail-fast 机制**。

> **fail-fast 机制**：多个线程对 fail-fast 集合进行修改的时候，可能会抛出`ConcurrentModificationException`。 即使是单线程下也有可能会出现这种情况，上面已经提到过。

Java8 开始，可以使用 `Collection#removeIf()`方法删除满足特定条件的元素,如

```java
List<Integer> list = new ArrayList<>();
for (int i = 1; i <= 10; ++i) {
    list.add(i);
}
list.removeIf(filter -> filter % 2 == 0); /* 删除list中的所有偶数 */
System.out.println(list); /* [1, 3, 5, 7, 9] */
```

除了上面介绍的直接使用 `Iterator` 进行遍历操作之外，还可以：

- 使用普通的 for 循环
- 使用 fail-safe 的集合类。`java.util`包下面的所有的集合类都是 fail-fast 的，而`java.util.concurrent`包下面的所有的类都是 fail-safe 的。



#### 集合去重

《阿里巴巴 Java 开发手册》的描述如下：

> **可以利用 `Set` 元素唯一的特性，可以快速对一个集合进行去重操作，避免使用 `List` 的 `contains()` 进行遍历去重或者判断包含操作。**

这里我们以 `HashSet` 和 `ArrayList` 为例说明。

```java
// Set 去重代码示例
public static <T> Set<T> removeDuplicateBySet(List<T> data) {

    if (CollectionUtils.isEmpty(data)) {
        return new HashSet<>();
    }
    return new HashSet<>(data);
}

// List 去重代码示例
public static <T> List<T> removeDuplicateByList(List<T> data) {

    if (CollectionUtils.isEmpty(data)) {
        return new ArrayList<>();

    }
    List<T> result = new ArrayList<>(data.size());
    for (T current : data) {
        if (!result.contains(current)) {
            result.add(current);
        }
    }
    return result;
}
```

两者的核心差别在于 `contains()` 方法的实现。

`HashSet` 的 `contains()` 方法底部依赖的 `HashMap` 的 `containsKey()` 方法，时间复杂度接近于 O（1）（没有出现哈希冲突的时候为 O（1））。

```java
private transient HashMap<E,Object> map;
public boolean contains(Object o) {
    return map.containsKey(o);
}
```

我们有 N 个元素插入进 Set 中，那时间复杂度就接近是 O (n)。

`ArrayList` 的 `contains()` 方法是通过遍历所有元素的方法来做的，时间复杂度接近是 O(n)。

```java
public boolean contains(Object o) {
    return indexOf(o) >= 0;
}
public int indexOf(Object o) {
    if (o == null) {
        for (int i = 0; i < size; i++)
            if (elementData[i]==null)
                return i;
    } else {
        for (int i = 0; i < size; i++)
            if (o.equals(elementData[i]))
                return i;
    }
    return -1;
}
```

我们的 `List` 有 N 个元素，那时间复杂度就接近是 O (n^2)。



#### 集合转数组

《阿里巴巴 Java 开发手册》的描述如下：

> **使用集合转数组的方法，必须使用集合的 `toArray(T[] array)`，传入的是类型完全一致、长度为 0 的空数组。**

`toArray(T[] array)` 方法的参数是一个泛型数组，如果 `toArray` 方法中没有传递任何参数的话返回的是 `Object`类 型数组。

```java
String [] s= new String[]{
    "dog", "lazy", "a", "over", "jumps", "fox", "brown", "quick", "A"
};
List<String> list = Arrays.asList(s);
Collections.reverse(list);
//没有指定类型的话会报错
s=list.toArray(new String[0]);
```

由于 JVM 优化，`new String[0]`作为`Collection.toArray()`方法的参数现在使用更好，`new String[0]`就是起一个模板的作用，指定了返回数组的类型，0 是为了节省空间，因为它只是为了说明返回的类型。



#### 数组转集合

《阿里巴巴 Java 开发手册》的描述如下：

> **使用工具类 `Arrays.asList()` 把数组转换成集合时，不能使用其修改集合相关的方法， 它的 `add/remove/clear` 方法会抛出 `UnsupportedOperationException` 异常。**

`Arrays.asList()`在平时开发中还是比较常见的，我们可以使用它将一个数组转换为一个 `List` 集合。

```java
String[] myArray = {"Apple", "Banana", "Orange"};
List<String> myList = Arrays.asList(myArray);
//上面两个语句等价于下面一条语句
List<String> myList = Arrays.asList("Apple","Banana", "Orange");
```

JDK 源码对于这个方法的说明：

```java
/**
  *返回由指定数组支持的固定大小的列表。此方法作为基于数组和基于集合的API之间的桥梁，
  * 与 Collection.toArray()结合使用。返回的List是可序列化并实现RandomAccess接口。
  */
public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}
```

使用注意事项：

**1、`Arrays.asList()`是泛型方法，传递的数组必须是对象数组，而不是基本类型。**

```java
int[] myArray = {1, 2, 3};
List myList = Arrays.asList(myArray);
System.out.println(myList.size());//1
System.out.println(myList.get(0));//数组地址值
System.out.println(myList.get(1));//报错：ArrayIndexOutOfBoundsException
int[] array = (int[]) myList.get(0);
System.out.println(array[0]);//1
```

当传入一个原生数据类型数组时，`Arrays.asList()` 的真正得到的参数就不是数组中的元素，而是数组对象本身！此时 `List` 的唯一元素就是这个数组，这也就解释了上面的代码。

我们使用包装类型数组就可以解决这个问题。

```java
Integer[] myArray = {1, 2, 3};
```

**2、使用集合的修改方法: `add()`、`remove()`、`clear()`会抛出异常。**

```java
List myList = Arrays.asList(1, 2, 3);
myList.add(4);//运行时报错：UnsupportedOperationException
myList.remove(1);//运行时报错：UnsupportedOperationException
myList.clear();//运行时报错：UnsupportedOperationException
```

`Arrays.asList()` 方法返回的并不是 `java.util.ArrayList` ，而是 `java.util.Arrays` 的一个内部类,这个内部类并没有实现集合的修改方法或者说并没有重写这些方法。

```java
List myList = Arrays.asList(1, 2, 3);
System.out.println(myList.getClass());//class java.util.Arrays$ArrayList
```

`java.util.Arrays$ArrayList` 的简易源码

```java
  private static class ArrayList<E> extends AbstractList<E>.
        implements RandomAccess, java.io.Serializable{
        ...
            
        @Override
        public E get(int index) {...}

        @Override
        public E set(int index, E element) {...}

        @Override
        public int indexOf(Object o) {...}

        @Override
        public boolean contains(Object o) {...}

        @Override
        public void forEach(Consumer<? super E> action) {...}

        @Override
        public void replaceAll(UnaryOperator<E> operator) {...}

        @Override
        public void sort(Comparator<? super E> c) {...}
    }
```

`add/remove/clear` 

```java
public E remove(int index) {
    throw new UnsupportedOperationException();
}
public boolean add(E e) {
    add(size(), e);
    return true;
}
public void add(int index, E element) {
    throw new UnsupportedOperationException();
}

public void clear() {
    removeRange(0, size());
}
protected void removeRange(int fromIndex, int toIndex) {
    ListIterator<E> it = listIterator(fromIndex);
    for (int i=0, n=toIndex-fromIndex; i<n; i++) {
        it.next();
        it.remove();
    }
}
```

**那我们如何正确的将数组转换为 `ArrayList` ?**

1、手动实现工具类

```java
//JDK1.5+
static <T> List<T> arrayToList(final T[] array) {
  final List<T> l = new ArrayList<T>(array.length);

  for (final T s : array) {
    l.add(s);
  }
  return l;
}


Integer [] myArray = { 1, 2, 3 };
System.out.println(arrayToList(myArray).getClass());//class java.util.ArrayList
```

2、最简便的方法

```java
List list = new ArrayList<>(Arrays.asList("a", "b", "c"))
```

3、使用 Java8 的 `Stream`(推荐)

```java
Integer [] myArray = { 1, 2, 3 };
List myList = Arrays.stream(myArray).collect(Collectors.toList());
//基本类型也可以实现转换（依赖boxed的装箱操作）
int [] myArray2 = { 1, 2, 3 };
List myList = Arrays.stream(myArray2).boxed().collect(Collectors.toList());
```

4、使用 Guava

对于不可变集合，可以使用[`ImmutableList`类及其[`of()`与`copyOf()`工厂方法：（参数不能为空）

```java
List<String> il = ImmutableList.of("string", "elements");  // from varargs
List<String> il = ImmutableList.copyOf(aStringArray);      // from array
```

对于可变集合，你可以使用[`Lists`open in new window]类及其`newArrayList()`工厂方法：

```java
List<String> l1 = Lists.newArrayList(anotherListOrCollection);    // from collection
List<String> l2 = Lists.newArrayList(aStringArray);               // from array
List<String> l3 = Lists.newArrayList("or", "string", "elements"); // from varargs
```

5、使用 Apache Commons Collections

```java
List<String> list = new ArrayList<String>();
CollectionUtils.addAll(list, str);
```

6、 使用 Java9 的 `List.of()`方法

```java
Integer[] array = {1, 2, 3};
List<Integer> list = List.of(array);
```



### 源码分析

#### ArrayList

##### ArrayList 简介

`ArrayList` 的底层是数组队列，相当于动态数组。与 Java 中的数组相比，它的容量能动态增长。在添加大量元素前，应用程序可以使用`ensureCapacity`操作来增加 `ArrayList` 实例的容量。这可以减少递增式再分配的数量。

`ArrayList` 继承于 `AbstractList` ，实现了 `List`, `RandomAccess`, `Cloneable`, `java.io.Serializable` 这些接口。

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable{

  }
```

- `List` : 表明它是一个**列表**，支持*添加*、*删除*、*查找*等操作，并且可以*通过下标进行访问*。
- `RandomAccess` ：这是一个标志接口，表明实现这个接口的 `List` 集合是支持 **快速随机访问** 的。在 `ArrayList` 中，我们即可以通过元素的序号快速获取元素对象，这就是快速随机访问。
- `Cloneable` ：表明它具有**拷贝**能力，可以进行*深拷贝*或*浅拷贝*操作。
- `Serializable` : 表明它可以进行序列化操作，也就是可以将对象转换为字节流进行持久化存储或网络传输，非常方便。

![arraylist-class-diagram](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307161506840.png)



##### ArrayList 核心源码解读

```java
// Java 8
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
    private static final long serialVersionUID = 8683452581122892189L;

    /**
     * 默认初始容量大小
     */
    private static final int DEFAULT_CAPACITY = 10;

    /**
     * 空数组（用于空实例）。
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};

    //用于默认大小空实例的共享空数组实例。
    //我们把它从EMPTY_ELEMENTDATA数组中区分出来，以知道在添加第一个元素时容量需要增加多少。
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    /**
     * 保存ArrayList数据的数组 transient修饰的关键字不会被序列化与
     */
    transient Object[] elementData;

    /**
     * ArrayList 所包含的元素个数
     */
    private int size;

    /**
     * 带初始容量参数的构造函数（用户可以在创建ArrayList对象时自己指定集合的初始大小）
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            //如果传入的参数大于0，创建initialCapacity大小的数组
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            //如果传入的参数等于0，创建空数组
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            //其他情况，抛出异常
            throw new IllegalArgumentException("Illegal Capacity: " + initialCapacity);
        }
    }

    /**
     * 默认无参构造函数
     * DEFAULTCAPACITY_EMPTY_ELEMENTDATA 为0.初始化为10，也就是说初始其实是空数组 当添加第一个元素的时候数组容量才变成10
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    /**
     * 构造一个包含指定集合的元素的列表，按照它们由集合的迭代器返回的顺序。
     */
    public ArrayList(Collection<? extends E> c) {
        //将指定集合转换为数组
        elementData = c.toArray();
        //如果elementData数组的长度不为0
        if ((size = elementData.length) != 0) {
            // 如果elementData不是Object类型数据（c.toArray可能返回的不是Object类型的数组所以加上下面的语句用于判断）
            if (elementData.getClass() != Object[].class)
                //将原来不是Object类型的elementData数组的内容，赋值给新的Object类型的elementData数组
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // 其他情况，用空数组代替
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }

    /**
     * 修改这个ArrayList实例的容量是列表的当前大小。 应用程序可以使用此操作来最小化ArrayList实例的存储。
     */
    public void trimToSize() {
        modCount++;
        if (size < elementData.length) {
            elementData = (size == 0) ? EMPTY_ELEMENTDATA : Arrays.copyOf(elementData, size);
        }
    }
//下面是ArrayList的扩容机制
//ArrayList的扩容机制提高了性能，如果每次只扩充一个，
//那么频繁的插入会导致频繁的拷贝，降低性能，而ArrayList的扩容机制避免了这种情况。

    /**
     * 如有必要，增加此ArrayList实例的容量，以确保它至少能容纳元素的数量
     *
     * @param minCapacity 所需的最小容量
     */
    public void ensureCapacity(int minCapacity) {
        //如果是true，minExpand的值为0，如果是false,minExpand的值为10
        int minExpand = (elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
                // any size if not default element table
                ? 0
                // larger than default for default empty table. It's already
                // supposed to be at default size.
                : DEFAULT_CAPACITY;
        //如果最小容量大于已有的最大容量
        if (minCapacity > minExpand) {
            ensureExplicitCapacity(minCapacity);
        }
    }

    //1.得到最小扩容量
    //2.通过最小容量扩容
    private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            // 获取“默认的容量”和“传入参数”两者之间的最大值
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }

        ensureExplicitCapacity(minCapacity);
    }

    //判断是否需要扩容
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            //调用grow方法进行扩容，调用此方法代表已经开始扩容了
            grow(minCapacity);
    }

    /**
     * 要分配的最大数组大小
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

    /**
     * ArrayList扩容的核心方法。
     */
    private void grow(int minCapacity) {
        // oldCapacity为旧容量，newCapacity为新容量
        int oldCapacity = elementData.length;
        //将oldCapacity 右移一位，其效果相当于oldCapacity /2，
        //我们知道位运算的速度远远快于整除运算，整句运算式的结果就是将新容量更新为旧容量的1.5倍，
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        //然后检查新容量是否大于最小需要容量，若还是小于最小需要容量，那么就把最小需要容量当作数组的新容量，
        if (newCapacity - minCapacity < 0) newCapacity = minCapacity;
        //再检查新容量是否超出了ArrayList所定义的最大容量，
        //若超出了，则调用hugeCapacity()来比较minCapacity和 MAX_ARRAY_SIZE，
        //如果minCapacity大于MAX_ARRAY_SIZE，则新容量则为Interger.MAX_VALUE，否则，新容量大小则为 MAX_ARRAY_SIZE。
        if (newCapacity - MAX_ARRAY_SIZE > 0) newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

    //比较minCapacity和 MAX_ARRAY_SIZE
    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE;
    }

    /**
     * 返回此列表中的元素数。
     */
    public int size() {
        return size;
    }

    /**
     * 如果此列表不包含元素，则返回 true 。
     */
    public boolean isEmpty() {
        //注意=和==的区别
        return size == 0;
    }

    /**
     * 如果此列表包含指定的元素，则返回true 。
     */
    public boolean contains(Object o) {
        //indexOf()方法：返回此列表中指定元素的首次出现的索引，如果此列表不包含此元素，则为-1
        return indexOf(o) >= 0;
    }

    /**
     * 返回此列表中指定元素的首次出现的索引，如果此列表不包含此元素，则为-1
     */
    public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i] == null) return i;
        } else {
            for (int i = 0; i < size; i++)
                //equals()方法比较
                if (o.equals(elementData[i])) return i;
        }
        return -1;
    }

    /**
     * 返回此列表中指定元素的最后一次出现的索引，如果此列表不包含元素，则返回-1。.
     */
    public int lastIndexOf(Object o) {
        if (o == null) {
            for (int i = size - 1; i >= 0; i--)
                if (elementData[i] == null) return i;
        } else {
            for (int i = size - 1; i >= 0; i--)
                if (o.equals(elementData[i])) return i;
        }
        return -1;
    }

    /**
     * 返回此ArrayList实例的浅拷贝。 （元素本身不被复制。）
     */
    public Object clone() {
        try {
            ArrayList<?> v = (ArrayList<?>) super.clone();
            //Arrays.copyOf功能是实现数组的复制，返回复制后的数组。参数是被复制的数组和复制的长度
            v.elementData = Arrays.copyOf(elementData, size);
            v.modCount = 0;
            return v;
        } catch (CloneNotSupportedException e) {
            // 这不应该发生，因为我们是可以克隆的
            throw new InternalError(e);
        }
    }

    /**
     * 以正确的顺序（从第一个到最后一个元素）返回一个包含此列表中所有元素的数组。
     * 返回的数组将是“安全的”，因为该列表不保留对它的引用。 （换句话说，这个方法必须分配一个新的数组）。
     * 因此，调用者可以自由地修改返回的数组。 此方法充当基于阵列和基于集合的API之间的桥梁。
     */
    public Object[] toArray() {
        return Arrays.copyOf(elementData, size);
    }

    /**
     * 以正确的顺序返回一个包含此列表中所有元素的数组（从第一个到最后一个元素）;
     * 返回的数组的运行时类型是指定数组的运行时类型。 如果列表适合指定的数组，则返回其中。
     * 否则，将为指定数组的运行时类型和此列表的大小分配一个新数组。
     * 如果列表适用于指定的数组，其余空间（即数组的列表数量多于此元素），则紧跟在集合结束后的数组中的元素设置为null 。
     * （这仅在调用者知道列表不包含任何空元素的情况下才能确定列表的长度。）
     */
    @SuppressWarnings("unchecked")
    public <T> T[] toArray(T[] a) {
        if (a.length < size)
            // 新建一个运行时类型的数组，但是ArrayList数组的内容
            return (T[]) Arrays.copyOf(elementData, size, a.getClass());
        //调用System提供的arraycopy()方法实现数组之间的复制
        System.arraycopy(elementData, 0, a, 0, size);
        if (a.length > size) a[size] = null;
        return a;
    }

    // Positional Access Operations

    @SuppressWarnings("unchecked")
    E elementData(int index) {
        return (E) elementData[index];
    }

    /**
     * 返回此列表中指定位置的元素。
     */
    public E get(int index) {
        rangeCheck(index);

        return elementData(index);
    }

    /**
     * 用指定的元素替换此列表中指定位置的元素。
     */
    public E set(int index, E element) {
        //对index进行界限检查
        rangeCheck(index);

        E oldValue = elementData(index);
        elementData[index] = element;
        //返回原来在这个位置的元素
        return oldValue;
    }

    /**
     * 将指定的元素追加到此列表的末尾。
     */
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        //这里看到ArrayList添加元素的实质就相当于为数组赋值
        elementData[size++] = e;
        return true;
    }

    /**
     * 在此列表中的指定位置插入指定的元素。
     * 先调用 rangeCheckForAdd 对index进行界限检查；然后调用 ensureCapacityInternal 方法保证capacity足够大；
     * 再将从index开始之后的所有成员后移一个位置；将element插入index位置；最后size加1。
     */
    public void add(int index, E element) {
        rangeCheckForAdd(index);

        ensureCapacityInternal(size + 1);  // Increments modCount!!
        //arraycopy()这个实现数组之间复制的方法一定要看一下，下面就用到了arraycopy()方法实现数组自己复制自己
        System.arraycopy(elementData, index, elementData, index + 1, size - index);
        elementData[index] = element;
        size++;
    }

    /**
     * 删除该列表中指定位置的元素。 将任何后续元素移动到左侧（从其索引中减去一个元素）。
     */
    public E remove(int index) {
        rangeCheck(index);

        modCount++;
        E oldValue = elementData(index);

        int numMoved = size - index - 1;
        if (numMoved > 0) System.arraycopy(elementData, index + 1, elementData, index, numMoved);
        elementData[--size] = null; // clear to let GC do its work
        //从列表中删除的元素
        return oldValue;
    }

    /**
     * 从列表中删除指定元素的第一个出现（如果存在）。 如果列表不包含该元素，则它不会更改。
     * 返回true，如果此列表包含指定的元素
     */
    public boolean remove(Object o) {
        if (o == null) {
            for (int index = 0; index < size; index++)
                if (elementData[index] == null) {
                    fastRemove(index);
                    return true;
                }
        } else {
            for (int index = 0; index < size; index++)
                if (o.equals(elementData[index])) {
                    fastRemove(index);
                    return true;
                }
        }
        return false;
    }

    /*
     * Private remove method that skips bounds checking and does not
     * return the value removed.
     */
    private void fastRemove(int index) {
        modCount++;
        int numMoved = size - index - 1;
        if (numMoved > 0) System.arraycopy(elementData, index + 1, elementData, index, numMoved);
        elementData[--size] = null; // clear to let GC do its work
    }

    /**
     * 从列表中删除所有元素。
     */
    public void clear() {
        modCount++;

        // 把数组中所有的元素的值设为null
        for (int i = 0; i < size; i++)
            elementData[i] = null;

        size = 0;
    }

    /**
     * 按指定集合的Iterator返回的顺序将指定集合中的所有元素追加到此列表的末尾。
     */
    public boolean addAll(Collection<? extends E> c) {
        Object[] a = c.toArray();
        int numNew = a.length;
        ensureCapacityInternal(size + numNew);  // Increments modCount
        System.arraycopy(a, 0, elementData, size, numNew);
        size += numNew;
        return numNew != 0;
    }

    /**
     * 将指定集合中的所有元素插入到此列表中，从指定的位置开始。
     */
    public boolean addAll(int index, Collection<? extends E> c) {
        rangeCheckForAdd(index);

        Object[] a = c.toArray();
        int numNew = a.length;
        ensureCapacityInternal(size + numNew);  // Increments modCount

        int numMoved = size - index;
        if (numMoved > 0) System.arraycopy(elementData, index, elementData, index + numNew, numMoved);

        System.arraycopy(a, 0, elementData, index, numNew);
        size += numNew;
        return numNew != 0;
    }

    /**
     * 从此列表中删除所有索引为fromIndex （含）和toIndex之间的元素。
     * 将任何后续元素移动到左侧（减少其索引）。
     */
    protected void removeRange(int fromIndex, int toIndex) {
        modCount++;
        int numMoved = size - toIndex;
        System.arraycopy(elementData, toIndex, elementData, fromIndex, numMoved);

        // clear to let GC do its work
        int newSize = size - (toIndex - fromIndex);
        for (int i = newSize; i < size; i++) {
            elementData[i] = null;
        }
        size = newSize;
    }

    /**
     * 检查给定的索引是否在范围内。
     */
    private void rangeCheck(int index) {
        if (index >= size) throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }

    /**
     * add和addAll使用的rangeCheck的一个版本
     */
    private void rangeCheckForAdd(int index) {
        if (index > size || index < 0) throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }

    /**
     * 返回IndexOutOfBoundsException细节信息
     */
    private String outOfBoundsMsg(int index) {
        return "Index: " + index + ", Size: " + size;
    }

    /**
     * 从此列表中删除指定集合中包含的所有元素。
     */
    public boolean removeAll(Collection<?> c) {
        Objects.requireNonNull(c);
        //如果此列表被修改则返回true
        return batchRemove(c, false);
    }

    /**
     * 仅保留此列表中包含在指定集合中的元素。
     * 换句话说，从此列表中删除其中不包含在指定集合中的所有元素。
     */
    public boolean retainAll(Collection<?> c) {
        Objects.requireNonNull(c);
        return batchRemove(c, true);
    }


    /**
     * 从列表中的指定位置开始，返回列表中的元素（按正确顺序）的列表迭代器。
     * 指定的索引表示初始调用将返回的第一个元素为next 。 初始调用previous将返回指定索引减1的元素。
     * 返回的列表迭代器是fail-fast 。
     */
    public ListIterator<E> listIterator(int index) {
        if (index < 0 || index > size) throw new IndexOutOfBoundsException("Index: " + index);
        return new ListItr(index);
    }

    /**
     * 返回列表中的列表迭代器（按适当的顺序）。
     * 返回的列表迭代器是fail-fast 。
     */
    public ListIterator<E> listIterator() {
        return new ListItr(0);
    }

    /**
     * 以正确的顺序返回该列表中的元素的迭代器。
     * 返回的迭代器是fail-fast 。
     */
    public Iterator<E> iterator() {
        return new Itr();
    }
}
```

##### ArrayList 扩容机制分析

构造函数

**（JDK8）ArrayList 有三种方式来初始化，构造方法源码如下：**

```java
/**
 * 默认初始容量大小
 */
private static final int DEFAULT_CAPACITY = 10;


private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

/**
 *默认构造函数，使用初始容量10构造一个空列表(无参数构造)
 */
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

/**
 * 带初始容量参数的构造函数。（用户自己指定容量）
 */
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {//初始容量大于0
        //创建initialCapacity大小的数组
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {//初始容量等于0
        //创建空数组
        this.elementData = EMPTY_ELEMENTDATA;
    } else {//初始容量小于0，抛出异常
        throw new IllegalArgumentException("Illegal Capacity: "+
                                           initialCapacity);
    }
}


/**
*构造包含指定collection元素的列表，这些元素利用该集合的迭代器按顺序返回
*如果指定的集合为null，throws NullPointerException。
*/
 public ArrayList(Collection<? extends E> c) {
    elementData = c.toArray();
    if ((size = elementData.length) != 0) {
        // c.toArray might (incorrectly) not return Object[] (see 6260652)
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        // replace with empty array.
        this.elementData = EMPTY_ELEMENTDATA;
    }
}
```

```java
/**
 * 基于上述代码的精简版本，只实现ArrayList集合存读和基础扩容，不考虑任何极端情况
 */
public class MyArrayList<E> {

    /**
     * ArrayList 所包含的元素个数
     */
    private int size;

    /**
     * 保存ArrayList数据的数组
     */
    transient Object[] array = {};

    private void grow(int minCapacity) {
        int oldCapacity = array.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0) newCapacity = minCapacity;
        array = Arrays.copyOf(array, newCapacity);
    }

    public E get(int index) {
        if (index >= size) throw new IndexOutOfBoundsException();
        return (E) array[index];
    }

    public int size() {
        return size;
    }

    public void add(E e) {
        grow(size + 1);
        array[size++] = e;
    }
}
```



**以无参数构造方法创建 `ArrayList` 时，实际上初始化赋值的是一个空数组。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为 10。**

> 补充：JDK6 new 无参构造的 `ArrayList` 对象时，直接创建了长度是 10 的 `Object[]` 数组 elementData 。



以无参构造函数创建的 ArrayList 为例，调用add()方法

```java
/**
 * 将指定的元素追加到此列表的末尾。
 */
public boolean add(E e) {
//添加元素之前，先调用ensureCapacityInternal方法
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    //这里看到ArrayList添加元素的实质就相当于为数组赋值
    elementData[size++] = e;
    return true;
}
```

> **注意**：JDK11 移除了 `ensureCapacityInternal()` 和 `ensureExplicitCapacity()` 方法



`ensureCapacityInternal()`

（JDK7）可以看到 `add` 方法 首先调用了`ensureCapacityInternal(size + 1)`

```java
//得到最小扩容量
private void ensureCapacityInternal(int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
          // 获取默认的容量和传入参数的较大值
        minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
    }

    ensureExplicitCapacity(minCapacity);
}
```

**当 要 add 进第 1 个元素时，minCapacity 为 1，在 Math.max()方法比较后，minCapacity 为 10。**

> 此处和后续 JDK8 代码格式化略有不同，核心代码基本一样。



`ensureExplicitCapacity()`

如果调用 `ensureCapacityInternal()` 方法就一定会进入（执行）这个方法

```java
//判断是否需要扩容
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        //调用grow方法进行扩容，调用此方法代表已经开始扩容了
        grow(minCapacity);
}
```

- 当我们要 add 进第 1 个元素到 ArrayList 时，elementData.length 为 0 （因为还是一个空的 list），因为执行了 `ensureCapacityInternal()` 方法 ，所以 minCapacity 此时为 10。此时，`minCapacity - elementData.length > 0`成立，所以会进入 `grow(minCapacity)` 方法。
- 当 add 第 2 个元素时，minCapacity 为 2，此时 elementData.length(容量)在添加第一个元素后扩容成 10 了。此时，`minCapacity - elementData.length > 0` 不成立，所以不会进入 （执行）`grow(minCapacity)` 方法。
- 添加第 3、4···到第 10 个元素时，依然不会执行 grow 方法，数组容量都为 10。

直到添加第 11 个元素，minCapacity(为 11)比 elementData.length（为 10）要大。进入 grow 方法进行扩容。



`grow()` 方法

```java
    /**
     * 要分配的最大数组大小
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

    /**
     * ArrayList扩容的核心方法。
     */
    private void grow(int minCapacity) {
        // oldCapacity为旧容量，newCapacity为新容量
        int oldCapacity = elementData.length;
        //将oldCapacity 右移一位，其效果相当于oldCapacity /2，
        //我们知道位运算的速度远远快于整除运算，整句运算式的结果就是将新容量更新为旧容量的1.5倍，
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        //然后检查新容量是否大于最小需要容量，若还是小于最小需要容量，那么就把最小需要容量当作数组的新容量，
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
       // 如果新容量大于 MAX_ARRAY_SIZE,进入(执行) `hugeCapacity()` 方法来比较 minCapacity 和 MAX_ARRAY_SIZE，
       //如果minCapacity大于最大容量，则新容量则为`Integer.MAX_VALUE`，否则，新容量大小则为 MAX_ARRAY_SIZE 即为 `Integer.MAX_VALUE - 8`。
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

**int newCapacity = oldCapacity + (oldCapacity >> 1),所以 ArrayList 每次扩容之后容量都会变为原来的 1.5 倍左右（oldCapacity 为偶数就是 1.5 倍，否则是 1.5 倍左右）！** 奇偶不同，比如：10+10/2 = 15, 33+33/2=49。如果是奇数的话会丢掉小数.

> ">>"（移位运算符）：>>1 右移一位相当于除 2，右移 n 位相当于除以 2 的 n 次方。这里 oldCapacity 明显右移了 1 位所以相当于 oldCapacity /2。对于大数据的 2 进制运算,位移运算符比那些普通运算符的运算要快很多,因为程序仅仅移动一下而已,不去计算,这样提高了效率,节省了资源

- 当 add 第 1 个元素时，oldCapacity 为 0，经比较后第一个 if 判断成立，newCapacity = minCapacity(为 10)。但是第二个 if 判断不会成立，即 newCapacity 不比 MAX_ARRAY_SIZE 大，则不会进入 `hugeCapacity` 方法。数组容量为 10，add 方法中 return true,size 增为 1。
- 当 add 第 11 个元素进入 grow 方法时，newCapacity 为 15，比 minCapacity（为 11）大，第一个 if 判断不成立。新容量没有大于数组最大 size，不会进入 hugeCapacity 方法。数组容量扩为 15，add 方法中 return true,size 增为 11。
- 以此类推······

> - java 中的 `length`属性是针对数组说的,比如说你声明了一个数组,想知道这个数组的长度则用到了 length 这个属性.
> - java 中的 `length()` 方法是针对字符串说的,如果想看这个字符串的长度则用到 `length()` 这个方法.
> - java 中的 `size()` 方法是针对泛型集合说的,如果想看这个泛型有多少个元素,就调用此方法来查看!



`hugeCapacity()` 方法。

从上面 `grow()` 方法源码可知：如果新容量大于 MAX_ARRAY_SIZE,进入(执行) `hugeCapacity()` 方法来比较 minCapacity 和 MAX_ARRAY_SIZE，如果 minCapacity 大于最大容量，则新容量则为`Integer.MAX_VALUE`，否则，新容量大小则为 MAX_ARRAY_SIZE 即为 `Integer.MAX_VALUE - 8`。

```java
private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    //对minCapacity和MAX_ARRAY_SIZE进行比较
    //若minCapacity大，将Integer.MAX_VALUE作为新数组的大小
    //若MAX_ARRAY_SIZE大，将MAX_ARRAY_SIZE作为新数组的大小
    //MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
        MAX_ARRAY_SIZE;
}
```

`System.arraycopy()` 方法

```java
// arraycopy 是一个 native 方法,接下来我们解释一下各个参数的具体意义
/**
*   复制数组
* @param src 源数组
* @param srcPos 源数组中的起始位置
* @param dest 目标数组
* @param destPos 目标数组中的起始位置
* @param length 要复制的数组元素的数量
*/
public static native void arraycopy(Object src,  int  srcPos,
                                    Object dest, int destPos,
                                    int length);
```

```java
/**
 * 在此列表中的指定位置插入指定的元素。
 *先调用 rangeCheckForAdd 对index进行界限检查；然后调用 ensureCapacityInternal 方法保证capacity足够大；
 *再将从index开始之后的所有成员后移一个位置；将element插入index位置；最后size加1。
 */
public void add(int index, E element) {
    rangeCheckForAdd(index);

    ensureCapacityInternal(size + 1);  // Increments modCount!!
    //arraycopy()方法实现数组自己复制自己
    //elementData:源数组;index:源数组中的起始位置;elementData：目标数组；index + 1：目标数组中的起始位置； size - index：要复制的数组元素的数量；
    System.arraycopy(elementData, index, elementData, index + 1, size - index);
    elementData[index] = element;
    size++;
}
```

```java
public class ArraycopyTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int[] a = new int[10];
		a[0] = 0;
		a[1] = 1;
		a[2] = 2;
		a[3] = 3;
		System.arraycopy(a, 2, a, 3, 3);
		a[2]=99;
		for (int i = 0; i < a.length; i++) {
			System.out.print(a[i] + " ");
		}
	}

}

// 0 1 99 2 3 0 0 0 0 0
```

`Arrays.copyOf()`方法

```java
public static int[] copyOf(int[] original, int newLength) {
    // 申请一个新的数组
    int[] copy = new int[newLength];
// 调用System.arraycopy,将源数组中的数据进行拷贝,并返回新的数组
    System.arraycopy(original, 0, copy, 0,
                     Math.min(original.length, newLength));
    return copy;
}
```

```java
/**
 以正确的顺序返回一个包含此列表中所有元素的数组（从第一个到最后一个元素）; 返回的数组的运行时类型是指定数组的运行时类型。
 */
public Object[] toArray() {
//elementData：要复制的数组；size：要复制的长度
    return Arrays.copyOf(elementData, size);
}
```



使用 `Arrays.copyOf()`方法主要是为了给原有数组扩容，测试代码如下：

```java
public class ArrayscopyOfTest {

	public static void main(String[] args) {
		int[] a = new int[3];
		a[0] = 0;
		a[1] = 1;
		a[2] = 2;
		int[] b = Arrays.copyOf(a, 10);
		System.out.println("b.length"+b.length);  // 10
	}
}
```

**联系：**

看两者源代码可以发现 `copyOf()`内部实际调用了 `System.arraycopy()` 方法

**区别：**

`arraycopy()` 需要目标数组，将原数组拷贝到你自己定义的数组里或者原数组，而且可以选择拷贝的起点和长度以及放入新数组中的位置 `copyOf()` 是系统自动在内部新建一个数组，并返回该数组。



`ensureCapacity`方法

`ArrayList` 源码中有一个 `ensureCapacity` 方法不知道大家注意到没有，这个方法 `ArrayList` 内部没有被调用过，所以很显然是提供给用户调用的，那么这个方法有什么作用呢？

```java
/**
 * 如有必要，增加此 ArrayList 实例的容量，以确保它至少可以容纳由minimum capacity参数指定的元素数。
 * @param   minCapacity   所需的最小容量
 */
public void ensureCapacity(int minCapacity) {
    int minExpand = (elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA)? 0: DEFAULT_CAPACITY;
    if (minCapacity > minExpand) {
        ensureExplicitCapacity(minCapacity);
    }
}
```

理论上来说，最好在向 `ArrayList` 添加大量元素之前用 `ensureCapacity` 方法，以减少增量重新分配的次数

```java
public class EnsureCapacityTest {
	public static void main(String[] args) {
		ArrayList<Object> list = new ArrayList<Object>();
		final int N = 10000000;
		long startTime = System.currentTimeMillis();
		for (int i = 0; i < N; i++) {
			list.add(i);
		}
		long endTime = System.currentTimeMillis();
		System.out.println("使用ensureCapacity方法前："+(endTime - startTime));

	}
}
```

```java
// 使用ensureCapacity方法前：2158
public class EnsureCapacityTest {
    public static void main(String[] args) {
        ArrayList<Object> list = new ArrayList<Object>();
        final int N = 10000000;
        long startTime1 = System.currentTimeMillis();
        list.ensureCapacity(N);
        for (int i = 0; i < N; i++) {
            list.add(i);
        }
        long endTime1 = System.currentTimeMillis();
        System.out.println("使用ensureCapacity方法后："+(endTime1 - startTime1));  // 使用ensureCapacity方法后：1773
    }
}
```

通过运行结果，可以看出向 `ArrayList` 添加大量元素之前使用`ensureCapacity` 方法可以提升性能。不过，这个性能差距几乎可以忽略不计。而且，实际项目根本也不可能往 `ArrayList` 里面添加这么多元素。



#### LinkedList

![linkedlist--class-diagram](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307161812585.png)

`LinkedList` 的类定义如下：

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{...}
```

`LinkedList` 继承了 `AbstractSequentialList` ，而 `AbstractSequentialList` 又继承于 `AbstractList` 。

`LinkedList` 实现了以下接口：

- `List` : 表明它是一个列表，支持添加、删除、查找等操作，并且可以通过下标进行访问。
- `Deque` ：继承自 `Queue` 接口，具有双端队列的特性，支持从两端插入和删除元素，方便实现栈和队列等数据结构。需要注意，`Deque` 的发音为 "deck" [dɛk]，这个大部分人都会读错。
- `Cloneable` ：表明它具有拷贝能力，可以进行深拷贝或浅拷贝操作。
- `Serializable` : 表明它可以进行序列化操作，也就是可以将对象转换为字节流进行持久化存储或网络传输，非常方便。

`LinkedList` 中的元素是通过 `Node` 定义的：



##### 初始化

`LinkedList` 中有一个无参构造函数和一个有参构造函数。

```java
// 创建一个空的链表对象
public LinkedList() {
}

// 接收一个集合类型作为参数，会创建一个与传入集合相同元素的链表对象
public LinkedList(Collection<? extends E> c) {
    this();
    addAll(c);
}
```

##### 插入元素

- `add(E e)`：用于在 `LinkedList` 的尾部插入元素，即将新元素作为链表的最后一个元素，时间复杂度为 O(1)。
- `add(int index, E element)`:用于在指定位置插入元素。这种插入方式需要先移动到指定位置，再修改指定节点的指针完成插入/删除，因此需要移动平均 n/2 个元素，时间复杂度为 O(n)。

```java
// 在链表尾部插入元素
public boolean add(E e) {
    linkLast(e);
    return true;
}

// 在链表指定位置插入元素
public void add(int index, E element) {
    // 下标越界检查
    checkPositionIndex(index);

    // 判断 index 是不是链表尾部位置
    if (index == size)
        // 如果是就直接调用 linkLast 方法将元素节点插入链表尾部即可
        linkLast(element);
    else
        // 如果不是则调用 linkBefore 方法将其插入指定元素之前
        linkBefore(element, node(index));
}

// 将元素节点插入到链表尾部
void linkLast(E e) {
    // 将最后一个元素赋值（引用传递）给节点 l
    final Node<E> l = last;
    // 创建节点，并指定节点前驱为链表尾节点 last，后继引用为空
    final Node<E> newNode = new Node<>(l, e, null);
    // 将 last 引用指向新节点
    last = newNode;
    // 判断尾节点是否为空
    // 如果 l 是null 意味着这是第一次添加元素
    if (l == null)
        // 如果是第一次添加，将first赋值为新节点，此时链表只有一个元素
        first = newNode;
    else
        // 如果不是第一次添加，将新节点赋值给l（添加前的最后一个元素）的next
        l.next = newNode;
    size++;
    modCount++;
}

// 在指定元素之前插入元素
void linkBefore(E e, Node<E> succ) {
    // assert succ != null;断言 succ不为 null
    // 定义一个节点元素保存 succ 的 prev 引用，也就是它的前一节点信息
    final Node<E> pred = succ.prev;
    // 初始化节点，并指明前驱和后继节点
    final Node<E> newNode = new Node<>(pred, e, succ);
    // 将 succ 节点前驱引用 prev 指向新节点
    succ.prev = newNode;
    // 判断尾节点是否为空，为空表示当前链表还没有节点
    if (pred == null)
        first = newNode;
    else
        // succ 节点前驱的后继引用指向新节点
        pred.next = newNode;
    size++;
    modCount++;
}
```



##### 获取元素

1. `getFirst()`：获取链表的第一个元素。
2. `getLast()`：获取链表的最后一个元素。
3. `get(int index)`：获取链表指定位置的元素。

```java
// 获取链表的第一个元素
public E getFirst() {
    final Node<E> f = first;
    if (f == null)
        throw new NoSuchElementException();
    return f.item;
}

// 获取链表的最后一个元素
public E getLast() {
    final Node<E> l = last;
    if (l == null)
        throw new NoSuchElementException();
    return l.item;
}

// 获取链表指定位置的元素
public E get(int index) {
  // 下标越界检查，如果越界就抛异常
  checkElementIndex(index);
  // 返回链表中对应下标的元素
  return node(index).item;
}
```

`node(int index)`

```java
// 返回指定下标的非空节点
Node<E> node(int index) {
    // 断言下标未越界
    // assert isElementIndex(index);
    // 如果index小于size的二分之一  从前开始查找（向后查找）  反之向前查找
    if (index < (size >> 1)) {
        Node<E> x = first;
        // 遍历，循环向后查找，直至 i == index
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```

`get(int index)` 或 `remove(int index)` 等方法内部都调用了该方法来获取对应的节点。该方法通过比较索引值与链表 size 的一半大小来确定从链表头还是尾开始遍历。如果索引值小于 size 的一半，就从链表头开始遍历，反之从链表尾开始遍历。这样可以在较短的时间内找到目标节点，充分利用了双向链表的特性来提高效率。



##### 删除元素

1. `removeFirst()`：删除并返回链表的第一个元素。
2. `removeLast()`：删除并返回链表的最后一个元素。
3. `remove(E e)`：删除链表中首次出现的指定元素，如果不存在该元素则返回 false。
4. `remove(int index)`：删除指定索引处的元素，并返回该元素的值。
5. `void clear()`：移除此链表中的所有元素。

```java
// 删除并返回链表的第一个元素
public E removeFirst() {
    final Node<E> f = first;
    if (f == null)
        throw new NoSuchElementException();
    return unlinkFirst(f);
}

// 删除并返回链表的最后一个元素
public E removeLast() {
    final Node<E> l = last;
    if (l == null)
        throw new NoSuchElementException();
    return unlinkLast(l);
}

// 删除链表中首次出现的指定元素，如果不存在该元素则返回 fals
public boolean remove(Object o) {
    // 如果指定元素为 null，遍历链表找到第一个为 null 的元素进行删除
    if (o == null) {
        for (Node<E> x = first; x != null; x = x.next) {
            if (x.item == null) {
                unlink(x);
                return true;
            }
        }
    } else {
        // 如果不为 null ,遍历链表找到要删除的节点
        for (Node<E> x = first; x != null; x = x.next) {
            if (o.equals(x.item)) {
                unlink(x);
                return true;
            }
        }
    }
    return false;
}

// 删除链表指定位置的元素
public E remove(int index) {
    // 下标越界检查，如果越界就抛异常
    checkElementIndex(index);
    return unlink(node(index));
}
```

`unlink(Node<E> x)`

```java
E unlink(Node<E> x) {
    // 断言 x 不为 null
    // assert x != null;
    // 获取当前节点（也就是待删除节点）的元素
    final E element = x.item;
    // 获取当前节点的下一个节点
    final Node<E> next = x.next;
    // 获取当前节点的前一个节点
    final Node<E> prev = x.prev;

    // 如果前一个节点为空，则说明当前节点是头节点
    if (prev == null) {
        // 直接让链表头指向当前节点的下一个节点
        first = next;
    } else { // 如果前一个节点不为空
        // 将前一个节点的 next 指针指向当前节点的下一个节点
        prev.next = next;
        // 将当前节点的 prev 指针置为 null，，方便 GC 回收
        x.prev = null;
    }

    // 如果下一个节点为空，则说明当前节点是尾节点
    if (next == null) {
        // 直接让链表尾指向当前节点的前一个节点
        last = prev;
    } else { // 如果下一个节点不为空
        // 将下一个节点的 prev 指针指向当前节点的前一个节点
        next.prev = prev;
        // 将当前节点的 next 指针置为 null，方便 GC 回收
        x.next = null;
    }

    // 将当前节点元素置为 null，方便 GC 回收
    x.item = null;
    size--;
    modCount++;
    return element;
}
```

`unlink()` 方法的逻辑如下：

1. 首先获取待删除节点 x 的前驱和后继节点；
2. 判断待删除节点是否为头节点或尾节点： 
   - 如果 x 是头节点，则将 first 指向 x 的后继节点 next
   - 如果 x 是尾节点，则将 last 指向 x 的前驱节点 prev
   - 如果 x 不是头节点也不是尾节点，执行下一步操作
3. 将待删除节点 x 的前驱的后继指向待删除节点的后继 next，断开 x 和 x.prev 之间的链接；
4. 将待删除节点 x 的后继的前驱指向待删除节点的前驱 prev，断开 x 和 x.next 之间的链接；
5. 将待删除节点 x 的元素置空，修改链表长度。



##### 遍历链表

推荐使用`for-each` 循环来遍历 `LinkedList` 中的元素， `for-each` 循环最终会转换成迭代器形式。

```java
LinkedList<String> list = new LinkedList<>();
list.add("apple");
list.add("banana");
list.add("pear");

for (String fruit : list) {
    System.out.println(fruit);
}
```

`LinkedList` 的遍历的核心就是它的迭代器的实现。

```java
// 双向迭代器
private class ListItr implements ListIterator<E> {
    // 表示上一次调用 next() 或 previous() 方法时经过的节点；
    private Node<E> lastReturned;
    // 表示下一个要遍历的节点；
    private Node<E> next;
    // 表示下一个要遍历的节点的下标，也就是当前节点的后继节点的下标；
    private int nextIndex;
    // 表示当前遍历期望的修改计数值，用于和 LinkedList 的 modCount 比较，判断链表是否被其他线程修改过。
    private int expectedModCount = modCount;
    …………
}
```

从头到尾方向的迭代：

```java
// 判断还有没有下一个节点
public boolean hasNext() {
    // 判断下一个节点的下标是否小于链表的大小，如果是则表示还有下一个元素可以遍历
    return nextIndex < size;
}
// 获取下一个节点
public E next() {
    // 检查在迭代过程中链表是否被修改过
    checkForComodification();
    // 判断是否还有下一个节点可以遍历，如果没有则抛出 NoSuchElementException 异常
    if (!hasNext())
        throw new NoSuchElementException();
    // 将 lastReturned 指向当前节点
    lastReturned = next;
    // 将 next 指向下一个节点
    next = next.next;
    nextIndex++;
    return lastReturned.item;
}
```

从尾到头方向的迭代：

```java
// 判断是否还有前一个节点
public boolean hasPrevious() {
    return nextIndex > 0;
}

// 获取前一个节点
public E previous() {
    // 检查是否在迭代过程中链表被修改
    checkForComodification();
    // 如果没有前一个节点，则抛出异常
    if (!hasPrevious())
        throw new NoSuchElementException();
    // 将 lastReturned 和 next 指针指向上一个节点
    lastReturned = next = (next == null) ? last : next.prev;
    nextIndex--;
    return lastReturned.item;
}
```

使用迭代器进行元素的插入和删除：

```java
LinkedList<String> list = new LinkedList<>();
list.add("apple");
list.add(null);
list.add("banana");

//  Collection 接口的 removeIf 方法底层依然是基于迭代器
list.removeIf(Objects::isNull);

for (String fruit : list) {
    System.out.println(fruit);
}
```

迭代器对应的移除元素的方法如下：

```java
// 从列表中删除上次被返回的元素
public void remove() {
    // 检查是否在迭代过程中链表被修改
    checkForComodification();
    // 如果上次返回的节点为空，则抛出异常
    if (lastReturned == null)
        throw new IllegalStateException();

    // 获取当前节点的下一个节点
    Node<E> lastNext = lastReturned.next;
    // 从链表中删除上次返回的节点
    unlink(lastReturned);
    // 修改指针
    if (next == lastReturned)
        next = lastNext;
    else
        nextIndex--;
    // 将上次返回的节点引用置为 null，方便 GC 回收
    lastReturned = null;
    expectedModCount++;
}
```



##### LinkedList 常用方法测试

```java
// 创建 LinkedList 对象
LinkedList<String> list = new LinkedList<>();

// 添加元素到链表末尾
list.add("apple");
list.add("banana");
list.add("pear");
System.out.println("链表内容：" + list);

// 在指定位置插入元素
list.add(1, "orange");
System.out.println("链表内容：" + list);

// 获取指定位置的元素
String fruit = list.get(2);
System.out.println("索引为 2 的元素：" + fruit);

// 修改指定位置的元素
list.set(3, "grape");
System.out.println("链表内容：" + list);

// 删除指定位置的元素
list.remove(0);
System.out.println("链表内容：" + list);

// 删除第一个出现的指定元素
list.remove("banana");
System.out.println("链表内容：" + list);

// 获取链表的长度
int size = list.size();
System.out.println("链表长度：" + size);

// 清空链表
list.clear();
System.out.println("清空后的链表：" + list);
```

```text
索引为 2 的元素：banana
链表内容：[apple, orange, banana, grape]
链表内容：[orange, banana, grape]
链表内容：[orange, grape]
链表长度：2
清空后的链表：[]
```



#### HashMap

**类的属性：**

```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable {
    // 序列号
    private static final long serialVersionUID = 362498820763181265L;
    // 默认的初始容量是16
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;
    // 最大容量
    static final int MAXIMUM_CAPACITY = 1 << 30;
    // 默认的负载因子
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
    // 当桶(bucket)上的结点数大于等于这个值时会转成红黑树
    static final int TREEIFY_THRESHOLD = 8;
    // 当桶(bucket)上的结点数小于等于这个值时树转链表
    static final int UNTREEIFY_THRESHOLD = 6;
    // 桶中结构转化为红黑树对应的table的最小容量
    static final int MIN_TREEIFY_CAPACITY = 64;
    // 存储元素的数组，总是2的幂次倍
    transient Node<K,V>[] table;
    // 存放具体元素的集
    transient Set<Map.Entry<K,>> entrySet;
    // 存放元素的个数，注意这个不等于数组的长度
    transient int size;
    // 每次扩容和更改map结构的计数器
    transient int modCount;
    // 阈值(容量*负载因子) 当实际大小超过阈值时，会进行扩容
    int threshold;
    // 负载因子
    final float loadFactor;
}
```

- **loadFactor 负载因子**

  loadFactor 负载因子是控制数组存放数据的疏密程度，loadFactor 越趋近于 1，那么 数组中存放的数据(entry)也就越多，也就越密，也就是会让链表的长度增加，loadFactor 越小，也就是趋近于 0，数组中存放的数据(entry)也就越少，也就越稀疏。

  **loadFactor 太大导致查找元素效率低，太小导致数组的利用率低，存放的数据会很分散。loadFactor 的默认值为 0.75f 是官方给出的一个比较好的临界值**。

  给定的默认容量为 16，负载因子为 0.75。Map 在使用过程中不断的往里面存放数据，当数量超过了 16 * 0.75 = 12 就需要将当前 16 的容量进行扩容，而扩容这个过程涉及到 rehash、复制数据等操作，所以非常消耗性能。

- **threshold**

  **threshold = capacity \* loadFactor**，**当 size > threshold** 的时候，那么就要考虑对数组的扩增了，也就是说，这个的意思就是 **衡量数组是否需要扩增的一个标准**。

**Node 节点类源码:**

```java
// 继承自 Map.Entry<K,V>
static class Node<K,V> implements Map.Entry<K,V> {
       final int hash;// 哈希值，存放元素到hashmap中时用来与其他元素hash值比较
       final K key;//键
       V value;//值
       // 指向下一个节点
       Node<K,V> next;
       Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }
        // 重写hashCode()方法
        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }
        // 重写 equals() 方法
        public final boolean equals(Object o) {
            if (o == this)
                return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
}
```

**树节点类源码:**

```java
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        TreeNode<K,V> parent;  // 父
        TreeNode<K,V> left;    // 左
        TreeNode<K,V> right;   // 右
        TreeNode<K,V> prev;    // needed to unlink next upon deletion
        boolean red;           // 判断颜色
        TreeNode(int hash, K key, V val, Node<K,V> next) {
            super(hash, key, val, next);
        }
        // 返回根节点
        final TreeNode<K,V> root() {
            for (TreeNode<K,V> r = this, p;;) {
                if ((p = r.parent) == null)
                    return r;
                r = p;
       }
```

##### HashMap 源码分析

###### 构造方法

HashMap 中有四个构造方法，它们分别如下：

```java
// 默认构造函数。
public HashMap() {
    this.loadFactor = DEFAULT_LOAD_FACTOR; // all   other fields defaulted
 }

 // 包含另一个“Map”的构造函数
 public HashMap(Map<? extends K, ? extends V> m) {
     this.loadFactor = DEFAULT_LOAD_FACTOR;
     putMapEntries(m, false);//下面会分析到这个方法
 }

 // 指定“容量大小”的构造函数
 public HashMap(int initialCapacity) {
     this(initialCapacity, DEFAULT_LOAD_FACTOR);
 }

 // 指定“容量大小”和“负载因子”的构造函数
 public HashMap(int initialCapacity, float loadFactor) {
     if (initialCapacity < 0)
         throw new IllegalArgumentException("Illegal initial capacity: " + initialCapacity);
     if (initialCapacity > MAXIMUM_CAPACITY)
         initialCapacity = MAXIMUM_CAPACITY;
     if (loadFactor <= 0 || Float.isNaN(loadFactor))
         throw new IllegalArgumentException("Illegal load factor: " + loadFactor);
     this.loadFactor = loadFactor;
     // 初始容量暂时存放到 threshold ，在resize中再赋值给 newCap 进行table初始化
     this.threshold = tableSizeFor(initialCapacity);
 }
```

> 值得注意的是上述四个构造方法中，都初始化了负载因子 loadFactor，由于HashMap中没有 capacity 这样的字段，即使指定了初始化容量 initialCapacity ，也只是通过 tableSizeFor 将其扩容到与 initialCapacity 最接近的2的幂次方大小，然后暂时赋值给 threshold ，后续通过 resize 方法将 threshold 赋值给 newCap 进行 table 的初始化。

**putMapEntries 方法：**

```java
final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) {
    int s = m.size();
    if (s > 0) {
        // 判断table是否已经初始化
        if (table == null) { // pre-size
            /*
             * 未初始化，s为m的实际元素个数，ft=s/loadFactor => s=ft*loadFactor, 跟我们前面提到的
             * 阈值=容量*负载因子 是不是很像，是的，ft指的是要添加s个元素所需的最小的容量
             */
            float ft = ((float)s / loadFactor) + 1.0F;
            int t = ((ft < (float)MAXIMUM_CAPACITY) ?
                    (int)ft : MAXIMUM_CAPACITY);
            /*
             * 根据构造函数可知，table未初始化，threshold实际上是存放的初始化容量，如果添加s个元素所
             * 需的最小容量大于初始化容量，则将最小容量扩容为最接近的2的幂次方大小作为初始化。
             * 注意这里不是初始化阈值
             */
            if (t > threshold)
                threshold = tableSizeFor(t);
        }
        // 已初始化，并且m元素个数大于阈值，进行扩容处理
        else if (s > threshold)
            resize();
        // 将m中的所有元素添加至HashMap中，如果table未初始化，putVal中会调用resize初始化或扩容
        for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
            K key = e.getKey();
            V value = e.getValue();
            putVal(hash(key), key, value, false, evict);
        }
    }
}
```

###### put 方法

HashMap 只提供了 put 用于添加元素，putVal 方法只是给 put 方法调用的一个方法，并没有提供给用户使用。

**对 putVal 方法添加元素的分析如下：**

1. 如果定位到的数组位置没有元素 就直接插入。
2. 如果定位到的数组位置有元素就和要插入的 key 比较，如果 key 相同就直接覆盖，如果 key 不相同，就判断 p 是否是一个树节点，如果是就调用`e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value)`将元素添加进入。如果不是就遍历链表插入(插入的是链表尾部)。

![put](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307161954288.png)

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    // table未初始化或者长度为0，进行扩容
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    // (n - 1) & hash 确定元素存放在哪个桶中，桶为空，新生成结点放入桶中(此时，这个结点是放在数组中)
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    // 桶中已经存在元素（处理hash冲突）
    else {
        Node<K,V> e; K k;
        //快速判断第一个节点table[i]的key是否与插入的key一样，若相同就直接使用插入的值p替换掉旧的值e。
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
        // 判断插入的是否是红黑树节点
        else if (p instanceof TreeNode)
            // 放入树中
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        // 不是红黑树节点则说明为链表结点
        else {
            // 在链表最末插入结点
            for (int binCount = 0; ; ++binCount) {
                // 到达链表的尾部
                if ((e = p.next) == null) {
                    // 在尾部插入新结点
                    p.next = newNode(hash, key, value, null);
                    // 结点数量达到阈值(默认为 8 )，执行 treeifyBin 方法
                    // 这个方法会根据 HashMap 数组来决定是否转换为红黑树。
                    // 只有当数组长度大于或者等于 64 的情况下，才会执行转换红黑树操作，以减少搜索时间。否则，就是只是对数组扩容。
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    // 跳出循环
                    break;
                }
                // 判断链表中结点的key值与插入的元素的key值是否相等
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    // 相等，跳出循环
                    break;
                // 用于遍历桶中的链表，与前面的e = p.next组合，可以遍历链表
                p = e;
            }
        }
        // 表示在桶中找到key值、hash值与插入元素相等的结点
        if (e != null) {
            // 记录e的value
            V oldValue = e.value;
            // onlyIfAbsent为false或者旧值为null
            if (!onlyIfAbsent || oldValue == null)
                //用新值替换旧值
                e.value = value;
            // 访问后回调
            afterNodeAccess(e);
            // 返回旧值
            return oldValue;
        }
    }
    // 结构性修改
    ++modCount;
    // 实际大小大于阈值则扩容
    if (++size > threshold)
        resize();
    // 插入后回调
    afterNodeInsertion(evict);
    return null;
}
```

**我们再来对比一下 JDK1.7 put 方法的代码**

**对于 put 方法的分析如下：**

- ① 如果定位到的数组位置没有元素 就直接插入。
- ② 如果定位到的数组位置有元素，遍历以这个元素为头结点的链表，依次和插入的 key 比较，如果 key 相同就直接覆盖，不同就采用头插法插入元素。

```java
public V put(K key, V value)
    if (table == EMPTY_TABLE) {
    inflateTable(threshold);
}
    if (key == null)
        return putForNullKey(value);
    int hash = hash(key);
    int i = indexFor(hash, table.length);
    for (Entry<K,V> e = table[i]; e != null; e = e.next) { // 先遍历
        Object k;
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }

    modCount++;
    addEntry(hash, key, value, i);  // 再插入
    return null;
}
```

###### get 方法

```java
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}

final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        // 数组元素相等
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        // 桶中不止一个节点
        if ((e = first.next) != null) {
            // 在树中get
            if (first instanceof TreeNode)
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            // 在链表中get
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```

###### resize 方法

进行扩容，会伴随着一次重新 hash 分配，并且会遍历 hash 表中所有的元素，是非常耗时的。在编写程序中，要尽量避免 resize。resize方法实际上是将 table 初始化和 table 扩容 进行了整合，底层的行为都是给 table 赋值一个新的数组。

```java
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    int oldThr = threshold;
    int newCap, newThr = 0;
    if (oldCap > 0) {
        // 超过最大值就不再扩充了，就只好随你碰撞去吧
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        // 没超过最大值，就扩充为原来的2倍
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY && oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold
    }
    else if (oldThr > 0) // initial capacity was placed in threshold
        // 创建对象时初始化容量大小放在threshold中，此时只需要将其作为新的数组容量
        newCap = oldThr;
    else {
        // signifies using defaults 无参构造函数创建的对象在这里计算容量和阈值
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    if (newThr == 0) {
        // 创建时指定了初始化容量或者负载因子，在这里进行阈值初始化，
    	// 或者扩容前的旧容量小于16，在这里计算新的resize上限
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ? (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr;
    @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    table = newTab;
    if (oldTab != null) {
        // 把每个bucket都移动到新的buckets中
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;
                if (e.next == null)
                    // 只有一个节点，直接计算元素新的位置即可
                    newTab[e.hash & (newCap - 1)] = e;
                else if (e instanceof TreeNode)
                    // 将红黑树拆分成2棵子树，拆分后的子树节点数小于等于6，则将树转化成链表
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else {
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {
                        next = e.next;
                        // 原索引
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        // 原索引+oldCap
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    // 原索引放到bucket里
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    // 原索引+oldCap放到bucket里
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

##### HashMap 常用方法测试

```java
package map;

import java.util.Collection;
import java.util.HashMap;
import java.util.Set;

public class HashMapDemo {

    public static void main(String[] args) {
        HashMap<String, String> map = new HashMap<String, String>();
        // 键不能重复，值可以重复
        map.put("san", "张三");
        map.put("si", "李四");
        map.put("wu", "王五");
        map.put("wang", "老王");
        map.put("wang", "老王2");// 老王被覆盖
        map.put("lao", "老王");
        System.out.println("-------直接输出hashmap:-------");
        System.out.println(map);
        /**
         * 遍历HashMap
         */
        // 1.获取Map中的所有键
        System.out.println("-------foreach获取Map中所有的键:------");
        Set<String> keys = map.keySet();
        for (String key : keys) {
            System.out.print(key+"  ");
        }
        System.out.println();//换行
        // 2.获取Map中所有值
        System.out.println("-------foreach获取Map中所有的值:------");
        Collection<String> values = map.values();
        for (String value : values) {
            System.out.print(value+"  ");
        }
        System.out.println();//换行
        // 3.得到key的值的同时得到key所对应的值
        System.out.println("-------得到key的值的同时得到key所对应的值:-------");
        Set<String> keys2 = map.keySet();
        for (String key : keys2) {
            System.out.print(key + "：" + map.get(key)+"   ");

        }
        /**
         * 如果既要遍历key又要value，那么建议这种方式，因为如果先获取keySet然后再执行map.get(key)，map内部会执行两次遍历。
         * 一次是在获取keySet的时候，一次是在遍历所有key的时候。
         */
        // 当我调用put(key,value)方法的时候，首先会把key和value封装到
        // Entry这个静态内部类对象中，把Entry对象再添加到数组中，所以我们想获取
        // map中的所有键值对，我们只要获取数组中的所有Entry对象，接下来
        // 调用Entry对象中的getKey()和getValue()方法就能获取键值对了
        Set<java.util.Map.Entry<String, String>> entrys = map.entrySet();
        for (java.util.Map.Entry<String, String> entry : entrys) {
            System.out.println(entry.getKey() + "--" + entry.getValue());
        }

        /**
         * HashMap其他常用方法
         */
        System.out.println("after map.size()："+map.size());
        System.out.println("after map.isEmpty()："+map.isEmpty());
        System.out.println(map.remove("san"));
        System.out.println("after map.remove()："+map);
        System.out.println("after map.get(si)："+map.get("si"));
        System.out.println("after map.containsKey(si)："+map.containsKey("si"));
        System.out.println("after containsValue(李四)："+map.containsValue("李四"));
        System.out.println(map.replace("si", "李四2"));
        System.out.println("after map.replace(si, 李四2):"+map);
    }

}
```



#### ConcurrentHashMap

##### 1.7存储结构

![Java 7 ConcurrentHashMap 存储结构](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172057249.png)

Java 7 中 `ConcurrentHashMap` 的存储结构如上图，`ConcurrnetHashMap` 由很多个 `Segment` 组合，而每一个 `Segment` 是一个类似于 `HashMap` 的结构，所以每一个 `HashMap` 的内部可以进行扩容。但是 `Segment` 的个数一旦**初始化就不能改变**，默认 `Segment` 的个数是 16 个，可以认为 `ConcurrentHashMap` 默认支持最多 16 个线程并发。



##### 初始化

通过 `ConcurrentHashMap` 的无参构造探寻 `ConcurrentHashMap` 的初始化流程。

```java
    /**
     * Creates a new, empty map with a default initial capacity (16),
     * load factor (0.75) and concurrencyLevel (16).
     */
    public ConcurrentHashMap() {
        this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR, DEFAULT_CONCURRENCY_LEVEL);
    }
```

无参构造中调用了有参构造，传入了三个参数的默认值，他们的值是。

```java
    /**
     * 默认初始化容量
     */
    static final int DEFAULT_INITIAL_CAPACITY = 16;

    /**
     * 默认负载因子
     */
    static final float DEFAULT_LOAD_FACTOR = 0.75f;

    /**
     * 默认并发级别
     */
    static final int DEFAULT_CONCURRENCY_LEVEL = 16;
```

接着看下这个有参构造函数的内部实现逻辑。

```java
@SuppressWarnings("unchecked")
public ConcurrentHashMap(int initialCapacity,float loadFactor, int concurrencyLevel) {
    // 参数校验
    if (!(loadFactor > 0) || initialCapacity < 0 || concurrencyLevel <= 0)
        throw new IllegalArgumentException();
    // 校验并发级别大小，大于 1<<16，重置为 65536
    if (concurrencyLevel > MAX_SEGMENTS)
        concurrencyLevel = MAX_SEGMENTS;
    // Find power-of-two sizes best matching arguments
    // 2的多少次方
    int sshift = 0;
    int ssize = 1;
    // 这个循环可以找到 concurrencyLevel 之上最近的 2的次方值
    while (ssize < concurrencyLevel) {
        ++sshift;
        ssize <<= 1;
    }
    // 记录段偏移量
    this.segmentShift = 32 - sshift;
    // 记录段掩码
    this.segmentMask = ssize - 1;
    // 设置容量
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    // c = 容量 / ssize ，默认 16 / 16 = 1，这里是计算每个 Segment 中的类似于 HashMap 的容量
    int c = initialCapacity / ssize;
    if (c * ssize < initialCapacity)
        ++c;
    int cap = MIN_SEGMENT_TABLE_CAPACITY;
    //Segment 中的类似于 HashMap 的容量至少是2或者2的倍数
    while (cap < c)
        cap <<= 1;
    // create segments and segments[0]
    // 创建 Segment 数组，设置 segments[0]
    Segment<K,V> s0 = new Segment<K,V>(loadFactor, (int)(cap * loadFactor),
                         (HashEntry<K,V>[])new HashEntry[cap]);
    Segment<K,V>[] ss = (Segment<K,V>[])new Segment[ssize];
    UNSAFE.putOrderedObject(ss, SBASE, s0); // ordered write of segments[0]
    this.segments = ss;
}
```

总结一下在 Java 7 中 ConcurrentHashMap 的初始化逻辑。

1. 必要参数校验。
2. 校验并发级别 `concurrencyLevel` 大小，如果大于最大值，重置为最大值。无参构造**默认值是 16.**
3. 寻找并发级别 `concurrencyLevel` 之上最近的 **2 的幂次方**值，作为初始化容量大小，**默认是 16**。
4. 记录 `segmentShift` 偏移量，这个值为【容量 = 2 的 N 次方】中的 N，在后面 Put 时计算位置时会用到。**默认是 32 - sshift = 28**.
5. 记录 `segmentMask`，默认是 ssize - 1 = 16 -1 = 15.
6. **初始化 `segments[0]`**，**默认大小为 2**，**负载因子 0.75**，**扩容阀值是 2\*0.75=1.5**，插入第二个值时才会进行扩容。



##### put

接着上面的初始化参数继续查看 put 方法源码。

```java
public V put(K key, V value) {
    Segment<K,V> s;
    if (value == null)
        throw new NullPointerException();
    int hash = hash(key);
    // hash 值无符号右移 28位（初始化时获得），然后与 segmentMask=15 做与运算
    // 其实也就是把高4位与segmentMask（1111）做与运算
    int j = (hash >>> segmentShift) & segmentMask;
    if ((s = (Segment<K,V>)UNSAFE.getObject          // nonvolatile; recheck
         (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment
        // 如果查找到的 Segment 为空，初始化
        s = ensureSegment(j);
    return s.put(key, hash, value, false);
}

@SuppressWarnings("unchecked")
private Segment<K,V> ensureSegment(int k) {
    final Segment<K,V>[] ss = this.segments;
    long u = (k << SSHIFT) + SBASE; // raw offset
    Segment<K,V> seg;
    // 判断 u 位置的 Segment 是否为null
    if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u)) == null) {
        Segment<K,V> proto = ss[0]; // use segment 0 as prototype
        // 获取0号 segment 里的 HashEntry<K,V> 初始化长度
        int cap = proto.table.length;
        // 获取0号 segment 里的 hash 表里的扩容负载因子，所有的 segment 的 loadFactor 是相同的
        float lf = proto.loadFactor;
        // 计算扩容阀值
        int threshold = (int)(cap * lf);
        // 创建一个 cap 容量的 HashEntry 数组
        HashEntry<K,V>[] tab = (HashEntry<K,V>[])new HashEntry[cap];
        if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u)) == null) { // recheck
            // 再次检查 u 位置的 Segment 是否为null，因为这时可能有其他线程进行了操作
            Segment<K,V> s = new Segment<K,V>(lf, threshold, tab);
            // 自旋检查 u 位置的 Segment 是否为null
            while ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))
                   == null) {
                // 使用CAS 赋值，只会成功一次
                if (UNSAFE.compareAndSwapObject(ss, u, null, seg = s))
                    break;
            }
        }
    }
    return seg;
}
```

上面的源码分析了 `ConcurrentHashMap` 在 put 一个数据时的处理流程，下面梳理下具体流程。

1. 计算要 put 的 key 的位置，获取指定位置的 `Segment`。

2. 如果指定位置的 `Segment` 为空，则初始化这个 `Segment`.

   **初始化 Segment 流程：**

   1. 检查计算得到的位置的 `Segment` 是否为 null.
   2. 为 null 继续初始化，使用 `Segment[0]` 的容量和负载因子创建一个 `HashEntry` 数组。
   3. 再次检查计算得到的指定位置的 `Segment` 是否为 null.
   4. 使用创建的 `HashEntry` 数组初始化这个 Segment.
   5. 自旋判断计算得到的指定位置的 `Segment` 是否为 null，使用 CAS 在这个位置赋值为 `Segment`.

3. `Segment.put` 插入 key,value 值。



上面探究了获取 `Segment` 段和初始化 `Segment` 段的操作。最后一行的 `Segment` 的 put 方法还没有查看，继续分析。

```java
final V put(K key, int hash, V value, boolean onlyIfAbsent) {
    // 获取 ReentrantLock 独占锁，获取不到，scanAndLockForPut 获取。
    HashEntry<K,V> node = tryLock() ? null : scanAndLockForPut(key, hash, value);
    V oldValue;
    try {
        HashEntry<K,V>[] tab = table;
        // 计算要put的数据位置
        int index = (tab.length - 1) & hash;
        // CAS 获取 index 坐标的值
        HashEntry<K,V> first = entryAt(tab, index);
        for (HashEntry<K,V> e = first;;) {
            if (e != null) {
                // 检查是否 key 已经存在，如果存在，则遍历链表寻找位置，找到后替换 value
                K k;
                if ((k = e.key) == key ||
                    (e.hash == hash && key.equals(k))) {
                    oldValue = e.value;
                    if (!onlyIfAbsent) {
                        e.value = value;
                        ++modCount;
                    }
                    break;
                }
                e = e.next;
            }
            else {
                // first 有值没说明 index 位置已经有值了，有冲突，链表头插法。
                if (node != null)
                    node.setNext(first);
                else
                    node = new HashEntry<K,V>(hash, key, value, first);
                int c = count + 1;
                // 容量大于扩容阀值，小于最大容量，进行扩容
                if (c > threshold && tab.length < MAXIMUM_CAPACITY)
                    rehash(node);
                else
                    // index 位置赋值 node，node 可能是一个元素，也可能是一个链表的表头
                    setEntryAt(tab, index, node);
                ++modCount;
                count = c;
                oldValue = null;
                break;
            }
        }
    } finally {
        unlock();
    }
    return oldValue;
}
```

由于 `Segment` 继承了 `ReentrantLock`，所以 `Segment` 内部可以很方便的获取锁，put 流程就用到了这个功能。

1. `tryLock()` 获取锁，获取不到使用 **`scanAndLockForPut`** 方法继续获取。

2. 计算 put 的数据要放入的 index 位置，然后获取这个位置上的 `HashEntry` 。

3. 遍历 put 新元素，为什么要遍历？因为这里获取的 `HashEntry` 可能是一个空元素，也可能是链表已存在，所以要区别对待。

   如果这个位置上的 **`HashEntry` 不存在**：

   1. 如果当前容量大于扩容阀值，小于最大容量，**进行扩容**。
   2. 直接头插法插入。

   如果这个位置上的 **`HashEntry` 存在**：

   1. 判断链表当前元素 key 和 hash 值是否和要 put 的 key 和 hash 值一致。一致则替换值
   2. 不一致，获取链表下一个节点，直到发现相同进行值替换，或者链表表里完毕没有相同的。 
      1. 如果当前容量大于扩容阀值，小于最大容量，**进行扩容**。
      2. 直接链表头插法插入。

4. 如果要插入的位置之前已经存在，替换后返回旧值，否则返回 null.

这里面的第一步中的 `scanAndLockForPut` 操作这里没有介绍，这个方法做的操作就是不断的自旋 `tryLock()` 获取锁。当自旋次数大于指定次数时，使用 `lock()` 阻塞获取锁。在自旋时顺表获取下 hash 位置的 `HashEntry`。

```java
private HashEntry<K,V> scanAndLockForPut(K key, int hash, V value) {
    HashEntry<K,V> first = entryForHash(this, hash);
    HashEntry<K,V> e = first;
    HashEntry<K,V> node = null;
    int retries = -1; // negative while locating node
    // 自旋获取锁
    while (!tryLock()) {
        HashEntry<K,V> f; // to recheck first below
        if (retries < 0) {
            if (e == null) {
                if (node == null) // speculatively create node
                    node = new HashEntry<K,V>(hash, key, value, null);
                retries = 0;
            }
            else if (key.equals(e.key))
                retries = 0;
            else
                e = e.next;
        }
        else if (++retries > MAX_SCAN_RETRIES) {
            // 自旋达到指定次数后，阻塞等到只到获取到锁
            lock();
            break;
        }
        else if ((retries & 1) == 0 &&
                 (f = entryForHash(this, hash)) != first) {
            e = first = f; // re-traverse if entry changed
            retries = -1;
        }
    }
    return node;
}
```

##### 扩容 rehash

`ConcurrentHashMap` 的扩容只会扩容到原来的两倍。老数组里的数据移动到新的数组时，位置要么不变，要么变为 `index+ oldSize`，参数里的 node 会在扩容之后使用链表**头插法**插入到指定位置。

```java
private void rehash(HashEntry<K,V> node) {
    HashEntry<K,V>[] oldTable = table;
    // 老容量
    int oldCapacity = oldTable.length;
    // 新容量，扩大两倍
    int newCapacity = oldCapacity << 1;
    // 新的扩容阀值
    threshold = (int)(newCapacity * loadFactor);
    // 创建新的数组
    HashEntry<K,V>[] newTable = (HashEntry<K,V>[]) new HashEntry[newCapacity];
    // 新的掩码，默认2扩容后是4，-1是3，二进制就是11。
    int sizeMask = newCapacity - 1;
    for (int i = 0; i < oldCapacity ; i++) {
        // 遍历老数组
        HashEntry<K,V> e = oldTable[i];
        if (e != null) {
            HashEntry<K,V> next = e.next;
            // 计算新的位置，新的位置只可能是不便或者是老的位置+老的容量。
            int idx = e.hash & sizeMask;
            if (next == null)   //  Single node on list
                // 如果当前位置还不是链表，只是一个元素，直接赋值
                newTable[idx] = e;
            else { // Reuse consecutive sequence at same slot
                // 如果是链表了
                HashEntry<K,V> lastRun = e;
                int lastIdx = idx;
                // 新的位置只可能是不便或者是老的位置+老的容量。
                // 遍历结束后，lastRun 后面的元素位置都是相同的
                for (HashEntry<K,V> last = next; last != null; last = last.next) {
                    int k = last.hash & sizeMask;
                    if (k != lastIdx) {
                        lastIdx = k;
                        lastRun = last;
                    }
                }
                // ，lastRun 后面的元素位置都是相同的，直接作为链表赋值到新位置。
                newTable[lastIdx] = lastRun;
                // Clone remaining nodes
                for (HashEntry<K,V> p = e; p != lastRun; p = p.next) {
                    // 遍历剩余元素，头插法到指定 k 位置。
                    V v = p.value;
                    int h = p.hash;
                    int k = h & sizeMask;
                    HashEntry<K,V> n = newTable[k];
                    newTable[k] = new HashEntry<K,V>(h, p.key, v, n);
                }
            }
        }
    }
    // 头插法插入新的节点
    int nodeIndex = node.hash & sizeMask; // add the new node
    node.setNext(newTable[nodeIndex]);
    newTable[nodeIndex] = node;
    table = newTable;
}
```

有些同学可能会对最后的两个 for 循环有疑惑，这里第一个 for 是为了寻找这样一个节点，这个节点后面的所有 next 节点的新位置都是相同的。然后把这个作为一个链表赋值到新位置。第二个 for 循环是为了把剩余的元素通过头插法插入到指定位置链表。这样实现的原因可能是基于概率统计，有深入研究的同学可以发表下意见。

##### get

到这里就很简单了，get 方法只需要两步即可。

1. 计算得到 key 的存放位置。
2. 遍历指定位置查找相同 key 的 value 值。

```java
public V get(Object key) {
    Segment<K,V> s; // manually integrate access methods to reduce overhead
    HashEntry<K,V>[] tab;
    int h = hash(key);
    long u = (((h >>> segmentShift) & segmentMask) << SSHIFT) + SBASE;
    // 计算得到 key 的存放位置
    if ((s = (Segment<K,V>)UNSAFE.getObjectVolatile(segments, u)) != null &&
        (tab = s.table) != null) {
        for (HashEntry<K,V> e = (HashEntry<K,V>) UNSAFE.getObjectVolatile
                 (tab, ((long)(((tab.length - 1) & h)) << TSHIFT) + TBASE);
             e != null; e = e.next) {
            // 如果是链表，遍历查找到相同 key 的 value。
            K k;
            if ((k = e.key) == key || (e.hash == h && key.equals(k)))
                return e.value;
        }
    }
    return null;
}
```



##### 1.8存储结构

![image-20230717205942564](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172059786.png)

可以发现 Java8 的 ConcurrentHashMap 相对于 Java7 来说变化比较大，不再是之前的 **Segment 数组 + HashEntry 数组 + 链表**，而是 **Node 数组 + 链表 / 红黑树**。当冲突链表达到一定长度时，链表会转换成红黑树。



##### 初始化

```java
/**
 * Initializes table, using the size recorded in sizeCtl.
 */
private final Node<K,V>[] initTable() {
    Node<K,V>[] tab; int sc;
    while ((tab = table) == null || tab.length == 0) {
        //　如果 sizeCtl < 0 ,说明另外的线程执行CAS 成功，正在进行初始化。
        if ((sc = sizeCtl) < 0)
            // 让出 CPU 使用权
            Thread.yield(); // lost initialization race; just spin
        else if (U.compareAndSwapInt(this, SIZECTL, sc, -1)) {
            try {
                if ((tab = table) == null || tab.length == 0) {
                    int n = (sc > 0) ? sc : DEFAULT_CAPACITY;
                    @SuppressWarnings("unchecked")
                    Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                    table = tab = nt;
                    sc = n - (n >>> 2);
                }
            } finally {
                sizeCtl = sc;
            }
            break;
        }
    }
    return tab;
}
```

从源码中可以发现 `ConcurrentHashMap` 的初始化是通过**自旋和 CAS** 操作完成的。里面需要注意的是变量 `sizeCtl` ，它的值决定着当前的初始化状态。

1. -1 说明正在初始化
2. -N 说明有 N-1 个线程正在进行扩容
3. 0 表示 table 初始化大小，如果 table 没有初始化
4. \>0 表示 table 扩容的阈值，如果 table 已经初始化。

##### put

直接过一遍 put 源码。

```java
public V put(K key, V value) {
    return putVal(key, value, false);
}

/** Implementation for put and putIfAbsent */
final V putVal(K key, V value, boolean onlyIfAbsent) {
    // key 和 value 不能为空
    if (key == null || value == null) throw new NullPointerException();
    int hash = spread(key.hashCode());
    int binCount = 0;
    for (Node<K,V>[] tab = table;;) {
        // f = 目标位置元素
        Node<K,V> f; int n, i, fh;// fh 后面存放目标位置的元素 hash 值
        if (tab == null || (n = tab.length) == 0)
            // 数组桶为空，初始化数组桶（自旋+CAS)
            tab = initTable();
        else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
            // 桶内为空，CAS 放入，不加锁，成功了就直接 break 跳出
            if (casTabAt(tab, i, null,new Node<K,V>(hash, key, value, null)))
                break;  // no lock when adding to empty bin
        }
        else if ((fh = f.hash) == MOVED)
            tab = helpTransfer(tab, f);
        else {
            V oldVal = null;
            // 使用 synchronized 加锁加入节点
            synchronized (f) {
                if (tabAt(tab, i) == f) {
                    // 说明是链表
                    if (fh >= 0) {
                        binCount = 1;
                        // 循环加入新的或者覆盖节点
                        for (Node<K,V> e = f;; ++binCount) {
                            K ek;
                            if (e.hash == hash &&
                                ((ek = e.key) == key ||
                                 (ek != null && key.equals(ek)))) {
                                oldVal = e.val;
                                if (!onlyIfAbsent)
                                    e.val = value;
                                break;
                            }
                            Node<K,V> pred = e;
                            if ((e = e.next) == null) {
                                pred.next = new Node<K,V>(hash, key,
                                                          value, null);
                                break;
                            }
                        }
                    }
                    else if (f instanceof TreeBin) {
                        // 红黑树
                        Node<K,V> p;
                        binCount = 2;
                        if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                       value)) != null) {
                            oldVal = p.val;
                            if (!onlyIfAbsent)
                                p.val = value;
                        }
                    }
                }
            }
            if (binCount != 0) {
                if (binCount >= TREEIFY_THRESHOLD)
                    treeifyBin(tab, i);
                if (oldVal != null)
                    return oldVal;
                break;
            }
        }
    }
    addCount(1L, binCount);
    return null;
}
```

1. 根据 key 计算出 hashcode 。
2. 判断是否需要进行初始化。
3. 即为当前 key 定位出的 Node，如果为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功。
4. 如果当前位置的 `hashcode == MOVED == -1`,则需要进行扩容。
5. 如果都不满足，则利用 synchronized 锁写入数据。
6. 如果数量大于 `TREEIFY_THRESHOLD` 则要执行树化方法，在 `treeifyBin` 中会首先判断当前数组长度 ≥64 时才会将链表转换为红黑树。



##### get

get 流程比较简单，直接过一遍源码。

```java
public V get(Object key) {
    Node<K,V>[] tab; Node<K,V> e, p; int n, eh; K ek;
    // key 所在的 hash 位置
    int h = spread(key.hashCode());
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (e = tabAt(tab, (n - 1) & h)) != null) {
        // 如果指定位置元素存在，头结点hash值相同
        if ((eh = e.hash) == h) {
            if ((ek = e.key) == key || (ek != null && key.equals(ek)))
                // key hash 值相等，key值相同，直接返回元素 value
                return e.val;
        }
        else if (eh < 0)
            // 头结点hash值小于0，说明正在扩容或者是红黑树，find查找
            return (p = e.find(h, key)) != null ? p.val : null;
        while ((e = e.next) != null) {
            // 是链表，遍历查找
            if (e.hash == h &&
                ((ek = e.key) == key || (ek != null && key.equals(ek))))
                return e.val;
        }
    }
    return null;
}
```

总结：

1. 根据 hash 值计算位置。
2. 查找到指定位置，如果头节点就是要找的，直接返回它的 value.
3. 如果头节点 hash 值小于 0 ，说明正在扩容或者是红黑树，查找之。
4. 如果是链表，遍历查找之。



#### LinkedHashMap

##### LinkedHashMap 简介

`LinkedHashMap` 是 Java 提供的一个集合类，它继承自 `HashMap`，并在 `HashMap` 基础上维护一条双向链表，使得具备如下特性:

1. 支持遍历时会按照插入顺序有序进行迭代。
2. 支持按照元素访问顺序排序,适用于封装 LRU 缓存工具。
3. 因为内部使用双向链表维护各个节点，所以遍历时的效率和元素个数成正比，相较于和容量成正比的 HashMap 来说，迭代效率会高很多。

`LinkedHashMap` 逻辑结构如下图所示，它是在 `HashMap` 基础上在各个节点之间维护一条双向链表，使得原本散列在不同 bucket 上的节点、链表、红黑树有序关联起来。

![LinkedHashMap 逻辑结构](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172102152.png)



##### LinkedHashMap 使用示例

###### 插入顺序遍历

如下所示，我们按照顺序往 `LinkedHashMap` 添加元素然后进行遍历。

```java
HashMap < String, String > map = new LinkedHashMap < > ();
map.put("a", "2");
map.put("g", "3");
map.put("r", "1");
map.put("e", "23");

for (Map.Entry < String, String > entry: map.entrySet()) {
    System.out.println(entry.getKey() + ":" + entry.getValue());
}
```

```java
a:2
g:3
r:1
e:23
```

可以看出，`LinkedHashMap` 的迭代顺序是和插入顺序一致的,这一点是 `HashMap` 所不具备的。



###### 访问顺序遍历

`LinkedHashMap` 定义了排序模式 `accessOrder`(boolean 类型，默认为 false)，访问顺序则为 true，插入顺序则为 false。

为了实现访问顺序遍历，我们可以使用传入 `accessOrder` 属性的 `LinkedHashMap` 构造方法，并将 `accessOrder` 设置为 true，表示其具备访问有序性。

```java
LinkedHashMap<Integer, String> map = new LinkedHashMap<>(16, 0.75f, true);
map.put(1, "one");
map.put(2, "two");
map.put(3, "three");
map.put(4, "four");
map.put(5, "five");
//访问元素2,该元素会被移动至链表末端
map.get(2);
//访问元素3,该元素会被移动至链表末端
map.get(3);
for (Map.Entry<Integer, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " : " + entry.getValue());
}
```

```java
1 : one
4 : four
5 : five
2 : two
3 : three
```

可以看出，`LinkedHashMap` 的迭代顺序是和访问顺序一致的。



###### LRU 缓存

从上一个我们可以了解到通过 `LinkedHashMap` 我们可以封装一个简易版的 LRU（**L**east **R**ecently **U**sed，最近最少使用） 缓存，确保当存放的元素超过容器容量时，将最近最少访问的元素移除。

![img](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172103407.png)

具体实现思路如下：

- 继承 `LinkedHashMap`;
- 构造方法中指定 `accessOrder` 为 true ，这样在访问元素时就会把该元素移动到链表尾部，链表首元素就是最近最少被访问的元素；
- 重写`removeEldestEntry` 方法，该方法会返回一个 boolean 值，告知 `LinkedHashMap` 是否需要移除链表首元素（缓存容量有限）。

```java
public class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private final int capacity;

    public LRUCache(int capacity) {
        super(capacity, 0.75f, true);
        this.capacity = capacity;
    }

    /**
     * 判断size超过容量时返回true，告知LinkedHashMap移除最老的缓存项(即链表的第一个元素)
     */
    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > capacity;
    }
}
```

测试代码如下，笔者初始化缓存容量为 2，然后按照次序先后添加 4 个元素。

```java
LRUCache < Integer, String > cache = new LRUCache < > (2);
cache.put(1, "one");
cache.put(2, "two");
cache.put(3, "three");
for (int i = 0; i < 4; i++) {
    System.out.println(cache.get(i));
}
```

```java
null
null
three
```

从输出结果来看，由于缓存容量为 2 ，因此，添加第 3 个元素时，第 1 个元素会被删除。添加第 4 个元素时，第 2 个元素会被删除。



##### LinkedHashMap 源码解析

###### Node 的设计

在正式讨论 `LinkedHashMap` 前，我们先来聊聊 `LinkedHashMap` 节点 `Entry` 的设计,我们都知道 `HashMap` 的 bucket 上的因为冲突转为链表的节点会在符合以下两个条件时会将链表转为红黑树:

1. 链表上的节点个数达到树化的阈值-1，即`TREEIFY_THRESHOLD - 1`。
2. bucket 的容量达到最小的树化容量即`MIN_TREEIFY_CAPACITY`。

而 `LinkedHashMap` 是在 `HashMap` 的基础上为 bucket 上的每一个节点建立一条双向链表，这就使得转为红黑树的树节点也需要具备双向链表节点的特性，即每一个树节点都需要拥有两个引用存储前驱节点和后继节点的地址,所以对于树节点类 `TreeNode` 的设计就是一个比较棘手的问题。

对此我们不妨来看看两者之间节点类的类图，可以看到:

1. `LinkedHashMap` 的节点内部类 `Entry` 基于 `HashMap` 的基础上，增加 `before` 和 `after` 指针使节点具备双向链表的特性。
2. `HashMap` 的树节点 `TreeNode` 继承了具备双向链表特性的 `LinkedHashMap` 的 `Entry`。

![LinkedHashMap 和 HashMap 之间的关系](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172104581.png)LinkedHashMap 和 HashMap 之间的关系

很多读者此时就会有这样一个疑问，为什么 `HashMap` 的树节点 `TreeNode` 要通过 `LinkedHashMap` 获取双向链表的特性呢?为什么不直接在 `Node` 上实现前驱和后继指针呢?

先来回答第一个问题，我们都知道 `LinkedHashMap` 是在 `HashMap` 基础上对节点增加双向指针实现双向链表的特性,所以 `LinkedHashMap` 内部链表转红黑树时，对应的节点会转为树节点 `TreeNode`,为了保证使用 `LinkedHashMap` 时树节点具备双向链表的特性，所以树节点 `TreeNode` 需要继承 `LinkedHashMap` 的 `Entry`。

再来说说第二个问题，我们直接在 `HashMap` 的节点 `Node` 上直接实现前驱和后继指针,然后 `TreeNode` 直接继承 `Node` 获取双向链表的特性为什么不行呢？其实这样做也是可以的。只不过这种做法会使得使用 `HashMap` 时存储键值对的节点类 `Node` 多了两个没有必要的引用，占用没必要的内存空间。

所以，为了保证 `HashMap` 底层的节点类 `Node` 没有多余的引用，又要保证 `LinkedHashMap` 的节点类 `Entry` 拥有存储链表的引用，设计者就让 `LinkedHashMap` 的节点 `Entry` 去继承 Node 并增加存储前驱后继节点的引用 `before`、`after`，让需要用到链表特性的节点去实现需要的逻辑。然后树节点 `TreeNode` 再通过继承 `Entry` 获取 `before`、`after` 两个指针。

```java
static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after;
        Entry(int hash, K key, V value, Node<K,V> next) {
            super(hash, key, value, next);
        }
    }
```

但是这样做，不也使得使用 `HashMap` 时的 `TreeNode` 多了两个没有必要的引用吗?这不也是一种空间的浪费吗？

```java
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
	//略

}
```

对于这个问题,引用作者的一段注释，作者们认为在良好的 `hashCode` 算法时，`HashMap` 转红黑树的概率不大。就算转为红黑树变为树节点，也可能会因为移除或者扩容将 `TreeNode` 变为 `Node`，所以 `TreeNode` 的使用概率不算很大，对于这一点资源空间的浪费是可以接受的。

```bash
Because TreeNodes are about twice the size of regular nodes, we
use them only when bins contain enough nodes to warrant use
(see TREEIFY_THRESHOLD). And when they become too small (due to
removal or resizing) they are converted back to plain bins.  In
usages with well-distributed user hashCodes, tree bins are
rarely used.  Ideally, under random hashCodes, the frequency of
nodes in bins follows a Poisson distribution
```



###### 构造方法

`LinkedHashMap` 构造方法有 4 个实现也比较简单，直接调用父类即 `HashMap` 的构造方法完成初始化。

```java
public LinkedHashMap() {
    super();
    accessOrder = false;
}

public LinkedHashMap(int initialCapacity) {
    super(initialCapacity);
    accessOrder = false;
}

public LinkedHashMap(int initialCapacity, float loadFactor) {
    super(initialCapacity, loadFactor);
    accessOrder = false;
}

public LinkedHashMap(int initialCapacity,
    float loadFactor,
    boolean accessOrder) {
    super(initialCapacity, loadFactor);
    this.accessOrder = accessOrder;
}
```

我们上面也提到了，默认情况下 `accessOrder` 为 false，如果我们要让 `LinkedHashMap` 实现键值对按照访问顺序排序(即将最近未访问的元素排在链表首部、最近访问的元素移动到链表尾部)，需要调用第 4 个构造方法将 `accessOrder` 设置为 true。



###### get 方法

`get` 方法是 `LinkedHashMap` 增删改查操作中唯一一个重写的方法， `accessOrder` 为 true 的情况下， 它会在元素查询完成之后，将当前访问的元素移到链表的末尾。

```java
public V get(Object key) {
     Node < K, V > e;
     //获取key的键值对,若为空直接返回
     if ((e = getNode(hash(key), key)) == null)
         return null;
     //若accessOrder为true，则调用afterNodeAccess将当前元素移到链表末尾
     if (accessOrder)
         afterNodeAccess(e);
     //返回键值对的值
     return e.value;
 }
```

从源码可以看出，`get` 的执行步骤非常简单:

1. 调用父类即 `HashMap` 的 `getNode` 获取键值对，若为空则直接返回。
2. 判断 `accessOrder` 是否为 true，若为 true 则说明需要保证 `LinkedHashMap` 的链表访问有序性，执行步骤 3。
3. 调用 `LinkedHashMap` 重写的 `afterNodeAccess` 将当前元素添加到链表末尾。

关键点在于 `afterNodeAccess` 方法的实现，这个方法负责将元素移动到链表末尾。

```java
void afterNodeAccess(Node < K, V > e) { // move node to last
    LinkedHashMap.Entry < K, V > last;
    //如果accessOrder 且当前节点不未链表尾节点
    if (accessOrder && (last = tail) != e) {

        //获取当前节点、以及前驱节点和后继节点
        LinkedHashMap.Entry < K, V > p =
            (LinkedHashMap.Entry < K, V > ) e, b = p.before, a = p.after;

        //将当前节点的后继节点指针指向空，使其和后继节点断开联系
        p.after = null;

        //如果前驱节点为空，则说明当前节点是链表的首节点，故将后继节点设置为首节点
        if (b == null)
            head = a;
        else
            //如果后继节点不为空，则让前驱节点指向后继节点
            b.after = a;

        //如果后继节点不为空，则让后继节点指向前驱节点
        if (a != null)
            a.before = b;
        else
            //如果后继节点为空，则说明当前节点在链表最末尾，直接让last 指向前驱节点,这个 else其实 没有意义，因为最开头if已经确保了p不是尾结点了，自然after不会是null
            last = b;

        //如果last为空，则说明当前链表只有一个节点p，则将head指向p
        if (last == null)
            head = p;
        else {
            //反之让p的前驱指针指向尾节点，再让尾节点的前驱指针指向p
            p.before = last;
            last.after = p;
        }
        //tail指向p，自此将节点p移动到链表末尾
        tail = p;

        ++modCount;
    }
}
```

从源码可以看出， `afterNodeAccess` 方法完成了下面这些操作:

1. 如果 `accessOrder` 为 true 且链表尾部不为当前节点 p，我们则需要将当前节点移到链表尾部。
2. 获取当前节点 p、以及它的前驱节点 b 和后继节点 a。
3. 将当前节点 p 的后继指针设置为 null，使其和后继节点 p 断开联系。
4. 尝试将前驱节点指向后继节点，若前驱节点为空，则说明当前节点 p 就是链表首节点，故直接将后继节点 a 设置为首节点，随后我们再将 p 追加到 a 的末尾。
5. 再尝试让后继节点 a 指向前驱节点 b。
6. 上述操作让前驱节点和后继节点完成关联，并将当前节点 p 独立出来，这一步则是将当前节点 p 追加到链表末端，如果链表末端为空，则说明当前链表只有一个节点 p，所以直接让 head 指向 p 即可。
7. 上述操作已经将 p 成功到达链表末端，最后我们将 tail 指针即指向链表末端的指针指向 p 即可。

可以结合这张图理解，展示了 key 为 13 的元素被移动到了链表尾部。

![LinkedHashMap 移动元素 13 到链表尾部](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172105551.png)

看不太懂也没关系，知道这个方法的作用就够了，后续有时间再慢慢消化。



###### remove 方法后置操作——afterNodeRemoval

`LinkedHashMap` 并没有对 `remove` 方法进行重写，而实直接继承 `HashMap` 的 `remove` 方法，为了保证键值对移除后双向链表中的节点也会同步被移除，`LinkedHashMap` 重写了 `HashMap` 的空实现方法 `afterNodeRemoval`。

```java
final Node<K,V> removeNode(int hash, Object key, Object value,
                               boolean matchValue, boolean movable) {
        //略
            if (node != null && (!matchValue || (v = node.value) == value ||
                                 (value != null && value.equals(v)))) {
                if (node instanceof TreeNode)
                    ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
                else if (node == p)
                    tab[index] = node.next;
                else
                    p.next = node.next;
                ++modCount;
                --size;
                //HashMap的removeNode完成元素移除后会调用afterNodeRemoval进行移除后置操作
                afterNodeRemoval(node);
                return node;
            }
        }
        return null;
    }
//空实现
void afterNodeRemoval(Node<K,V> p) { }
```

我们可以看到从 `HashMap` 继承来的 `remove` 方法内部调用的 `removeNode` 方法将节点从 bucket 删除后，调用了 `afterNodeRemoval`。

```java
void afterNodeRemoval(Node<K,V> e) { // unlink

		//获取当前节点p、以及e的前驱节点b和后继节点a
        LinkedHashMap.Entry<K,V> p =
            (LinkedHashMap.Entry<K,V>)e, b = p.before, a = p.after;
		//将p的前驱和后继指针都设置为null，使其和前驱、后继节点断开联系
        p.before = p.after = null;

		//如果前驱节点为空，则说明当前节点p是链表首节点，让head指针指向后继节点a即可
        if (b == null)
            head = a;
        else
        //如果前驱节点b不为空，则让b直接指向后继节点a
            b.after = a;

		//如果后继节点为空，则说明当前节点p在链表末端，所以直接让tail指针指向前驱节点a即可
        if (a == null)
            tail = b;
        else
        //反之后继节点的前驱指针直接指向前驱节点
            a.before = b;
    }
```

从源码可以看出， `afterNodeRemoval` 方法的整体操作就是让当前节点 p 和前驱节点、后继节点断开联系，等待 gc 回收，整体步骤为:

1. 获取当前节点 p、以及 e 的前驱节点 b 和后继节点 a。
2. 让当前节点 p 和其前驱、后继节点断开联系。
3. 尝试让前驱节点 b 指向后继节点 a，若 b 为空则说明当前节点 p 在链表首部，我们直接将 head 指向后继节点 a 即可。
4. 尝试让后继节点 a 指向前驱节点 b，若 a 为空则说明当前节点 p 在链表末端，所以直接让 tail 指针指向前驱节点 a 即可。

可以结合这张图理解，展示了 key 为 13 的元素被删除，也就是从链表中移除了这个元素。

![image-20230717222955690](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172229060.png)



###### put 方法后置操作——afterNodeInsertion

同样的 `LinkedHashMap` 并没有实现插入方法，而是直接继承 `HashMap` 的所有插入方法交由用户使用，但为了维护双向链表访问的有序性，它做了这样两件事:

1. 重写 `afterNodeAccess`(上文提到过),如果当前被插入的 key 已存在与 `map` 中，因为 `LinkedHashMap` 的插入操作会将新节点追加至链表末尾，所以对于存在的 key 则调用 `afterNodeAccess` 将其放到链表末端。
2. 重写了 `HashMap` 的 `afterNodeInsertion` 方法，当 `removeEldestEntry` 返回 true 时，会将链表首节点移除。

这一点我们可以在 `HashMap` 的插入操作核心方法 `putVal` 中看到。

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        	//略
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                 //如果当前的key在map中存在，则调用afterNodeAccess
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
         //调用插入后置方法，该方法被LinkedHashMap重写
        afterNodeInsertion(evict);
        return null;
    }
```

上述步骤的源码上文已经解释过了，所以这里我们着重了解一下 `afterNodeInsertion` 的工作流程，假设我们的重写了 `removeEldestEntry`，当链表 `size` 超过 `capacity` 时，就返回 true。

```java
/**
 * 判断size超过容量时返回true，告知LinkedHashMap移除最老的缓存项(即链表的第一个元素)
 */
protected boolean removeEldestEntry(Map.Entry < K, V > eldest) {
    return size() > capacity;
}
```

以下图为例，假设笔者最后新插入了一个不存在的节点 19,假设 `capacity` 为 4，所以 `removeEldestEntry` 返回 true，我们要将链表首节点移除。

![LinkedHashMap 中插入新元素 19](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172107237.png)

移除的步骤很简单，查看链表首节点是否存在，若存在则断开首节点和后继节点的关系，并让首节点指针指向下一节点，所以 head 指针指向了 12，节点 10 成为没有任何引用指向的空对象，等待 GC。

![image-20230717211007155](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172110378.png)

```java
void afterNodeInsertion(boolean evict) { // possibly remove eldest
        LinkedHashMap.Entry<K,V> first;
        //如果evict为true且队首元素不为空以及removeEldestEntry返回true，则说明我们需要最老的元素(即在链表首部的元素)移除。
        if (evict && (first = head) != null && removeEldestEntry(first)) {
        	//获取链表首部的键值对的key
            K key = first.key;
            //调用removeNode将元素从HashMap的bucket中移除，并和LinkedHashMap的双向链表断开，等待gc回收
            removeNode(hash(key), key, null, false, true);
        }
    }
```

从源码可以看出， `afterNodeInsertion` 方法完成了下面这些操作:

1. 判断 `eldest` 是否为 true，只有为 true 才能说明可能需要将最年长的键值对(即链表首部的元素)进行移除，具体是否具体要进行移除，还得确定链表是否为空`((first = head) != null)`，以及 `removeEldestEntry` 方法是否返回 true，只有这两个方法返回 true 才能确定当前链表不为空，且链表需要进行移除操作了。
2. 获取链表第一个元素的 key。
3. 调用 `HashMap` 的 `removeNode` 方法，该方法我们上文提到过，它会将节点从 `HashMap` 的 bucket 中移除，并且 `LinkedHashMap` 还重写了 `removeNode` 中的 `afterNodeRemoval` 方法，所以这一步将通过调用 `removeNode` 将元素从 `HashMap` 的 bucket 中移除，并和 `LinkedHashMap` 的双向链表断开，等待 gc 回收。



##### LinkedHashMap 和 HashMap 遍历性能比较

`LinkedHashMap` 维护了一个双向链表来记录数据插入的顺序，因此在迭代遍历生成的迭代器的时候，是按照双向链表的路径进行遍历的。这一点相比于 `HashMap` 那种遍历整个 bucket 的方式来说，高效需多。

这一点我们可以从两者的迭代器中得以印证，先来看看 `HashMap` 的迭代器，可以看到 `HashMap` 迭代键值对时会用到一个 `nextNode` 方法，该方法会返回 next 指向的下一个元素，并会从 next 开始遍历 bucket 找到下一个 bucket 中不为空的元素 Node。

```java
 final class EntryIterator extends HashIterator
 implements Iterator < Map.Entry < K, V >> {
     public final Map.Entry < K,
     V > next() {
         return nextNode();
     }
 }

 //获取下一个Node
 final Node < K, V > nextNode() {
     Node < K, V > [] t;
     //获取下一个元素next
     Node < K, V > e = next;
     if (modCount != expectedModCount)
         throw new ConcurrentModificationException();
     if (e == null)
         throw new NoSuchElementException();
     //将next指向bucket中下一个不为空的Node
     if ((next = (current = e).next) == null && (t = table) != null) {
         do {} while (index < t.length && (next = t[index++]) == null);
     }
     return e;
 }
```

相比之下 `LinkedHashMap` 的迭代器则是直接使用通过 `after` 指针快速定位到当前节点的后继节点，简洁高效需多。

```java
 final class LinkedEntryIterator extends LinkedHashIterator
 implements Iterator < Map.Entry < K, V >> {
     public final Map.Entry < K,
     V > next() {
         return nextNode();
     }
 }
 //获取下一个Node
 final LinkedHashMap.Entry < K, V > nextNode() {
     //获取下一个节点next
     LinkedHashMap.Entry < K, V > e = next;
     if (modCount != expectedModCount)
         throw new ConcurrentModificationException();
     if (e == null)
         throw new NoSuchElementException();
     //current 指针指向当前节点
     current = e;
     //next直接当前节点的after指针快速定位到下一个节点
     next = e.after;
     return e;
 }
```

压测测试插入 1000w 和迭代 1000w 条数据的耗时，代码如下:

```java
int count = 1000_0000;
Map<Integer, Integer> hashMap = new HashMap<>();
Map<Integer, Integer> linkedHashMap = new LinkedHashMap<>();

long start, end;

start = System.currentTimeMillis();
for (int i = 0; i < count; i++) {
    hashMap.put(ThreadLocalRandom.current().nextInt(1, count), ThreadLocalRandom.current().nextInt(0, count));
}
end = System.currentTimeMillis();
System.out.println("map time putVal: " + (end - start));

start = System.currentTimeMillis();
for (int i = 0; i < count; i++) {
    linkedHashMap.put(ThreadLocalRandom.current().nextInt(1, count), ThreadLocalRandom.current().nextInt(0, count));
}
end = System.currentTimeMillis();
System.out.println("linkedHashMap putVal time: " + (end - start));

start = System.currentTimeMillis();
long num = 0;
for (Integer v : hashMap.values()) {
    num = num + v;
}
end = System.currentTimeMillis();
System.out.println("map get time: " + (end - start));

start = System.currentTimeMillis();
for (Integer v : linkedHashMap.values()) {
    num = num + v;
}
end = System.currentTimeMillis();
System.out.println("linkedHashMap get time: " + (end - start));
System.out.println(num);
```

从输出结果来看，因为 `LinkedHashMap` 需要维护双向链表的缘故，插入元素相较于 `HashMap` 会更耗时，但是有了双向链表明确的前后节点关系，迭代效率相对于前者高效了需多。不过，总体来说却别不大，毕竟数据量这么庞大。

```bash
map time putVal: 5880
linkedHashMap putVal time: 7567
map get time: 143
linkedHashMap get time: 67
63208969074998
```



##### LinkedHashMap 常见面试题

###### 什么是 LinkedHashMap？

`LinkedHashMap` 是 Java 集合框架中 `HashMap` 的一个子类，它继承了 `HashMap` 的所有属性和方法，并且在 `HashMap` 的基础重写了 `afterNodeRemoval`、`afterNodeInsertion`、`afterNodeAccess` 方法。使之拥有顺序插入和访问有序的特性。



###### LinkedHashMap 如何按照插入顺序迭代元素？

`LinkedHashMap` 按照插入顺序迭代元素是它的默认行为。`LinkedHashMap` 内部维护了一个双向链表，用于记录元素的插入顺序。因此，当使用迭代器迭代元素时，元素的顺序与它们最初插入的顺序相同。



###### LinkedHashMap 如何按照访问顺序迭代元素？

`LinkedHashMap` 可以通过构造函数中的 `accessOrder` 参数指定按照访问顺序迭代元素。当 `accessOrder` 为 true 时，每次访问一个元素时，该元素会被移动到链表的末尾，因此下次访问该元素时，它就会成为链表中的最后一个元素，从而实现按照访问顺序迭代元素。



###### LinkedHashMap 如何实现 LRU 缓存？

将 `accessOrder` 设置为 true 并重写 `removeEldestEntry` 方法当链表大小超过容量时返回 true，使得每次访问一个元素时，该元素会被移动到链表的末尾。一旦插入操作让 `removeEldestEntry` 返回 true 时，视为缓存已满，`LinkedHashMap` 就会将链表首元素移除，由此我们就能实现一个 LRU 缓存。



###### LinkedHashMap 和 HashMap 有什么区别？

`LinkedHashMap` 和 `HashMap` 都是 Java 集合框架中的 Map 接口的实现类。它们的最大区别在于迭代元素的顺序。`HashMap` 迭代元素的顺序是不确定的，而 `LinkedHashMap` 提供了按照插入顺序或访问顺序迭代元素的功能。此外，`LinkedHashMap` 内部维护了一个双向链表，用于记录元素的插入顺序或访问顺序，而 `HashMap` 则没有这个链表。因此，`LinkedHashMap` 的插入性能可能会比 `HashMap` 略低，但它提供了更多的功能并且迭代效率相较于 `HashMap` 更加高效。





#### CopyOnWriteArrayList

##### CopyOnWriteArrayList简介

在 JDK1.5 之前，如果想要使用并发安全的 `List` 只能选择 `Vector`。而 `Vector` 是一种老旧的集合，已经被淘汰。`Vector` 对于增删改查等方法基本都加了 `synchronized`，这种方式虽然能够保证同步，但这相当于对整个 `Vector` 加上了一把大锁，使得每个方法执行的时候都要去获得锁，导致性能非常低下。

JDK1.5 引入了 `Java.util.concurrent`（JUC）包，其中提供了很多线程安全且并发性能良好的容器，其中唯一的线程安全 `List` 实现就是 `CopyOnWriteArrayList` 。



###### CopyOnWriteArrayList 的优点？

对于大部分业务场景来说，读取操作往往是远大于写入操作的。由于读取操作不会对原有数据进行修改，因此，对于每次读取都进行加锁其实是一种资源浪费。相比之下，我们应该允许多个线程同时访问 `List` 的内部数据，毕竟对于读取操作来说是安全的。

这种思路与 `ReentrantReadWriteLock` 读写锁的设计思想非常类似，即读读不互斥、读写互斥、写写互斥（只有读读不互斥）。`CopyOnWriteArrayList` 更进一步地实现了这一思想。为了将读操作性能发挥到极致，`CopyOnWriteArrayList` 中的读取操作是完全无需加锁的。更加厉害的是，写入操作也不会阻塞读取操作，只有写写才会互斥。这样一来，读操作的性能就可以大幅度提升。

`CopyOnWriteArrayList` 线程安全的核心在于其采用了 **写时复制（Copy-On-Write）** 的策略，从 `CopyOnWriteArrayList` 的名字就能看出了。



###### Copy-On-Write 的思想是什么？

`CopyOnWriteArrayList`名字中的“Copy-On-Write”即写时复制，简称 COW。

下面是维基百科对 Copy-On-Write 的介绍，介绍的挺不错：

> 写入时复制（英语：Copy-on-write，简称 COW）是一种计算机程序设计领域的优化策略。其核心思想是，如果有多个调用者（callers）同时请求相同资源（如内存或磁盘上的数据存储），他们会共同获取相同的指针指向相同的资源，直到某个调用者试图修改资源的内容时，系统才会真正复制一份专用副本（private copy）给该调用者，而其他调用者所见到的最初的资源仍然保持不变。这过程对其他的调用者都是透明的。此作法主要的优点是如果调用者没有修改该资源，就不会有副本（private copy）被创建，因此多个调用者只是读取操作时可以共享同一份资源。

这里再以 `CopyOnWriteArrayList`为例介绍：当需要修改（ `add`，`set`、`remove` 等操作） `CopyOnWriteArrayList` 的内容时，不会直接修改原数组，而是会先创建底层数组的副本，对副本数组进行修改，修改完之后再将修改后的数组赋值回去，这样就可以保证写操作不会影响读操作了。

可以看出，写时复制机制非常适合读多写少的并发场景，能够极大地提高系统的并发性能。

不过，写时复制机制并不是银弹，其依然存在一些缺点，下面列举几点：

1. 内存占用：每次写操作都需要复制一份原始数据，会占用额外的内存空间，在数据量比较大的情况下，可能会导致内存资源不足。
2. 写操作开销：每一次写操作都需要复制一份原始数据，然后再进行修改和替换，所以写操作的开销相对较大，在写入比较频繁的场景下，性能可能会受到影响。
3. 数据一致性问题：修改操作不会立即反映到最终结果中，还需要等待复制完成，这可能会导致一定的数据一致性问题。





##### CopyOnWriteArrayList 源码分析

这里以 JDK1.8 为例，分析一下 `CopyOnWriteArrayList` 的底层核心源码。

`CopyOnWriteArrayList` 的类定义如下：

```java
public class CopyOnWriteArrayList<E>
extends Object
implements List<E>, RandomAccess, Cloneable, Serializable
{
  //...
}
```

`CopyOnWriteArrayList` 实现了以下接口：

- `List` : 表明它是一个列表，支持添加、删除、查找等操作，并且可以通过下标进行访问。
- `RandomAccess` ：这是一个标志接口，表明实现这个接口的 `List` 集合是支持 **快速随机访问** 的。
- `Cloneable` ：表明它具有拷贝能力，可以进行深拷贝或浅拷贝操作。
- `Serializable` : 表明它可以进行序列化操作，也就是可以将对象转换为字节流进行持久化存储或网络传输，非常方便。

![CopyOnWriteArrayList 类图](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172112991.png)



###### 初始化

`CopyOnWriteArrayList` 中有一个无参构造函数和两个有参构造函数。

```java
// 创建一个空的 CopyOnWriteArrayList
public CopyOnWriteArrayList() {
    setArray(new Object[0]);
}

// 按照集合的迭代器返回的顺序创建一个包含指定集合元素的 CopyOnWriteArrayList
public CopyOnWriteArrayList(Collection<? extends E> c) {
    Object[] elements;
    if (c.getClass() == CopyOnWriteArrayList.class)
        elements = ((CopyOnWriteArrayList<?>)c).getArray();
    else {
        elements = c.toArray();
        // c.toArray might (incorrectly) not return Object[] (see 6260652)
        if (elements.getClass() != Object[].class)
            elements = Arrays.copyOf(elements, elements.length, Object[].class);
    }
    setArray(elements);
}

// 创建一个包含指定数组的副本的列表
public CopyOnWriteArrayList(E[] toCopyIn) {
    setArray(Arrays.copyOf(toCopyIn, toCopyIn.length, Object[].class));
}
```



###### 插入元素

`CopyOnWriteArrayList` 的 `add()`方法有三个版本：

- `add(E e)`：在 `CopyOnWriteArrayList` 的尾部插入元素。
- `add(int index, E element)`：在 `CopyOnWriteArrayList` 的指定位置插入元素。
- `addIfAbsent(E e)`：如果指定元素不存在，那么添加该元素。如果成功添加元素则返回 true。

这里以`add(E e)`为例进行介绍：

```java
// 插入元素到 CopyOnWriteArrayList 的尾部
public boolean add(E e) {
    final ReentrantLock lock = this.lock;
    // 加锁
    lock.lock();
    try {
        // 获取原来的数组
        Object[] elements = getArray();
        // 原来数组的长度
        int len = elements.length;
        // 创建一个长度+1的新数组，并将原来数组的元素复制给新数组
        Object[] newElements = Arrays.copyOf(elements, len + 1);
        // 元素放在新数组末尾
        newElements[len] = e;
        // array指向新数组
        setArray(newElements);
        return true;
    } finally {
        // 解锁
        lock.unlock();
    }
}
```

从上面的源码可以看出：

- `add`方法内部用到了 `ReentrantLock` 加锁，保证了同步，避免了多线程写的时候会复制出多个副本出来。锁被修饰保证了锁的内存地址肯定不会被修改，并且，释放锁的逻辑放在 `finally` 中，可以保证锁能被释放。
- `CopyOnWriteArrayList` 通过复制底层数组的方式实现写操作，即先创建一个新的数组来容纳新添加的元素，然后在新数组中进行写操作，最后将新数组赋值给底层数组的引用，替换掉旧的数组。这也就证明了我们前面说的：`CopyOnWriteArrayList` 线程安全的核心在于其采用了 **写时复制（Copy-On-Write）** 的策略。
- 每次写操作都需要通过 `Arrays.copyOf` 复制底层数组，时间复杂度是 O(n) 的，且会占用额外的内存空间。因此，`CopyOnWriteArrayList` 适用于读多写少的场景，在写操作不频繁且内存资源充足的情况下，可以提升系统的性能表现。
- `CopyOnWriteArrayList` 中并没有类似于 `ArrayList` 的 `grow()` 方法扩容的操作。

> `Arrays.copyOf` 方法的时间复杂度是 O(n)，其中 n 表示需要复制的数组长度。因为这个方法的实现原理是先创建一个新的数组，然后将源数组中的数据复制到新数组中，最后返回新数组。这个方法会复制整个数组，因此其时间复杂度与数组长度成正比，即 O(n)。值得注意的是，由于底层调用了系统级别的拷贝指令，因此在实际应用中这个方法的性能表现比较优秀，但是也需要注意控制复制的数据量，避免出现内存占用过高的情况。



###### 读取元素

`CopyOnWriteArrayList` 的读取操作是基于内部数组 `array` 并没有发生实际的修改，因此在读取操作时不需要进行同步控制和锁操作，可以保证数据的安全性。这种机制下，多个线程可以同时读取列表中的元素。

```java
// 底层数组，只能通过getArray和setArray方法访问
private transient volatile Object[] array;

public E get(int index) {
    return get(getArray(), index);
}

final Object[] getArray() {
    return array;
}

private E get(Object[] a, int index) {
    return (E) a[index];
}
```

不过，`get`方法是弱一致性的，在某些情况下可能读到旧的元素值。

`get(int index)`方法是分两步进行的：

1. 通过`getArray()`获取当前数组的引用；
2. 直接从数组中获取下标为 index 的元素。

这个过程并没有加锁，所以在并发环境下可能出现如下情况：

1. 线程 1 调用`get(int index)`方法获取值，内部通过`getArray()`方法获取到了 array 属性值；
2. 线程 2 调用`CopyOnWriteArrayList`的`add`、`set`、`remove` 等修改方法时，内部通过`setArray`方法修改了`array`属性的值；
3. 线程 1 还是从旧的 `array` 数组中取值。



###### 获取列表中元素的个数

```java
public int size() {
    return getArray().length;
}
```

`CopyOnWriteArrayList`中的`array`数组每次复制都刚好能够容纳下所有元素，并不像`ArrayList`那样会预留一定的空间。因此，`CopyOnWriteArrayList`中并没有`size`属性`CopyOnWriteArrayList`的底层数组的长度就是元素个数，因此`size()`方法只要返回数组长度就可以了。



###### 删除元素

`CopyOnWriteArrayList`删除元素相关的方法一共有 4 个：

1. `remove(int index)`：移除此列表中指定位置上的元素。将任何后续元素向左移动（从它们的索引中减去 1）。
2. `boolean remove(Object o)`：删除此列表中首次出现的指定元素，如果不存在该元素则返回 false。
3. `boolean removeAll(Collection<?> c)`：从此列表中删除指定集合中包含的所有元素。
4. `void clear()`：移除此列表中的所有元素。

这里以`remove(int index)`为例进行介绍：

```java
public E remove(int index) {
    // 获取可重入锁
    final ReentrantLock lock = this.lock;
    // 加锁
    lock.lock();
    try {
    	   //获取当前array数组
        Object[] elements = getArray();
        // 获取当前array长度
        int len = elements.length;
        //获取指定索引的元素(旧值)
        E oldValue = get(elements, index);
        int numMoved = len - index - 1;
        // 判断删除的是否是最后一个元素
        if (numMoved == 0)
        	   // 如果删除的是最后一个元素，直接复制该元素前的所有元素到新的数组
            setArray(Arrays.copyOf(elements, len - 1));
        else {
            // 分段复制，将index前的元素和index+1后的元素复制到新数组
            // 新数组长度为旧数组长度-1
            Object[] newElements = new Object[len - 1];
            System.arraycopy(elements, 0, newElements, 0, index);
            System.arraycopy(elements, index + 1, newElements, index,
                             numMoved);
            //将新数组赋值给array引用
            setArray(newElements);
        }
        return oldValue;
    } finally {
       	// 解锁
        lock.unlock();
    }
}
```



###### 判断元素是否存在

`CopyOnWriteArrayList`提供了两个用于判断指定元素是否在列表中的方法：

- `contains(Object o)`：判断是否包含指定元素。
- `containsAll(Collection<?> c)`：判断是否保证指定集合的全部元素。

```java
// 判断是否包含指定元素
public boolean contains(Object o) {
    //获取当前array数组
    Object[] elements = getArray();
    //调用index尝试查找指定元素，如果返回值大于等于0，则返回true，否则返回false
    return indexOf(o, elements, 0, elements.length) >= 0;
}

// 判断是否保证指定集合的全部元素
public boolean containsAll(Collection<?> c) {
    //获取当前array数组
    Object[] elements = getArray();
    //获取数组长度
    int len = elements.length;
    //遍历指定集合
    for (Object e : c) {
        //循环调用indexOf方法判断，只要有一个没有包含就直接返回false
        if (indexOf(e, elements, 0, len) < 0)
            return false;
    }
    //最后表示全部包含或者制定集合为空集合，那么返回true
    return true;
}
```



##### 常用方法测试

```java
// 创建一个 CopyOnWriteArrayList 对象
CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();

// 向列表中添加元素
list.add("Java");
list.add("Python");
list.add("C++");
System.out.println("初始列表：" + list);

// 使用 get 方法获取指定位置的元素
System.out.println("列表第二个元素为：" + list.get(1));

// 使用 remove 方法删除指定元素
boolean result = list.remove("C++");
System.out.println("删除结果：" + result);
System.out.println("列表删除元素后为：" + list);

// 使用 set 方法更新指定位置的元素
list.set(1, "Golang");
System.out.println("列表更新后为：" + list);

// 使用 add 方法在指定位置插入元素
list.add(0, "PHP");
System.out.println("列表插入元素后为：" + list);

// 使用 size 方法获取列表大小
System.out.println("列表大小为：" + list.size());

// 使用 removeAll 方法删除指定集合中所有出现的元素
result = list.removeAll(List.of("Java", "Golang"));
System.out.println("批量删除结果：" + result);
System.out.println("列表批量删除元素后为：" + list);

// 使用 clear 方法清空列表中所有元素
list.clear();
System.out.println("列表清空后为：" + list);
```

```text
列表更新后为：[Java, Golang]
列表插入元素后为：[PHP, Java, Golang]
列表大小为：3
批量删除结果：true
列表批量删除元素后为：[PHP]
列表清空后为：[]
```



#### ArrayBlockingQueue

##### 阻塞队列简介

###### 阻塞队列的历史

Java 阻塞队列的历史可以追溯到 JDK1.5 版本，当时 Java 平台增加了 `java.util.concurrent`，即我们常说的 JUC 包，其中包含了各种并发流程控制工具、并发容器、原子类等。这其中自然也包含了我们这篇文章所讨论的阻塞队列。

为了解决高并发场景下多线程之间数据共享的问题，JDK1.5 版本中出现了 `ArrayBlockingQueue` 和 `LinkedBlockingQueue`，它们是带有生产者-消费者模式实现的并发容器。其中，`ArrayBlockingQueue` 是有界队列，即添加的元素达到上限之后，再次添加就会被阻塞或者抛出异常。而 `LinkedBlockingQueue` 则由链表构成的队列，正是因为链表的特性，所以 `LinkedBlockingQueue` 在添加元素上并不会向 `ArrayBlockingQueue` 那样有着较多的约束，所以 `LinkedBlockingQueue` 设置队列是否有界是可选的(注意这里的无界并不是指可以添加任务数量的元素，而是说队列的大小默认为 `Integer.MAX_VALUE`，近乎于无限大)。

随着 Java 的不断发展，JDK 后续的几个版本又对阻塞队列进行了不少的更新和完善:

1. JDK1.6 版本:增加 `SynchronousQueue`，一个不存储元素的阻塞队列。
2. JDK1.7 版本:增加 `TransferQueue`，一个支持更多操作的阻塞队列。
3. JDK1.8 版本:增加 `DelayQueue`，一个支持延迟获取元素的阻塞队列。



###### 阻塞队列的思想

阻塞队列就是典型的生产者-消费者模型，它可以做到以下几点:

1. 当阻塞队列数据为空时，所有的消费者线程都会被阻塞，等待队列非空。
2. 当生产者往队列里填充数据后，队列就会通知消费者队列非空，消费者此时就可以进来消费。
3. 当阻塞队列因为消费者消费过慢或者生产者存放元素过快导致队列填满时无法容纳新元素时，生产者就会被阻塞，等待队列非满时继续存放元素。
4. 当消费者从队列中消费一个元素之后，队列就会通知生产者队列非满，生产者可以继续填充数据了。

总结一下：阻塞队列就说基于非空和非满两个条件实现生产者和消费者之间的交互，尽管这些交互流程和等待通知的机制实现非常复杂，好在 Doug Lea 的操刀之下已将阻塞队列的细节屏蔽，我们只需调用 `put`、`take`、`offfer`、`poll` 等 API 即可实现多线程之间的生产和消费。

这也使得阻塞队列在多线程开发中有着广泛的运用，最常见的例子无非是我们的线程池,从源码中我们就能看出当核心线程无法及时处理任务时，这些任务都会扔到 `workQueue` 中。

```java
public ThreadPoolExecutor(int corePoolSize,
                            int maximumPoolSize,
                            long keepAliveTime,
                            TimeUnit unit,
                            BlockingQueue<Runnable> workQueue,
                            ThreadFactory threadFactory,
                            RejectedExecutionHandler handler) {// ...}
```



##### ArrayBlockingQueue 常见方法及测试

简单了解了阻塞队列的历史之后，我们就开始重点讨论本篇文章所要介绍的并发容器——`ArrayBlockingQueue`。为了后续更加深入的了解 `ArrayBlockingQueue`，我们不妨基于下面几个实例了解以下 `ArrayBlockingQueue` 的使用。

先看看第一个例子，我们这里会用两个线程分别模拟生产者和消费者，生产者生产完会使用 `put` 方法生产 10 个元素给消费者进行消费，当队列元素达到我们设置的上限 5 时，`put` 方法就会阻塞。 同理消费者也会通过 `take` 方法消费元素，当队列为空时，`take` 方法就会阻塞消费者线程。这里笔者为了保证消费者能够在消费完 10 个元素后及时退出。便通过倒计时门闩，来控制消费者结束，生产者在这里只会生产 10 个元素。当消费者将 10 个元素消费完成之后，按下倒计时门闩，所有线程都会停止。

```java
public class ProducerConsumerExample {

    public static void main(String[] args) throws InterruptedException {

        // 创建一个大小为 5 的 ArrayBlockingQueue
        ArrayBlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);

        // 创建生产者线程
        Thread producer = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    // 向队列中添加元素，如果队列已满则阻塞等待
                    queue.put(i);
                    System.out.println("生产者添加元素：" + i);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        });

        CountDownLatch countDownLatch = new CountDownLatch(1);

        // 创建消费者线程
        Thread consumer = new Thread(() -> {
            try {
                int count = 0;
                while (true) {

                    // 从队列中取出元素，如果队列为空则阻塞等待
                    int element = queue.take();
                    System.out.println("消费者取出元素：" + element);
                    ++count;
                    if (count == 10) {
                        break;
                    }
                }

                countDownLatch.countDown();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        });

        // 启动线程
        producer.start();
        consumer.start();

        // 等待线程结束
        producer.join();
        consumer.join();

        countDownLatch.await();

        producer.interrupt();
        consumer.interrupt();
    }

}
```

代码输出结果如下，可以看到只有生产者往队列中投放元素之后消费者才能消费，这也就意味着当队列中没有数据的时消费者就会阻塞，等待队列非空再继续消费。

```cpp
生产者添加元素：1
生产者添加元素：2
消费者取出元素：1
消费者取出元素：2
消费者取出元素：3
生产者添加元素：3
生产者添加元素：4
生产者添加元素：5
消费者取出元素：4
生产者添加元素：6
消费者取出元素：5
生产者添加元素：7
生产者添加元素：8
生产者添加元素：9
生产者添加元素：10
消费者取出元素：6
消费者取出元素：7
消费者取出元素：8
消费者取出元素：9
消费者取出元素：10
```

了解了 `put`、`take` 这两个会阻塞的存和取方法之后，我我们再来看看阻塞队列中非阻塞的入队和出队方法 `offer` 和 `poll`。

如下所示，我们设置了一个大小为 3 的阻塞队列，我们会尝试在队列用 offer 方法存放 4 个元素，然后再从队列中用 `poll` 尝试取 4 次。

```cpp
public class OfferPollExample {

    public static void main(String[] args) {
        // 创建一个大小为 3 的 ArrayBlockingQueue
        ArrayBlockingQueue<String> queue = new ArrayBlockingQueue<>(3);

        // 向队列中添加元素
        System.out.println(queue.offer("A"));
        System.out.println(queue.offer("B"));
        System.out.println(queue.offer("C"));

        // 尝试向队列中添加元素，但队列已满，返回 false
        System.out.println(queue.offer("D"));

        // 从队列中取出元素
        System.out.println(queue.poll());
        System.out.println(queue.poll());
        System.out.println(queue.poll());

        // 尝试从队列中取出元素，但队列已空，返回 null
        System.out.println(queue.poll());
    }

}
```

最终代码的输出结果如下，可以看到因为队列的大小为 3 的缘故，我们前 3 次存放到队列的结果为 true，第 4 次存放时，由于队列已满，所以存放结果返回 false。这也是为什么我们后续的 `poll` 方法只得到了 3 个元素的值。

```cpp
true
true
true
false
A
B
C
null
```

了解了阻塞存取和非阻塞存取，我们再来看看阻塞队列的一个比较特殊的操作，某些场景下，我们希望能够一次性将阻塞队列的结果存到列表中再进行批量操作，我们就可以使用阻塞队列的 `drainTo` 方法，这个方法会一次性将队列中所有元素存放到列表，如果队列中有元素，且成功存到 list 中则 `drainTo` 会返回本次转移到 list 中的元素数，反之若队列为空，`drainTo` 则直接返回 0。

```java
public class DrainToExample {

    public static void main(String[] args) {
        // 创建一个大小为 5 的 ArrayBlockingQueue
        ArrayBlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);

        // 向队列中添加元素
        queue.add(1);
        queue.add(2);
        queue.add(3);
        queue.add(4);
        queue.add(5);

        // 创建一个 List，用于存储从队列中取出的元素
        List<Integer> list = new ArrayList<>();

        // 从队列中取出所有元素，并添加到 List 中
        queue.drainTo(list);

        // 输出 List 中的元素
        System.out.println(list);
    }

}
```

代码输出结果如下

```cpp
[1, 2, 3, 4, 5]
```

##### ArrayBlockingQueue 源码分析

自此我们对阻塞队列的使用有了基本的印象，接下来我们就可以进一步了解一下 `ArrayBlockingQueue` 的工作机制了。



###### 整体设计

在了解 `ArrayBlockingQueue` 的具体细节之前，我们先来看看 `ArrayBlockingQueue` 的类图

![image-20230717212746440](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172127695.png)

从图中我们可以看出，`ArrayBlockingQueue` 继承了阻塞队列 `BlockingQueue` 这个接口，不难猜出通过继承 `BlockingQueue` 这个接口之后，`ArrayBlockingQueue` 就拥有了阻塞队列那些常见的操作行为。

同时， `ArrayBlockingQueue` 还继承了 `AbstractQueue` 这个抽象类，这个继承了 `AbstractCollection` 和 `Queue` 的抽象类，从抽象类的特定和语义我们也可以猜出，这个继承关系使得 `ArrayBlockingQueue` 拥有了队列的常见操作。

所以我们是否可以得出这样一个结论，通过继承 `AbstractQueue` 获得队列所有的操作模板，其实现的入队和出队操作的整体框架。然后 `ArrayBlockingQueue` 通过继承 `BlockingQueue` 获取到阻塞队列的常见操作并将这些操作实现，填充到 `AbstractQueue` 模板方法的细节中，由此 `ArrayBlockingQueue` 成为一个完整的阻塞队列。

为了印证这一点，我们到源码中一探究竟。首先我们先来看看 `AbstractQueue`，从类的继承关系我们可以大致得出，它通过 `AbstractCollection` 获得了集合的常见操作方法，然后通过 `Queue` 接口获得了队列的特性。

```java
public abstract class AbstractQueue<E>
    extends AbstractCollection<E>
    implements Queue<E> {
   		//...
}
```

对于集合的操作无非是增删改查，所以我们不妨从添加方法入手，从源码中我们可以看到，它实现了 `AbstractCollection` 的 `add` 方法，其内部逻辑如下:

1. 调用继承 `Queue` 接口的来的 `offer` 方法，如果 `offer` 成功则返回 `true`。
2. 如果 `offer` 失败，即代表当前元素入队失败直接抛异常。

```java
public boolean add(E e) {
        if (offer(e))
            return true;
        else
            throw new IllegalStateException("Queue full");
    }
```

而 `AbstractQueue` 中并没有对 `Queue` 的 `offer` 的实现，很明显这样做的目的是定义好了 `add` 的核心逻辑，将 `offer` 的细节交由其子类即我们的 `ArrayBlockingQueue` 实现。

到此，我们对于抽象类 `AbstractQueue` 的分析就结束了，我们继续看看 `ArrayBlockingQueue` 中另一个重要的继承接口 `BlockingQueue`。

点开 `BlockingQueue` 之后，我们可以看到这个接口同样继承了 `Queue` 接口，这就意味着它也具备了队列所拥有的所有行为。同时，它还定义了自己所需要实现的方法。

```java
public interface BlockingQueue<E> extends Queue<E> {

   	//元素入队成功返回true，反之则会抛出异常IllegalStateException
    boolean add(E e);

   	//元素入队成功返回true，反之返回false
    boolean offer(E e);

   	//元素入队成功则直接返回，如果队列已满元素不可入队则将线程阻塞，因为阻塞期间可能会被打断，所以这里方法签名抛出了InterruptedException
    void put(E e) throws InterruptedException;

   //和上一个方法一样,只不过队列满时只会阻塞单位为unit，时间为timeout的时长，如果在等待时长内没有入队成功则直接返回false。
    boolean offer(E e, long timeout, TimeUnit unit)
        throws InterruptedException;

    //从队头取出一个元素，如果队列为空则阻塞等待，因为会阻塞线程的缘故，所以该方法可能会被打断，所以签名定义了InterruptedException
    E take() throws InterruptedException;

 	   //取出队头的元素并返回，如果当前队列为空则阻塞等待timeout且单位为unit的时长，如果这个时间段没有元素则直接返回null。
    E poll(long timeout, TimeUnit unit)
        throws InterruptedException;

   	 //获取队列剩余元素个数
    int remainingCapacity();

 	  //删除我们指定的对象，如果成功返回true，反之返回false。
    boolean remove(Object o);

    //判断队列中是否包含指定元素
    public boolean contains(Object o);

   	//将队列中的元素全部存到指定的集合中
    int drainTo(Collection<? super E> c);

    //转移maxElements个元素到集合中
    int drainTo(Collection<? super E> c, int maxElements);
}
```

了解了 `BlockingQueue` 的常见操作后，我们就知道了 `ArrayBlockingQueue` 通过继承 `BlockingQueue` 的方法并实现后，填充到 `AbstractQueue` 的方法上，由此我们便知道了上文中 `AbstractQueue` 的 `add` 方法的 `offer` 方法是哪里是实现的了。

```java
public boolean add(E e) {
        //AbstractQueue的offer来自下层的ArrayBlockingQueue从BlockingQueue继承并实现的offer方法
        if (offer(e))
            return true;
        else
            throw new IllegalStateException("Queue full");
    }
```



###### 初始化

了解 `ArrayBlockingQueue` 的细节前，我们不妨先看看其构造函数，了解一下其初始化过程。从源码中我们可以看出 `ArrayBlockingQueue` 有 3 个构造方法，而最核心的构造方法就是下方这一个。

```java
// capacity 表示队列初始容量，fair 表示 锁的公平性
public ArrayBlockingQueue(int capacity, boolean fair) {
									//如果设置的队列大小小于0，则直接抛出IllegalArgumentException
        if (capacity <= 0)
            throw new IllegalArgumentException();
        //初始化一个数组用于存放队列的元素
        this.items = new Object[capacity];
        //创建阻塞队列流程控制的锁
        lock = new ReentrantLock(fair);
        //用lock锁创建两个条件控制队列生产和消费
        notEmpty = lock.newCondition();
        notFull =  lock.newCondition();
    }
```

这个构造方法里面有两个比较核心的成员变量 `notEmpty`(非空) 和 `notFull` （非满） ，需要我们格外留意，它们是实现生产者和消费者有序工作的关键所在，这一点笔者会在后续的源码解析中详细说明，这里我们只需初步了解一下阻塞队列的构造即可。

另外两个构造方法都是基于上述的构造方法，默认情况下，我们会使用下面这个构造方法，该构造方法就意味着 `ArrayBlockingQueue` 用的是非公平锁，即各个生产者或者消费者线程收到通知后，对于锁的争抢是随机的。

```java
 public ArrayBlockingQueue(int capacity) {
        this(capacity, false);
    }
```

还有一个不怎么常用的构造方法，在初始化容量和锁的非公平性之后，它还提供了一个 `Collection` 参数，从源码中不难看出这个构造方法是将外部传入的集合的元素在初始化时直接存放到阻塞队列中。

```java
public ArrayBlockingQueue(int capacity, boolean fair,
                              Collection<? extends E> c) {
        //初始化容量和锁的公平性
        this(capacity, fair);

        final ReentrantLock lock = this.lock;
        //上锁并将c中的元素存放到ArrayBlockingQueue底层的数组中
        lock.lock();
        try {
            int i = 0;
            try {
            					//遍历并添加元素到数组中
                for (E e : c) {
                    checkNotNull(e);
                    items[i++] = e;
                }
            } catch (ArrayIndexOutOfBoundsException ex) {
                throw new IllegalArgumentException();
            }
            //记录当前队列容量
            count = i;
 													 //更新下一次put或者offer或用add方法添加到队列底层数组的位置
            putIndex = (i == capacity) ? 0 : i;
        } finally {
            //完成遍历后释放锁
            lock.unlock();
        }
    }
```



###### 阻塞式获取和新增元素

`ArrayBlockingQueue` 阻塞式获取和新增元素对应的就是生产者-消费者模型，虽然它也支持非阻塞式获取和新增元素（例如 `poll()` 和 `offer(E e)` 方法，后文会介绍到），但一般不会使用。

`ArrayBlockingQueue` 阻塞式获取和新增元素的方法为：

- `put(E e)`：将元素插入队列中，如果队列已满，则该方法会一直阻塞，直到队列有空间可用或者线程被中断。
- `take()` ：获取并移除队列头部的元素，如果队列为空，则该方法会一直阻塞，直到队列非空或者线程被中断。

这两个方法实现的关键就是在于两个条件对象 `notEmpty`(非空) 和 `notFull` （非满），这个我们在上文的构造方法中有提到。

接下来笔者就通过两张图让大家了解一下这两个条件是如何在阻塞队列中运用的。

![ArrayBlockingQueue-notEmpty-take](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172128476.png)

假设我们的代码消费者先启动，当它发现队列中没有数据，那么非空条件就会将这个线程挂起，即等待条件非空时挂起。然后 CPU 执行权到达生产者，生产者发现队列中可以存放数据，于是将数据存放进去，通知此时条件非空，此时消费者就会被唤醒到队列中使用 `take` 等方法获取值了。

![ArrayBlockingQueue-notFull-put](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172128895.png)

随后的执行中，生产者生产速度远远大于消费者消费速度，于是生产者将队列塞满后再次尝试将数据存入队列，发现队列已满，于是阻塞队列就将当前线程挂起，等待非满。然后消费者拿着 CPU 执行权进行消费，于是队列可以存放新数据了，发出一个非满的通知，此时挂起的生产者就会等待 CPU 执行权到来时再次尝试将数据存到队列中。

简单了解阻塞队列的基于两个条件的交互流程之后，我们不妨看看 `put` 和 `take` 方法的源码。

```java
public void put(E e) throws InterruptedException {
        //确保插入的元素不为null
        checkNotNull(e);
        //加锁
        final ReentrantLock lock = this.lock;
        //这里使用lockInterruptibly()方法而不是lock()方法是为了能够响应中断操作，如果在等待获取锁的过程中被打断则该方法会抛出InterruptedException异常。
        lock.lockInterruptibly();
        try {
       				 //如果count等数组长度则说明队列已满，当前线程将被挂起放到AQS队列中，等待队列非满时插入（非满条件）。
           //在等待期间，锁会被释放，其他线程可以继续对队列进行操作。
            while (count == items.length)
                notFull.await();
        			 //如果队列可以存放元素，则调用enqueue将元素入队
            enqueue(e);
        } finally {
            //释放锁
            lock.unlock();
        }
    }
```

`put`方法内部调用了 `enqueue` 方法来实现元素入队，我们继续深入查看一下 `enqueue` 方法的实现细节：

```java
private void enqueue(E x) {
       //获取队列底层的数组
        final Object[] items = this.items;
        //将putindex位置的值设置为我们传入的x
        items[putIndex] = x;
        //更新putindex，如果putindex等于数组长度，则更新为0
        if (++putIndex == items.length)
            putIndex = 0;
        //队列长度+1
        count++;
        //通知队列非空，那些因为获取元素而阻塞的线程可以继续工作了
        notEmpty.signal();
    }
```

从源码中可以看到入队操作的逻辑就是在数组中追加一个新元素，整体执行步骤为:

1. 获取 `ArrayBlockingQueue` 底层的数组 `items`。
2. 将元素存到 `putIndex` 位置。
3. 更新 `putIndex` 到下一个位置，如果 `putIndex` 等于队列长度，则说明 `putIndex` 已经到达数组末尾了，下一次插入则需要 0 开始。(`ArrayBlockingQueue` 用到了循环队列的思想，即从头到尾循环复用一个数组)
4. 更新 `count` 的值，表示当前队列长度+1。
5. 调用 `notEmpty.signal()` 通知队列非空，消费者可以从队列中获取值了。

自此我们了解了 `put` 方法的流程，为了更加完整的了解 `ArrayBlockingQueue` 关于生产者-消费者模型的设计，我们继续看看阻塞获取队列元素的 `take` 方法。

```java
public E take() throws InterruptedException {
		      //获取锁
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
        				//如果队列中元素个数为0，则将当前线程打断并存入AQS队列中，等待队列非空时获取并移除元素（非空条件）
            while (count == 0)
                notEmpty.await();
        			 //如果队列不为空则调用dequeue获取元素
            return dequeue();
        } finally {
          	 //释放锁
            lock.unlock();
        }
    }
```

理解了 `put` 方法再看`take` 方法就很简单了，其核心逻辑和`put` 方法正好是相反的，比如`put` 方法在队列满的时候等待队列非满时插入元素（非满条件），而`take` 方法等待队列非空时获取并移除元素（非空条件）。

`take`方法内部调用了 `dequeue` 方法来实现元素出队，其核心逻辑和 `enqueue` 方法也是相反的。

```java
private E dequeue() {
      	//获取阻塞队列底层的数组
        final Object[] items = this.items;
        @SuppressWarnings("unchecked")
        //从队列中获取takeIndex位置的元素
        E x = (E) items[takeIndex];
        //将takeIndex置空
        items[takeIndex] = null;
        //takeIndex向后挪动，如果等于数组长度则更新为0
        if (++takeIndex == items.length)
            takeIndex = 0;
        //队列长度减1
        count--;
        if (itrs != null)
            itrs.elementDequeued();
        //通知那些被打断的线程当前队列状态非满，可以继续存放元素
        notFull.signal();
        return x;
    }
```

由于`dequeue` 方法（出队）和上面介绍的 `enqueue` 方法（入队）的步骤大致类似，这里就不重复介绍了。

为了帮助理解，我专门画了一张图来展示 `notEmpty`(非空) 和 `notFull` （非满）这两个条件对象是如何控制 `ArrayBlockingQueue` 的存和取的。

![ArrayBlockingQueue-notEmpty-notFull](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172129779.png)



- **消费者**：当消费者从队列中 `take` 或者 `poll` 等操作取出一个元素之后，就会通知队列非满，此时那些等待非满的生产者就会被唤醒等待获取 CPU 时间片进行入队操作。
- **生产者**：当生产者将元素存到队列中后，就会触发通知队列非空，此时消费者就会被唤醒等待 CPU 时间片尝试获取元素。如此往复，两个条件对象就构成一个环路，控制着多线程之间的存和取。



###### 非阻塞式获取和新增元素

`ArrayBlockingQueue` 非阻塞式获取和新增元素的方法为：

- `offer(E e)`：将元素插入队列尾部。如果队列已满，则该方法会直接返回 false，不会等待并阻塞线程。
- `poll()`：获取并移除队列头部的元素，如果队列为空，则该方法会直接返回 null，不会等待并阻塞线程。
- `add(E e)`：将元素插入队列尾部。如果队列已满则会抛出 `IllegalStateException` 异常，底层基于 `offer(E e)` 方法。
- `remove()`：移除队列头部的元素，如果队列为空则会抛出 `NoSuchElementException` 异常，底层基于 `poll()`。
- `peek()`：获取但不移除队列头部的元素，如果队列为空，则该方法会直接返回 null，不会等待并阻塞线程。

先来看看 `offer` 方法，逻辑和 `put` 差不多，唯一的区别就是入队失败时不会阻塞当前线程，而是直接返回 `false`。

```java
public boolean offer(E e) {
        //确保插入的元素不为null
        checkNotNull(e);
        //获取锁
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
           	//队列已满直接返回false
            if (count == items.length)
                return false;
            else {
                //反之将元素入队并直接返回true
                enqueue(e);
                return true;
            }
        } finally {
            //释放锁
            lock.unlock();
        }
    }
```

`poll` 方法同理，获取元素失败也是直接返回空，并不会阻塞获取元素的线程。

```java
public E poll() {
        final ReentrantLock lock = this.lock;
        //上锁
        lock.lock();
        try {
        	//如果队列为空直接返回null，反之出队返回元素值
            return (count == 0) ? null : dequeue();
        } finally {
            lock.unlock();
        }
    }
```

`add` 方法其实就是对于 `offer` 做了一层封装，如下代码所示，可以看到 `add` 会调用没有规定时间的 `offer`，如果入队失败则直接抛异常。

```java
public boolean add(E e) {
		//调用下方的add
        return super.add(e);
    }


public boolean add(E e) {
		//调用offer如果失败直接抛出异常
        if (offer(e))
            return true;
        else
            throw new IllegalStateException("Queue full");
    }
```

`remove` 方法同理，调用 `poll`，如果返回 `null` 则说明队列没有元素，直接抛出异常。

```java
public E remove() {
        E x = poll();
        if (x != null)
            return x;
        else
            throw new NoSuchElementException();
    }
```

`peek()` 方法的逻辑也很简单，内部调用了 `itemAt` 方法。

```java
public E peek() {
        //加锁
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            //当队列为空时返回 null
            return itemAt(takeIndex);
        } finally {
            //释放锁
            lock.unlock();
        }
    }

//返回队列中指定位置的元素
@SuppressWarnings("unchecked")
final E itemAt(int i) {
    return (E) items[i];
}
```



###### 指定超时时间内阻塞式获取和新增元素

在 `offer(E e)` 和 `poll()` 非阻塞获取和新增元素的基础上，设计者提供了带有等待时间的 `offer(E e, long timeout, TimeUnit unit)` 和 `poll(long timeout, TimeUnit unit)` ，用于在指定的超时时间内阻塞式地添加和获取元素。

```java
 public boolean offer(E e, long timeout, TimeUnit unit)
        throws InterruptedException {

        checkNotNull(e);
        long nanos = unit.toNanos(timeout);
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
        //队列已满，进入循环
            while (count == items.length) {
            //时间到了队列还是满的，则直接返回false
                if (nanos <= 0)
                    return false;
                 //阻塞nanos时间，等待非满
                nanos = notFull.awaitNanos(nanos);
            }
            enqueue(e);
            return true;
        } finally {
            lock.unlock();
        }
    }
```

可以看到，带有超时时间的 `offer` 方法在队列已满的情况下，会等待用户所传的时间段，如果规定时间内还不能存放元素则直接返回 `false`。

```java
public E poll(long timeout, TimeUnit unit) throws InterruptedException {
        long nanos = unit.toNanos(timeout);
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
        	//队列为空，循环等待，若时间到还是空的，则直接返回null
            while (count == 0) {
                if (nanos <= 0)
                    return null;
                nanos = notEmpty.awaitNanos(nanos);
            }
            return dequeue();
        } finally {
            lock.unlock();
        }
    }
```

同理，带有超时时间的 `poll` 也一样，队列为空则在规定时间内等待，若时间到了还是空的，则直接返回 null。



###### 判断元素是否存在

`ArrayBlockingQueue` 提供了 `contains(Object o)` 来判断指定元素是否存在于队列中。

```java
public boolean contains(Object o) {
    //若目标元素为空，则直接返回 false
    if (o == null) return false;
    //获取当前队列的元素数组
    final Object[] items = this.items;
    //加锁
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        // 如果队列非空
        if (count > 0) {
            final int putIndex = this.putIndex;
            //从队列头部开始遍历
            int i = takeIndex;
            do {
                if (o.equals(items[i]))
                    return true;
                if (++i == items.length)
                    i = 0;
            } while (i != putIndex);
        }
        return false;
    } finally {
        //释放锁
        lock.unlock();
    }
}
```



##### ArrayBlockingQueue 获取和新增元素的方法对比

为了帮助理解 `ArrayBlockingQueue` ，我们再来对比一下上面提到的这些获取和新增元素的方法。

新增元素：

| 方法                                      | 队列满时处理方式                                         | 方法返回值 |
| ----------------------------------------- | -------------------------------------------------------- | ---------- |
| `put(E e)`                                | 线程阻塞，直到中断或被唤醒                               | void       |
| `offer(E e)`                              | 直接返回 false                                           | boolean    |
| `offer(E e, long timeout, TimeUnit unit)` | 指定超时时间内阻塞，超过规定时间还未添加成功则返回 false | boolean    |
| `add(E e)`                                | 直接抛出 `IllegalStateException` 异常                    | boolean    |

获取/移除元素：

| 方法                                | 队列空时处理方式                                    | 方法返回值 |
| ----------------------------------- | --------------------------------------------------- | ---------- |
| `take()`                            | 线程阻塞，直到中断或被唤醒                          | E          |
| `poll()`                            | 返回 null                                           | E          |
| `poll(long timeout, TimeUnit unit)` | 指定超时时间内阻塞，超过规定时间还是空的则返回 null | E          |
| `peek()`                            | 返回 null                                           | E          |
| `remove()`                          | 直接抛出 `NoSuchElementException` 异常              | boolean    |

![ArrayBlockingQueue-get-add-element-methods](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172129071.png)



##### ArrayBlockingQueue 相关面试题

###### ArrayBlockingQueue 是什么？它的特点是什么？

`ArrayBlockingQueue` 是 `BlockingQueue` 接口的有界队列实现类，常用于多线程之间的数据共享，底层采用数组实现，从其名字就能看出来了。

`ArrayBlockingQueue` 的容量有限，一旦创建，容量不能改变。

为了保证线程安全，`ArrayBlockingQueue` 的并发控制采用可重入锁 `ReentrantLock` ，不管是插入操作还是读取操作，都需要获取到锁才能进行操作。并且，它还支持公平和非公平两种方式的锁访问机制，默认是非公平锁。

`ArrayBlockingQueue` 虽名为阻塞队列，但也支持非阻塞获取和新增元素（例如 `poll()` 和 `offer(E e)` 方法），只是队列满时添加元素会抛出异常，队列为空时获取的元素为 null，一般不会使用。



###### ArrayBlockingQueue 和 LinkedBlockingQueue 有什么区别？

`ArrayBlockingQueue` 和 `LinkedBlockingQueue` 是 Java 并发包中常用的两种阻塞队列实现，它们都是线程安全的。不过，不过它们之间也存在下面这些区别：

- 底层实现：`ArrayBlockingQueue` 基于数组实现，而 `LinkedBlockingQueue` 基于链表实现。
- 是否有界：`ArrayBlockingQueue` 是有界队列，必须在创建时指定容量大小。`LinkedBlockingQueue` 创建时可以不指定容量大小，默认是`Integer.MAX_VALUE`，也就是无界的。但也可以指定队列大小，从而成为有界的。
- 锁是否分离： `ArrayBlockingQueue`中的锁是没有分离的，即生产和消费用的是同一个锁；`LinkedBlockingQueue`中的锁是分离的，即生产用的是`putLock`，消费是`takeLock`，这样可以防止生产者和消费者线程之间的锁争夺。
- 内存占用：`ArrayBlockingQueue` 需要提前分配数组内存，而 `LinkedBlockingQueue` 则是动态分配链表节点内存。这意味着，`ArrayBlockingQueue` 在创建时就会占用一定的内存空间，且往往申请的内存比实际所用的内存更大，而`LinkedBlockingQueue` 则是根据元素的增加而逐渐占用内存空间。



###### ArrayBlockingQueue 和 ConcurrentLinkedQueue 有什么区别？

`ArrayBlockingQueue` 和 `ConcurrentLinkedQueue` 是 Java 并发包中常用的两种队列实现，它们都是线程安全的。不过，不过它们之间也存在下面这些区别：

- 底层实现：`ArrayBlockingQueue` 基于数组实现，而 `ConcurrentLinkedQueue` 基于链表实现。
- 是否有界：`ArrayBlockingQueue` 是有界队列，必须在创建时指定容量大小，而 `ConcurrentLinkedQueue` 是无界队列，可以动态地增加容量。
- 是否阻塞：`ArrayBlockingQueue` 支持阻塞和非阻塞两种获取和新增元素的方式（一般只会使用前者）， `ConcurrentLinkedQueue` 是无界的，仅支持非阻塞式获取和新增元素。



###### ArrayBlockingQueue 的实现原理是什么？

`ArrayBlockingQueue` 的实现原理主要分为以下几点（这里以阻塞式获取和新增元素为例介绍）：

- `ArrayBlockingQueue` 内部维护一个定长的数组用于存储元素。
- 通过使用 `ReentrantLock` 锁对象对读写操作进行同步，即通过锁机制来实现线程安全。
- 通过 `Condition` 实现线程间的等待和唤醒操作。

这里再详细介绍一下线程间的等待和唤醒具体的实现（不需要记具体的方法，面试中回答要点即可）：

- 当队列已满时，生产者线程会调用 `notFull.await()` 方法让生产者进行等待，等待队列非满时插入（非满条件）。
- 当队列为空时，消费者线程会调用 `notEmpty.await()`方法让消费者进行等待，等待队列非空时消费（非空条件）。
- 当有新的元素被添加时，生产者线程会调用 `notEmpty.signal()`方法唤醒正在等待消费的消费者线程。
- 当队列中有元素被取出时，消费者线程会调用 `notFull.signal()`方法唤醒正在等待插入元素的生产者线程。

关于 `Condition`接口的补充：

> `Condition`是 JDK1.5 之后才有的，它具有很好的灵活性，比如可以实现多路通知功能也就是在一个`Lock`对象中可以创建多个`Condition`实例（即对象监视器），**线程对象可以注册在指定的`Condition`中，从而可以有选择性的进行线程通知，在调度线程上更加灵活。 在使用`notify()/notifyAll()`方法进行通知时，被通知的线程是由 JVM 选择的，用`ReentrantLock`类结合`Condition`实例可以实现“选择性通知”** ，这个功能非常重要，而且是 `Condition` 接口默认提供的。而`synchronized`关键字就相当于整个 `Lock` 对象中只有一个`Condition`实例，所有的线程都注册在它一个身上。如果执行`notifyAll()`方法的话就会通知所有处于等待状态的线程，这样会造成很大的效率问题。而`Condition`实例的`signalAll()`方法，只会唤醒注册在该`Condition`实例中的所有等待线程。



#### PriorityQueue

##### PriorityQueue 简介

`PriorityQueue` 是 Java 中的一种队列数据结构，被称为 **优先级队列** 。它和普通队列不同，普通队列都是遵循先进先出（FIFO）的原则，即先添加的元素先出队，后添加的元素后出队。而 `PriorityQueue` 则是按照元素的优先级来决定出队的顺序，默认情况下，优先级越小的元素越优先出队。

![queue](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172204858.png)

如下图所示，如果是普通队列，我们按照元素 1 到元素 4 的顺序依次入队的话，那么出队的顺序则也是元素 1 到 4。

![normal-queue](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172205353.png)

而优先队列的逻辑存储结构和普通队列有所不同，以 `PriorityQueue` 为例，其底层实际上是使用 **小顶堆** 形式的二叉堆，即值最小的元素优先出队。

可能很多读者对二叉堆的不是很理解，这里笔者以小顶堆为例。假如我们的元素 1 到元素 4 的优先级分别是:2、1、3、4。那么按照 JDK 所提供的小顶堆，元素的排列就会以下面这种形式呈现，可以看到二叉堆这种数据结构符合以下几种性质:

1. 二叉堆是一个完全二叉树，即在节点个数无法构成一个满二叉树(每一层节点都排满)时，叶子节点会出现在最后一层，不足双数的情况下叶子节点会尽可能靠左，如下图最后一层只有一个元素 4，就把它放在左边。
2. 小顶堆的情况下，父节点的值永远小于两个子节点，大顶堆反之。
3. 二叉堆的大小关系永远只针对父子孙这样的层级，假设我们用的是小顶堆，这并不能说明第二层节点就一定比第三层节点小，如下图的元素 3 和元素 4。

![_min-heap1](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172205627.png)

`PriorityQueue`可以让我们方便地按照优先级执行任务，只要把任务按照优先级放入队列，每次取出的都是优先级最高（值最小）的任务，不用关心排序和出队的细节。

简而言之，`PriorityQueue` 适合解决找最大值或最小值的问题，常用于任务调度、事件处理、图论算法等场景。



##### 手写 PriorityQueue

JDK 提供的 `PriorityQueue` 用到堆的思想，为了更好地理解 `PriorityQueue` 的底层设计，我会先手写一个二叉堆带读者了解一下二叉堆的基本性质以及 siftUp 和 siftDown 操作。然后，我会基于这个二叉堆模仿 JDK 实现一个自己的 `PriorityQueue` 。

并且，最后我还会用自己的 `PriorityQueue` 和 JDK 提供的 `PriorityQueue` 来真正的解决一道经典的 Leetcode 题，帮助大家熟练掌握 `PriorityQueue`。



###### 二叉堆简介

二叉堆从逻辑结构上，我们可以将其看作是一个完全二叉树，而完全二叉树是一种比较特定情况下的树形结构，它有以下几种性质:

1. 非叶子节点层次尽可能满。
2. 叶子节点数量为 `2^(h-1)` 个，注意这里是 h 是从 1 开始算的;如果不满的情况下，叶子节点要尽可能的靠左排列。

并且，二叉堆会按照排序的规则分为 **小顶堆** 和 **大顶堆** 。

如下图所示，这就是典型的小顶堆，它既像一个完全二叉树(最后一层不满的情况下，节点尽可能靠左，非叶子节点的层节点排满)。又按照小顶堆的规则排列节点，即每一个父节点的值都比它下一层的子节点小。

![min-heap1](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172207045.png)

而大顶堆则相反，在符合完全二叉树的性质的情况下，所有的父节点的值都比其下一层的子节点的值大。

![max-heap1](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172208277.png)



###### 基于 JDK 动态数组实现二叉堆

为了实现一个二叉堆，我们需要选择合适的数据结构来存储元素。常用的选择有数组和链表，二者在入队和出队操作上的时间复杂度都是 **O(logn)** 。由于链表需要额外的空间来存储节点间的父子关系，而数组可以利用地址连续固定的特点，用简单的公式来表示父子关系。因此，我们选择使用动态数组来实现一个二叉堆，这也是JDK提供的 `PriorityQueue` 的做法。

接下来，我们通过观察下图来推导一下这个表示父子关系的公式。

![minheap-enqueue-element-5-over](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172209879.png)

我们用 `parentIndex` 表示父节点索引，用 `leftIndex` 表示左子节点索引，用 `rightIndex` 表示右子节点。最终，得出以下 3 个公式:

1. `leftIndex = 2 * parentIndex+1`
2. `rightIndex = 2 * parentIndex+2`
3. `parentIndex=⌊(leftIndex/rightIndex-1)/2⌋`(⌊⌋表示向下取整)

得出这套规律之后，我们就可以开始着手编写二叉堆 `MinHeap` 的代码了。



###### 源码实现

`MinHeap` 类的定义如下：

```java
/**
 * 元素是可比较的小顶堆
 */
public class MinHeap<E> {

    private List<E> list;
    private Comparator<E> comparator;
    // ...
}
```

只有 2 个比较核心的变量属性，内容如下：

- `list`：表示存放元素的列表（底层基于数组）
- `comparator`：表示比较器对象，如果为空，使用自然排序

`MinHeap` 类的构造方法如下，共有 5 个（注释非常清楚，这里就不额外解释了）：

```java
/**
 * 创建一个空的最小堆
 */
public MinHeap() {
    list = new ArrayList<>();
}

/**
 * 创建一个空的最小堆，并指定比较器来决定元素之间的顺序
 */
public MinHeap(Comparator<E> comparator) {
    list = new ArrayList<>();
    this.comparator = comparator;
}

/**
 * 参数为数组，将给定的数组转换为一个最小堆
 */
public MinHeap(E[] arr) {
    list = new ArrayList<>(Arrays.asList(arr));
    heapify();
}

/**
 * 创建指定容量的最小堆
 */
public MinHeap(int capacity) {
    list = new ArrayList<>(capacity);
}


/**
 * 创建指定容量的最小堆并指定比较器
 */
public MinHeap(int capacity, Comparator<E> comparator) {
    list = new ArrayList<>(capacity);
    this.comparator = comparator;
}
```

第 3 个构造方法 `MinHeap(E[] arr) ` 涉及到 `heapify()` 方法，我们会再后面进行详细介绍。

接下来，我们把集合中一些常见的操作例如获取元素大小、是否为空、获取当前节点左子节点、右子节点、父节点等方法先定义好，比较简单。

```java
/**
 * 返回堆中元素个数
 */
public int size() {
    return list.size();
}

/**
 * 判断堆元素是否为空
 */
public boolean isEmpty() {
    return list.isEmpty();
}


/**
 * 获取当前节点的父节点索引
 */
private int parentIndex(int childIndex) {
    if (childIndex == 0) {
        throw new IllegalArgumentException();
    }

    return (childIndex - 1) / 2;
}

/**
 * 返回当前节点的左子节点
 */
private int leftIndex(int index) {
    return 2 * index + 1;
}

/**
 * 返回当前节点的右子节点
 */
private int rightIndex(int index) {
    return 2 * index + 2;
}
```

然后，我们再来着手开发一下入队的操作，我们还是以这个小顶堆为例，假如我们现在要插入一个元素 5(优先级为 0)，要怎么做呢？

![原始小顶堆](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172211422.png)

首先我们为了保证元素插入的效率，自然会优先将其添加到数组末端，添加完成之后，我们发现小顶堆失衡了，子节点的值比父节点的值还要小，所以我们需要 `siftUp` 保持一下平衡。

![minheap-enqueue-element-5](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172211279.png)

`siftUp` 的操作很简单，首先将元素 5 和其父节点元素 1 比较，发现元素 5 的值比元素 1 的小，两者交换一下位置。

![minheap-enqueue-element-5-shifup](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172212152.png)

因为索引 1 位置的元素变为元素 5，所以元素 5 需要再次和当前的父节点比较一下，发现元素 5 的值也比元素 2 的值小，所以两者需要再次交换位置。

最终元素 5 到达根节点，无需再进行比较，自此一个新元素入队完成。

![minheap-enqueue-element-5-over](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172212270.png)



自此我们的编码也可以很快速的编写完成了，代码如下所示,整体步骤为:

1. 将新元素插入二叉堆(数组)末端。
2. 自二叉堆末端开始开始比较，如果当前节点比父节点小，则交换两者的值，直到父节点值比子节点小或者索引为 0 为止。

```java
/**
 * 将元素存到小顶堆中
 */
public void add(E e) {
    list.add(e);
    siftUp(list.size() - 1);
}

/**
 * 为了保证新增节点后，数组仍符合小顶堆特性，这里需要siftUp保持一下平衡
 */
private void siftUp(int index) {
    if (comparator != null) {
        //循环自新增节点开始自底向上比较子节点和父节点大小，如果子节点大于父节点则交换两者的值
        while (index > 0 && comparator.compare(list.get(parentIndex(index)), list.get(index)) > 0) {
            E tmpElement = list.get(index);
            list.set(index, list.get(parentIndex(index)));
            list.set(parentIndex(index), tmpElement);
            index = parentIndex(index);
        }
    } else {
        //循环自新增节点开始自底向上比较子节点和父节点大小，如果子节点大于父节点则交换两者的值
        while (index > 0 && ((Comparable) list.get(parentIndex(index))).compareTo(list.get(index)) > 0) {
            E tmpElement = list.get(index);
            list.set(index, list.get(parentIndex(index)));
            list.set(parentIndex(index), tmpElement);
            index = parentIndex(index);
        }
    }
}
```

完成入队操作之后，我们就需要完成出队操作了。

还是以上一张图为例，此时我们的小顶堆长这样，出队操作时，我们要将堆顶元素弹出。

![minheap-enqueue-element-5-over (1)](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172216415.png)



弹出之后，堆顶为空，如果我们单纯的使用左子节点或者右子节点进行 `siftUp`,平均比较次数就会激增，所以最稳妥的做法，是将末端节点移动到堆顶，进行 `siftDown` 操作。

![minheap-dequeue-element-5](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172216873.png)

所以我们首先将末端元素移动到堆顶，然后从左右子节点中找到一个最小的和它进行比较，如果存有比它更小的，则交换位置。 经过比较我们发现元素 2 更小，所以元素 1 要和元素 2 进行位置交换。

![minheap-dequeue-element-5-siftdown](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172217108.png)

交换完成后如下图所示，此时我们发现元素 1 的值比元素 4 小，所以无需进行交换，比较结束，自此二叉堆的一个出队操作就完成了。

![minheap-dequeue-element-5-over](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172217368.png)

所以我们的出队操作的代码也实现了，这里的操作步骤和描述差不多，唯一需要注意的是比较的时候注意判断左右子节点索引计算时必须小于数组大小，避免越界问题。

```java
/**
 * 获取小顶堆堆顶的元素
 */
public E poll() {
    if (list == null || list.isEmpty()) {
        return null;
    }
    E ret = list.get(0);
    list.set(0, list.get(list.size() - 1));
    list.remove(list.size() - 1);
    siftDown(0);
    return ret;
}

/**
 * 将数组转为堆
 */
private void heapify() {
    //找到最后一个非叶子节点,往前遍历
    for (int i = parentIndex(list.size() - 1); i >= 0; i--) {
        //对每个非叶子节点执行siftDown,使其范围内保持小顶堆特性
        siftDown(i);
    }
}    

private void siftDown(int index) {

    if (comparator != null) {
        //如果左节点小于数组大小才进入循环，避免数组越界
        while (leftIndex(index) < list.size()) {
            //获取左索引
            int cmpIdx = leftIndex(index);

            //获取左或者右子节点中值更小的索引
            if (rightIndex(index) < list.size() &&
                    comparator.compare(list.get(leftIndex(index)), list.get(rightIndex(index))) > 0) {
                cmpIdx = rightIndex(index);
            }

            //如果父节点比子节点小则停止比较，结束循环
            if (comparator.compare(list.get(index), list.get(cmpIdx)) <= 0) {
                break;
            }

            //反之交换位置，将index更新为交换后的索引index，进入下一个循环
            E tmpElement = list.get(cmpIdx);
            list.set(cmpIdx, list.get(index));
            list.set(index, tmpElement);


            index = cmpIdx;

        }
    } else {
        //如果左节点小于数组大小才进入循环，避免数组越界
        while (leftIndex(index) < list.size()) {
            //获取左索引
            int cmpIdx = leftIndex(index);

            //获取左或者右子节点中值更小的索引
            if (rightIndex(index) < list.size() &&
                    ((Comparable) list.get(leftIndex(index))).compareTo(list.get(rightIndex(index))) > 0) {
                cmpIdx = rightIndex(index);
            }

            //如果父节点比子节点小则停止比较，结束循环
            if (((Comparable) list.get(index)).compareTo(list.get(cmpIdx)) <= 0) {
                break;
            }

            //反之交换位置，将index更新为交换后的索引index，进入下一个循环
            E tmpElement = list.get(cmpIdx);
            list.set(cmpIdx, list.get(index));
            list.set(index, tmpElement);
            index = cmpIdx;

        }
    }


}
```

假如我们现在有下面这样一个数组，我们希望可以将其转换为优先队列，要如何做到呢？你一定觉得，我们遍历数组将元素一个个投入到堆中即可，这样做虽然方便，但是事件复杂度却是 O(n log n),所以笔者现在就要介绍出一种复杂度为 O(n)的方法——`heapify`。

![minheap-array](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172217329.png)

`heapify` 做法很简单，即从堆的最后一个非叶子节点开始遍历每一个非叶子节点，让这些非叶子节点不断进行 `siftDown` 操作，直到根节点完成 `siftDown` 从而实现快速完成小顶堆的创建。

就以上面的数组为例子，如果我们自上而下、自作向右将其看作一个小顶堆，那么它就是下面这张图的样子。

![minheap-array-tree](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172218378.png)

首先我们找到最后一个非叶子节点，获取方式很简单，上文说到过二叉堆是一个典型的完全二叉树，假设最后一个叶子节点，即数组的最后一个元素的索引为 `childIndex` 。

则我们可以通过下面这个公式得出最后一个非叶子节点:

```java
int parentIndex=(childIndex - 1) / 2
```

是不是很熟悉呢？说白了就是我们上文实现的 `parentIndex` 方法，所以我们定位到了 23，然后 23 这个节点调用 `siftDown` 方法，查看左右节点中有没有比它更小的节点，然后完成交换，在对比过程中发现并没有，于是我们再往前推进，找到到数第二个非叶子节点 14，发现 14 的叶子节点也都比 14 大，所以 `siftDown` 没有进行任何操作，我们再次往前推进。

最终我们来到的 20，对 20 进行 `siftDown`，结果发现 17 比 20 小，所以我们将这个两个节点进行交换,交换完成之后 20 到了叶子节点，没有需要进行比较的子节点，本次 `siftDown` 结束。

![minheap-array-tree-siftdown1](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172218100.png)

继续向前推进，发现 18 的左子节点比 18 小，我们对其进行位置交换，紧接着 18 来到的新位置，发现两个子节点都比 18 大，siftDown 结束。

![minheap-array-tree-siftdown2](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172218771.png)

最终我们来到了根节点 16，发现左子节点 14 比它小，进行位置交换，然后 16 来到的新位置，发现子节点都比它大，自此数组变为一个标准的小顶堆。

![minheap-array-tree-siftdown3](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172219231.png)

了解了整个过程之后，我们的编码工作就变得很简单了，从最后一个非叶子节点往前遍历到根节点，不断执行 `siftDown`，这些都是我们现有的方法所以实现起来就很方便了。

```java
/**
 * 将数组转为堆
 */
private void heapify() {
    //找到最后一个非叶子节点,往前遍历
    for (int i = parentIndex(list.size() - 1); i >= 0; i--) {
        //对每个非叶子节点执行siftDown,使其范围内保持小顶堆特性
        siftDown(i);
    }
}
```

最后，我们再添加一个查看堆顶的元素但不移除的方法 `peek()`。

```java
public E peek() {
    if (list == null || list.isEmpty()) {
        return null;
    }
    return list.get(0);
}
```

###### 测试

为了验证笔者小顶堆各个操作的正确性，笔者编写了下面这样一段测试代码，首先随机生成 1000w 个数据存到小顶堆中，然后进行出队并将元素存到新数组中，进行遍历如果前一个元素比后一个元素大，则说明我们的小顶堆实现的有问题。

```java
int n = 1000_0000;

MinHeap<Integer> heap = new MinHeap<>(n, Comparator.comparingInt(a -> a));

//往堆中随机存放1000w个元素
for (int i = 0; i < n; i++) {
    heap.add(ThreadLocalRandom.current().nextInt(0, n));
}

int[] arr = new int[n];

//进行出队操作，并将元素存到数组中
for (int i = 0; i < n; i++) {
    arr[i] = heap.poll();
}

//循环遍历，如果前一个元素比后一个元素大，则说明我们的小顶堆有问题
for (int i = 1; i < n; i++) {
    if (arr[i - 1] > arr[i]) {
        throw new RuntimeException("err heap");
    }
}
```



###### 小结

自此我们便完成了一个二叉堆的实现，它的入队和出队操作的时间复杂度都是 O(log n)，而查询堆顶元素的复杂度则是 O(1);正是这种出色的且均衡的出队效率，使得 JDK 优先队列的实现采用的便是二叉堆的思想，而不是普通数组插入然后按照优先队列进行排序等方案。



##### 基于二叉堆实现一个 PriorityQueue

上文中我们实现了一个小顶堆，此时我们就可以基于这个小顶堆实现一个类似于的 JDK 的优先队列 `PriorityQueue`。

在实现优先队列之前，我们需要定义一个队列接口 `Queue<E> `确定一下优先队列所需要具备的行为。

```java
public interface Queue<E> {

    /**
     * 获取当前队列大小
     */
    int size();

    /**
     * 判断当前队列是否为空
     */
    boolean isEmpty();

    /**
     * 添加元素到优先队列中
     */
    boolean offer(E e);

    /**
     * 返回优先队列优先级最高的元素,如果队列不存在元素则直接返回空
     */
    E poll();

    /**
     * 查看优先队列堆顶元素,如果队列为空则返回空
     */
    E peek();
}
```

确定了队列应该具备的行为之后，我们就可以基于二叉堆实现优先队列了，由于优先级关系的维护我们已经用二叉堆实现了，所以我们的 `PriorityQueue` 实现也只需对二叉堆进行一个简单的封装即可。

```java
public class PriorityQueue<E extends Comparable> implements Queue<E> {

    private MinHeap<E> data;


    public PriorityQueue() {
        data = new MinHeap<>();
    }

    public PriorityQueue(Comparator comparator) {
        data = new MinHeap<>(comparator);
    }


    @Override
    public int size() {
        return data.size();
    }

    @Override
    public boolean isEmpty() {
        return data.isEmpty();
    }

    @Override
    public boolean offer(E e) {
        data.add(e);
        return true;
    }

    @Override
    public E poll() {
        return data.poll();
    }

    @Override
    public E peek() {
        return data.peek();
    }
}
```

我们的测试代码很简单，因为我们优先队列底层采用的是小顶堆，所以我们随机在优先队列中存放 1000w 条数据，然后使用 poll 取出并存到数组中，因为优先队列底层实现用的是小顶堆，所以假如我们的数组前一个元素大于后一个元素，我们即可说明这个优先队列优先级排列有问题，反之则说明我们的优先队列是实现是正确的。

```java
//往队列中随机添加1000_0000条数据
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();
int n = 1000_0000;
for (int i = 0; i < n; i++) {
    priorityQueue.offer(ThreadLocalRandom.current().nextInt(1, n));
}

//将优先队列中的数据按照优先级取出并存到数组中
int[] arr = new int[n];

for (int i = 0; i < n; i++) {
    arr[i] = priorityQueue.poll();
}

//如果前一个元素大于后一个元素,则说明优先队列优先级排列有问题
for (int i = 1; i < arr.length; i++) {
    if (arr[i - 1] > arr[i]) {
        throw new RuntimeException("error PriorityQueue");
    }
}
```

###### JDK 自带 PriorityQueue 使用示例

有了手写 `PriorityQueue` 的经验之后，我们对 `PriorityQueue` 已经有了不错的掌握，所以在阅读 `PriorityQueue` 源码前，我们不妨介绍一个 `PriorityQueue` 的使用示例了解一下 JDK 的 `PriorityQueue`。



###### 基本类型优先队列

第一个例子其实和我们手写的测试代码是一样的，可以看出笔者除了添加一个引包逻辑以外，并没有对代码做任何改动，从测试结果来看 JDK 的 PriorityQueue 中的元素也是按照升序进行优先级排列的。

```java
import java.util.PriorityQueue;
public class Main {


    public static void main(String[] args) {
        //往队列中随机添加1000_0000条数据
        PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();
        int n = 1000_0000;
        for (int i = 0; i < n; i++) {
            priorityQueue.offer(RandomUtil.randomInt(1, n));
        }

        //将优先队列中的数据按照优先级取出并存到数组中
        int[] arr = new int[n];

        for (int i = 0; i < n; i++) {
            arr[i] = priorityQueue.poll();
        }

        //如果前一个元素大于后一个元素,则说明优先队列优先级排列有问题
        for (int i = 1; i < arr.length; i++) {
            if (arr[i - 1] > arr[i]) {
                throw new RuntimeException("error PriorityQueue");
            }
        }

    }
}
```



###### 特殊类型优先队列

假如我们希望将自定义的对象存放到优先队列中，并且我们希望优先级按照年龄进行升序排序，那么我们就可以使用 JDK 的优先队列。 通过指明队列的泛型以及比较器，我们即可非常方便的实现一个存放自定义对象的优先队列。

```java
public class Example {
    static class Person {
        String name;
        int age;

        Person(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public int getAge() {
            return age;
        }
    }

    public static void main(String[] args) {
        PriorityQueue<Person> pq = new PriorityQueue<>(Comparator.comparing(Person::getAge));
        pq.add(new Person("Alice", 25));
        pq.add(new Person("Bob", 30));
        pq.add(new Person("Charlie", 20));

        while (!pq.isEmpty()) {
            Person p = pq.poll();
            System.out.println(p.getName() + " (" + p.getAge() + ")");
        }
    }
}
```



###### 构造函数

有了前面手写 `PriorityQueue` 以及 `PriorityQueue` 使用经验之后，我们就可以深入阅读 `PriorityQueue` 源码了。

分析源码时，我们可以先看看构造函数了解一下这个类的大概，由于 `PriorityQueue` 构造方法大部分存在复用，所以笔者找到了几个最核心的构造方法。

先来看看这个传入数组容量 `initialCapacity` 和比较器的构造方法，可以看到 `PriorityQueue` 的构造方法要求用户传入一个 `initialCapacity` 用于初始化一个数组 `queue`，不难猜出这个数组就是优先队列底层所用到的二叉小顶堆。

同时该构造方法还要求用户传入一个 `comparator` 即一个比较器，说明 `queue` 的元素在进行入队操作时是需要比较的，而这个 `comparator` 就是比较的依据。

```java
public PriorityQueue(int initialCapacity,
                         Comparator<? super E> comparator) {
        //如果初始化容量小于1则抛出异常
        if (initialCapacity < 1)
            throw new IllegalArgumentException();
        this.queue = new Object[initialCapacity];
        this.comparator = comparator;
    }
```

再来看看另一个核心构造方法，我们发现 PriorityQueue 支持将不同的 Collection 类转为 PriorityQueue，对此我们不妨对每一段逻辑进行深入分析。

```java
public PriorityQueue(Collection<? extends E> c) {
		//SortedSet类型的集合转为PriorityQueue，从SortedSet中获取一个比较器进行初始化，然后按照比较器的规则转移元素到PriorityQueue中
        if (c instanceof SortedSet<?>) {
            SortedSet<? extends E> ss = (SortedSet<? extends E>) c;
            this.comparator = (Comparator<? super E>) ss.comparator();
            initElementsFromCollection(ss);
        }
        //如果集合类型是PriorityQueue则获取PriorityQueue的比较器，并将PriorityQueue的元素转移到我们的PriorityQueue中
        else if (c instanceof PriorityQueue<?>) {
            PriorityQueue<? extends E> pq = (PriorityQueue<? extends E>) c;
            this.comparator = (Comparator<? super E>) pq.comparator();
            initFromPriorityQueue(pq);
        }
        else {
            this.comparator = null;
            initFromCollection(c);
        }
    }
```

当集合类型为 `SortedSet` 时，因为 `SortedSet` 是天生有序的，所以 `PriorityQueue` 直接获取其比较器之后，调用了一个 `initElementsFromCollection`，我们不妨看看 `initElementsFromCollection` 具体做了些什么。

```java
//SortedSet类型的集合转为PriorityQueue，从SortedSet中获取一个比较器进行初始化，然后按照比较器的规则转移元素到PriorityQueue中
        if (c instanceof SortedSet<?>) {
            SortedSet<? extends E> ss = (SortedSet<? extends E>) c;
            this.comparator = (Comparator<? super E>) ss.comparator();
            initElementsFromCollection(ss);
        }
```

可以看到 `initElementsFromCollection` 只不过是将集合元素转为数组，然后赋值给优先队列底层的数组成员变量 queue。`当` a 数组未能返回一个 `Object[]`类型时，则调用 `Arrays.copyOf` 方法将其转为为一个正确的 `Object[]`数组。 然后遍历元素进行判空，一切正常则将其赋值给 `queue`，并记录此时 `queue` 的长度。

```java
private void initElementsFromCollection(Collection<? extends E> c) {
        Object[] a = c.toArray();
        // If c.toArray incorrectly doesn't return Object[], copy it.
        if (a.getClass() != Object[].class)
            a = Arrays.copyOf(a, a.length, Object[].class);
        int len = a.length;
        if (len == 1 || this.comparator != null)
            for (int i = 0; i < len; i++)
                if (a[i] == null)
                    throw new NullPointerException();
        this.queue = a;
        this.size = a.length;
    }
```

我们再来看看 `PriorityQueue` 集合传入时的处理逻辑，同样的将 `PriorityQueue` 的比较器赋值给当前 `PriorityQueue` 之后，调用了一个 `initFromPriorityQueue` 方法，我们步入看看。

```java
 //如果集合类型是PriorityQueue则获取PriorityQueue的比较器，并将PriorityQueue的元素转移到我们的PriorityQueue中
        else if (c instanceof PriorityQueue<?>) {
            PriorityQueue<? extends E> pq = (PriorityQueue<? extends E>) c;
            this.comparator = (Comparator<? super E>) pq.comparator();
            initFromPriorityQueue(pq);
        }
```

可以看到 `initFromPriorityQueue` 操作就是让传入的 `PriorityQueue` 通过 `toArray` 返回底层的小顶堆数组，然后赋值给我们的 `PriorityQueue`，在记录一下当前 `PriorityQueue` 的长度。

```java
private void initFromPriorityQueue(PriorityQueue<? extends E> c) {
        if (c.getClass() == PriorityQueue.class) {
            this.queue = c.toArray();
            this.size = c.size();
        } else {
           //略
        }
    }
```

对于一般集合，我们的逻辑会走到这里，默认设置比较器为空，然后也调用了 `initFromCollection`，我们步入查看逻辑。

```java
 else {
            this.comparator = null;
            initFromCollection(c);
        }
```

对于非 `PriorityQueue` 的集合，它调用了 `initFromCollection`，我们步入看看。

```java
private void initFromPriorityQueue(PriorityQueue<? extends E> c) {
        if (c.getClass() == PriorityQueue.class) {
          //略
        } else {
            initFromCollection(c);
        }
    }
```

可以看到它的实现就是调用上文所介绍的 `initElementsFromCollection` 将数组存到 `queue` 中，然后调用一个 `heapify` 将这个数组转为小顶堆。

```java
 private void initFromCollection(Collection<? extends E> c) {
        initElementsFromCollection(c);
        heapify();
    }
```

对于 JDK 实现的 `heapify`，可以发现它的逻辑和我们所实现的差不多，只不过获取父节点的操作使用了高效的右移运算，同样的遍历所有非叶子节点进行 `siftDown` 生成一个完整的小顶堆。

```java
private void heapify() {
        for (int i = (size >>> 1) - 1; i >= 0; i--)
            siftDown(i, (E) queue[i]);
    }
```



###### 入队操作

对于 PriorityQueue 来说，核心的入队就是 offer，它的核心步骤为:

1. 校验元素是否为空。
2. 设置新插入的位置为 size。
3. 判断数组容量是否足够，如果不够则扩容。
4. 如果是第一个元素，则直接将其放到索引 0 位置。
5. 如果不是第一个元素，则调用 `siftUp` 将元素放入队列。

```java
public boolean offer(E e) {
		//校验元素是否为空
        if (e == null)
            throw new NullPointerException();
        modCount++;
        //设置新插入的位置为size
        int i = size;
        //判断数组容量是否足够，如果不够则扩容
        if (i >= queue.length)
            grow(i + 1);
        size = i + 1;
        //如果是第一个元素，则直接将其放到索引0位置
        if (i == 0)
            queue[0] = e;
        else
        //如果不是第一个元素，则调用siftUp将元素放入队列
            siftUp(i, e);
        return true;
    }
```

在 `siftUp` 操作时会判断比较器是否为空，如果不为空则使用传入的比较器生成小顶堆，反之就将元素 x 转为 `Comparable` 对象进行比较。 因为整体比较逻辑都一样，所以我们就以 `siftUpUsingComparator` 查看一下进行 `siftUp` 操作时对于入队元素的处理逻辑。

```java
private void siftUp(int k, E x) {
        if (comparator != null)
            siftUpUsingComparator(k, x);
        else
            siftUpComparable(k, x);
    }
```

`siftUpUsingComparator` 和我们手写的 `siftUp` 逻辑差不多,都是不断向上比较父节点，找到比自己大的则交换位置，直到到达根节点或者比较父节点比自己小为止，整体来说 `PriorityQueue` 的 `siftUp` 分为以下几个步骤:

1. 获取入队元素当前索引位置的父索引 parent。
2. 根据父索引找到元素 e。
3. 如果新节点 x 比 e 大，则说明当前入队操作符合小顶堆要求，直接结束循环。
4. 如果 x 比 e 小，则将父节点 e 的值改为我们入队元素 x 的值。
5. k 指向父索引，继续循环向上比较父索引，直到找到比 x 还小的父节点 e，终止循环。
6. 将 x 存到符合要求的索引位置 k。

```java
private void siftUpUsingComparator(int k, E x) {
        while (k > 0) {
        //获取入队元素当前索引位置的父索引parent
            int parent = (k - 1) >>> 1;
            //根据父索引找到元素e
            Object e = queue[parent];
            // 如果新节点x比e大，则说明当前入队操作符合小顶堆要求，直接结束循环
            if (comparator.compare(x, (E) e) >= 0)
                break;
			//如果x比e小，则将父节点e的值改为我们入队元素x的值
            queue[k] = e;
            //k指向父索引，继续循环向上比较父索引，直到找到比x还小的父节点e，终止循环
            k = parent;
        }
        //将x存到符合要求的索引位置k
        queue[k] = x;
    }
```



###### 出队操作

出队操作和我们手写的逻辑也差不多，只不过逻辑处理的更加细致，它的逻辑步骤为:

1. size 减 1 并赋值给 s。
2. 如果队列为空则返回 null。
3. 如果队列不为空则拷贝索引 0 位置即优先级最高的元素。
4. 将数组 0 位置设置为 null。
5. 如果 s 不为 0，说明队列中不止一个元素，需要维持小顶堆的特性，需要从堆顶开始进行 siftDown 操作。
6. 返回优先队列优先级最高的元素 result。

```java
public E poll() {
        if (size == 0)
            return null;
        int s = --size;
        modCount++;
        E result = (E) queue[0];
        E x = (E) queue[s];
        queue[s] = null;
        if (s != 0)
            siftDown(0, x);
        return result;
    }
```



###### 查看优先级最高的元素

peek 方法可以不改变队列结构查看优先级最高的元素，如果队列为空则返回 null，反之返回 0 索引位置的元素。

```java
 public E peek() {
        return (size == 0) ? null : (E) queue[0];
    }
```



##### Leetcode 中关于 PriorityQueue 的使用

因为优先队列自带优先级排列的天然优势，所以使用优先队列进行一些词频统计等操作也是非常快速和方便的。

就比如 [Leetcode 347. 前 K 个高频元素这道题目，它要求我们返回整数数组中前 k 个高频元素，常规做法我们可以会将元素存放到 `map` 中，然后对这个 `map` 进行排序，尽管它确实可以完成这个题目，但是从排序和复杂度和优先队列差不多都是 `O(n log n)`，但在实现的复杂度上，在 `PriorityQueue` 的封装下，使用 `PriorityQueue` 来存放元素以及从元素中获取前 k 个元素的操作，相比于前者，同样的思想下后者的实现会更简单一些。



![leetcode347](https://raw.githubusercontent.com/ZXJ-OvO/picgo-img/master/202307172219218.png)

实现思路

解决这个问题我们需要统计一下词频，所以我们需要借助额外的集合，所以整体步骤为:

1. 用一个 map 记录一下每一个元素的频率。
2. 创建一个优先队列，比较这个 map 中的 value，按照 value 生成优先队列。
3. 如果队列长度小于 k(即题目要求返回的前 k 个高频元素)，则直接将元素存入队列。
4. 如果队列长度等于 k，则比较优先队列中队首的元素(value 最小的元素)是否比要入队的元素，如果入队元素比队首元素大，则说明入队元素出现频率比队首元素更频繁，则移除队首元素，再将新添加的元素入队。
5. 遍历优先队列元素，存放到数组中返回。

最终我们就实现了这套完整的代码:

```java
public int[] topKFrequent(int[] nums, int k) {
        //统计各个元素出现的频率并存到map中
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            if (!map.containsKey(num)) {
                map.put(num, 1);
            } else {
                map.put(num, map.get(num) + 1);
            }
        }

        //优先队列按照map的value进行升序排序
        PriorityQueue<Integer> queue = new PriorityQueue<>(Comparator.comparingInt(map::get));

        //遍历map,筛选出前k个高频元素
        map.entrySet().forEach(e -> {
            if (queue.size() < k) {
                queue.add(e.getKey());
            } else if (e.getValue() > map.get(queue.peek())) {
                queue.remove();
                queue.add(e.getKey());
            }
        });

        //转为数组返回
        int[] arr = queue.stream().mapToInt(Integer::intValue).toArray();
        return arr;

    }
```

```java
public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            if (!map.containsKey(num)) {
                map.put(num, 1);
            } else {
                map.put(num, map.get(num) + 1);
            }
        }


        PriorityQueue<Integer> queue = new PriorityQueue<>(Comparator.comparingInt(map::get));

        map.entrySet().forEach(e -> {
            if (queue.size() < k) {
                queue.offer(e.getKey());
            } else if (e.getValue() > map.get(queue.peek())) {
                queue.poll();
                queue.offer(e.getKey());
            }
        });


        List<Integer> list = new ArrayList<>();
        while (!queue.isEmpty()) {
            list.add(queue.poll());
        }

        return list.stream().mapToInt(Integer::intValue).toArray();

    }

    public static void main(String[] args) {
        int[] nums = {1,1,32,312,3,432,412412412,44,4,444,444,4,4,4,4,4,4,4,4,4,6,6,6,6,6,4,4,4,44,3,4324,34,23432,4324,324,324,324,3242,34};
        int k = 2;
        for (int i : (new Solution()).topKFrequent(nums, k)) {
            System.out.println(i);
        }
    }
```

##### PriorityQueue 常见面试题

###### PriorityQueue 为什么要用二叉堆实现，二叉堆有什么使用优势吗？

这个问题我们可以用反证法来分析:

1. 假如我们使用排序的数组，那么入队操作则是 O(1),但出队操作确实得我们需要遍历一遍数组找到最小的哪一个,所以复杂度为 O(n)。
2. 假如我们使用有序数组来实现，那么入队操作则因为需要排序变为 O(n),而出队操作则变为 O(1)。 所以折中考虑，使用带有完全二叉树性质的二叉堆，使得入队和出队操作都是 O(log^n)最合适。



###### PriorityQueue 是线程安全的吗？

我们随意查看入队源码，没有任何针对线程安全的处理操作，所以它是非线程安全的。



###### PriorityQueue 底层是用什么实现的？初始化容量是多少？

我们查看 `PriorityQueue` 的默认构造方法,可以看到 `PriorityQueue` 底层使用一个数组来表示优先队列，而这个数组实际上用到的 **小顶堆** 的思想来维持优先级的。

初始化容量我们可以查看构造方法有一个`DEFAULT_INITIAL_CAPACITY,DEFAULT_INITIAL_CAPACITY` 用于初始化数组，查看其定义可以发现默认初始化容量大小为 11。

```java
private static final int DEFAULT_INITIAL_CAPACITY = 11;
public PriorityQueue() {
        this(DEFAULT_INITIAL_CAPACITY, null);
    }
```



###### 如果我希望 PriorityQueue 按照从大到小的顺序排序要怎么做？

因为 `PriorityQueue` 底层的数组实现的是一个小顶堆，所以如果我们希望按照降序排列可以将比较器取反一下即可。

示例代码：

```java
public static void main(String[] args) {
        //将比较器取反一下实现升序排列
        PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(Comparator.comparingInt(Integer::intValue).reversed());
        //随机插入10个元素
        int n = 10;
        for (int i = 0; i < n; i++) {
            int num = RandomUtil.randomInt(1, n);
            System.out.println("add " + num);
            priorityQueue.offer(num);
        }

        //输出看看是否符合预期
        int s = priorityQueue.size();
        for (int i = 0; i < s; i++) {
            System.out.println(priorityQueue.poll());
        }

    }
```



###### 为什么 PriorityQueue 底层用数组构成小顶堆而不是使用链表呢？

先说结论： **使用数组避免了为维护父子及左邻右舍等节点关系的内存空间占用** 。

**为什么还要维护左邻右舍关系呢？** 我们都知道 `PriorityQueue` 支持传入一个集合生成优先队列，假如我们传入的是一个无序的 `List`,那么在数组转二叉堆时需要经过一个 `heapify` 的操作，该操作需要从最后一个非叶子节点开始，直到根节点为止,不断 `siftDown` 维持自己以及子孙节点间的优先级关系。

如果使用链表这些关系的维护就变得繁琐且占用内存空间，使用数组就不一样了，因为地址的连续性和明确性，我们定位邻节点只需按照公式获得最后一个非叶子节点的索引，然后不断减一就能到达邻节点了。

综上所述，使用数组可以以`O(1)` 的时间复杂度定位到最后一个非叶子节点,通过一个减 1 操作即到达下一个非叶子节点，这种轻量级的关系维护是链表所不具备的。





















## 并发编程

## IO/NIO

## JVM

## JDK新特性



# 前端基础

#### setInterval()和setTimeout()的作用，怎么关闭setInterval()

`setInterval()` 和 `setTimeout()` 是 JavaScript 中的定时器函数，用于在指定的时间间隔或延迟后执行指定的函数或代码块。

1. `setInterval()`：
   - `setInterval()` 函数用于重复执行指定的函数或代码块，每隔一定的时间间隔执行一次，直到被取消。
   - 它接受两个参数：第一个参数是要执行的函数或代码块，第二个参数是时间间隔（以毫秒为单位）。
   - 例如：`setInterval(function(){ /* code to be executed */ }, 1000);` 会每隔一秒执行一次指定的代码块。

2. `setTimeout()`：
   - `setTimeout()` 函数用于延迟执行指定的函数或代码块，只执行一次。
   - 它接受两个参数：第一个参数是要执行的函数或代码块，第二个参数是延迟时间（以毫秒为单位）。
   - 例如：`setTimeout(function(){ /* code to be executed */ }, 2000);` 会在延迟两秒后执行指定的代码块。

关闭 `setInterval()` 定时器的方法是使用 `clearInterval()` 函数。`clearInterval()` 接受一个参数，即要取消的定时器的标识符（返回值为 `setInterval()` 的调用结果）。例如：

```javascript
const intervalId = setInterval(function() {
  /* code to be executed repeatedly */
}, 1000);

// 要取消定时器，调用 clearInterval() 并传入标识符
clearInterval(intervalId);
```

以上代码将会停止之前使用 `setInterval()` 设置的定时器，不再执行指定的代码块。



# 计算机基础

## 计算机网络

#### HTTP 请求的 GET 与 POST 方式的区别

HTTP 请求的 GET 和 POST 方式是两种常用的请求方法，它们在以下几个方面有区别：

1. 参数传递方式：
   - GET：参数通过 URL 的查询字符串传递，以键值对的形式出现在 URL 后面，例如 `https://example.com/api?param1=value1&param2=value2`
   - POST：参数通过请求体传递，以键值对或其他格式（如 JSON）出现在请求的主体中。

2. 数据安全性：
   - GET：参数暴露在 URL 中，可能会被保存在浏览器历史记录或服务器日志中，不适合传递敏感信息。
   - POST：参数在请求体中，相对更安全，不会出现在 URL 中，适合传递敏感信息。

3. 数据长度限制：
   - GET：URL 有长度限制，不同浏览器和服务器对 URL 长度的限制可能不同，通常较小。
   - POST：由于参数在请求体中，通常没有明显长度限制，可以传递较大的数据。

4. 数据语义：
   - GET：用于获取资源，请求应该是无副作用的，不应该对服务器数据做修改。
   - POST：用于提交数据，可能对服务器产生副作用，会修改服务器上的数据。

5. 缓存：
   - GET：可被缓存，浏览器会缓存 GET 请求的结果。
   - POST：默认不可被缓存，不会被浏览器缓存。

总结：GET 适用于获取数据，参数在 URL 中传递，不适合传递敏感信息，而 POST 适用于提交数据，参数在请求体中传递，相对更安全，适合传递敏感信息和大量数据。



## 操作系统

## 数据结构

## 算法

# 数据库

## 数据库基础

## MySQL

## Redis

## Elasticsearch

## MongoDB

# 开发工具

## Maven

### Maven 介绍

Apache Maven 的本质是一个基于项目对象模型 (Project Object Model，POM) 软件项目管理和理解工具。



### Maven 坐标

- **groupId**(必须): 定义了当前 Maven 项目隶属的组织或公司。groupId 一般分为多段，通常情况下，第一段为域，第二段为公司名称。域又分为 org、com、cn 等，其中 org 为非营利组织，com 为商业组织
- **artifactId**(必须)：定义了当前 Maven 项目的名称，项目的唯一的标识符，对应项目根目录的名称。
- **version**(必须)：定义了 Maven 项目当前所处版本。
- **packaging**（可选）：定义了 Maven 项目的打包方式（比如 jar，war...），默认使用 jar。
- **classifier**(可选)：常用于区分从同一 POM 构建的具有不同内容的构件，可以是任意的字符串，附加在版本号之后。



### Maven 依赖

#### 依赖配置

```xml
<project>
    <dependencies>
        <dependency>
            <groupId></groupId>
            <artifactId></artifactId>
            <version></version>
            <type>...</type>
            <scope>...</scope>
            <optional>...</optional>
            <exclusions>
                <exclusion>
                  <groupId>...</groupId>
                  <artifactId>...</artifactId>
                </exclusion>
          </exclusions>
        </dependency>
      </dependencies>
</project>
```

- dependencies：一个 pom.xml 文件中只能存在一个，用来管理依赖的总标签。
- dependency：包含在 dependencies 标签中，可以有多个，每一个表示项目的一个依赖。
- groupId,artifactId,version(必要)：依赖的基本坐标，对于任何一个依赖来说。
- type(可选)：依赖的类型，对应于项目坐标定义的 packaging。大部分情况下，该元素不必声明，其默认值是 jar。
- scope(可选)：依赖的范围，默认值是 compile。
- optional(可选)：标记依赖是否可选
- exclusions(可选)：用来排除传递性依赖,例如 jar 包冲突



#### 依赖范围

**classpath** 用于指定 `.class` 文件存放的位置，类加载器会从该路径中加载所需的 `.class` 文件到内存中。

Maven 在编译、执行测试、实际运行有着三套不同的 classpath：

- **编译 classpath**：编译主代码有效
- **测试 classpath**：编译、运行测试代码有效
- **运行 classpath**：项目运行时有效

Maven 的依赖范围如下：

- **compile**：编译依赖范围（默认），使用此依赖范围对于编译、测试、运行三种都有效
- **test**：测试依赖范围，此依赖范围只能用于测试，如JUnit
- **provided**：此依赖范围，对于编译和测试有效，而对运行时无效。比如 `servlet-api.jar`
- **runtime**：运行时依赖范围，对于测试和运行有效，但是在编译主代码时无效，如 JDBC 驱动
- **system**：系统依赖范围，使用 system 范围的依赖时必须通过 systemPath 元素显示地指定依赖文件的路径，不依赖 Maven 仓库解析，所以可能会造成建构的不可移植



#### 传递依赖性

1. 对于 Maven 而言，同一个 groupId 同一个 artifactId 下，只能使用一个 version。若相同类型但版本不同的依赖存在于同一个 pom 文件，只会引入后一个声明的依赖。

   ```xml
   <dependency>
       <groupId>in.hocg.boot</groupId>
       <artifactId>mybatis-plus-spring-boot-starter</artifactId>
       <version>1.0.48</version>
   </dependency>
   <!-- 只会使用 1.0.49 这个版本的依赖 -->
   <dependency>
       <groupId>in.hocg.boot</groupId>
       <artifactId>mybatis-plus-spring-boot-starter</artifactId>
       <version>1.0.49</version>
   </dependency>
   
   ```

2. 项目的两个依赖同时引入了某个依赖。遵循**路径最短优先**和**声明顺序优先**两大原则。解决这个问题的过程也被称为 Maven 依赖调解 。

   ```
   依赖链路一：A -> B -> C -> X(1.0)
   依赖链路二：A -> D -> X(2.0)
   ```

   两条依赖路径上有两个版本的 X，为了避免依赖重复，Maven 只会选择其中的一个进行解析。

   **路径最短优先**

   ```text
   依赖链路一：A -> B -> C -> X(1.0) // dist = 3
   依赖链路二：A -> D -> X(2.0) // dist = 2
   ```

   依赖链路二的路径最短，因此，X(2.0)会被解析使用。

   不过，你也可以发现。路径最短优先原则并不是通用的，像下面这种路径长度相等的情况就不能单单通过其解决了：

   ```text
   依赖链路一：A -> B -> X(1.0) // dist = 3
   依赖链路二：A -> D -> X(2.0) // dist = 2
   ```

   因此，Maven 又定义了声明顺序优先原则。

   依赖调解第一原则不能解决所有问题，比如这样的依赖关系：A->B->Y(1.0)、A-> C->Y(2.0)，Y(1.0)和 Y(2.0)的依赖路径长度是一样的，都为 2。Maven 定义了依赖调解的第二原则：

   **声明顺序优先**

   在依赖路径长度相等的前提下，在 `pom.xml` 中依赖声明的顺序决定了谁会被解析使用，顺序最前的那个依赖优胜。该例中，如果 B 的依赖声明在 D 之前，那么 X (1.0)就会被解析使用。

   ```xml
   <!-- A pom.xml -->
   <dependencies>
       ...
       dependency B
       ...
       dependency D
   </dependencies>
   ```

### 排除依赖

单纯依赖 Maven 来进行依赖调解，在很多情况下是不适用的，需要我们手动排除依赖。

举个例子，当前项目存在下面这样的依赖关系：

```text
依赖链路一：A -> B -> C -> X(1.5) // dist = 3
依赖链路二：A -> D -> X(1.0) // dist = 2
```

根据路径最短优先原则，X(1.0) 会被解析使用，也就是说实际用的是 1.0 版本的 X。

但是：

- 如果 D 依赖用到了 1.5 版本的 X 中才有的一个类，运行项目就会报`NoClassDefFoundError`错误。

- 如果 D 依赖用到了 1.5 版本的 X 中才有的一个方法，运行项目就会报`NoSuchMethodError`错误。

可以通过`exclusion`标签手动将 X(1.0) 给排除。

```xml
<dependency>
    ......
    <exclusions>
      <exclusion>
        <artifactId>x</artifactId>
        <groupId>org.apache.x</groupId>
      </exclusion>
    </exclusions>
</dependency>
```

一般解决依赖冲突的时候，优先保留版本较高的。这是因为大部分 jar 在升级的时候都会做到向下兼容。

如果高版本修改了低版本的一些类或者方法的话，这个时候就不能直接保留高版本了，而是应该考虑优化上层依赖，比如升级上层依赖的版本。

```text
依赖链路一：A -> B -> C -> X(1.5) // dist = 3
依赖链路二：A -> D -> X(1.0) // dist = 2
```

保留 1.5 版本的 X，但是这个版本的 X 删除了 1.0 版本中的某些类。这个时候可以考虑升级 D 的版本到一个 X 兼容的版本。



### Maven 仓库

> 在 Maven 中，任何一个依赖、插件或者项目构建的输出，都可以称为 **构件** 。

Maven 仓库分为：

- **本地仓库**：运行 Maven 的计算机上的一个目录，它缓存远程下载的构件并包含尚未发布的临时构件。

- **远程仓库**：官方或者其他组织维护的 Maven 仓库。

  - **中央仓库**：这个仓库是由 Maven 社区来维护的，里面存放了绝大多数开源软件的包

  - **私服**：私服是一种特殊的远程 Maven 仓库，它是架设在局域网内的仓库服务

  - **其他的公共仓库**：有一些公共仓库是为了加速访问（阿里云 Maven 镜像）或者部分不存在于中央仓库中的构件

Maven 依赖包寻找顺序：

1. 先去本地仓库找寻，有就直接使用。
2. 本地仓库没有找到的话，会去远程仓库找，下载包到本地仓库。
3. 远程仓库没有找到的话，会报错。



### Maven 生命周期

Maven 的生命周期就是为了对所有的构建过程进行抽象和统一，包含了项目的清理、初始化、编译、测试、打包、集成测试、验证、部署和站点生成等几乎所有构建步骤。

Maven 定义了 3 个生命周期`META-INF/plexus/components.xml`：

- `default`生命周期
- `clean`生命周期
- `site`生命周期

这些生命周期是相互独立的，每个生命周期包含多个阶段(phase)。并且，这些阶段是有序的，也就是说，后面的阶段依赖于前面的阶段。当执行某个阶段的时候，会先执行它前面的阶段。

执行 Maven 生命周期的命令格式如下：

```bash
mvn 阶段 [阶段2] ...[阶段n]
```



#### default 生命周期

`default`生命周期是在没有任何关联插件的情况下定义的，是 Maven 的主要生命周期，用于构建应用程序，共包含 23 个阶段。

```xml
<phases>
  <!-- 验证项目是否正确，并且所有必要的信息可用于完成构建过程 -->
  <phase>validate</phase>
  <!-- 建立初始化状态，例如设置属性 -->
  <phase>initialize</phase>
  <!-- 生成要包含在编译阶段的源代码 -->
  <phase>generate-sources</phase>
  <!-- 处理源代码 -->
  <phase>process-sources</phase>
  <!-- 生成要包含在包中的资源 -->
  <phase>generate-resources</phase>
  <!-- 将资源复制并处理到目标目录中，为打包阶段做好准备。 -->
  <phase>process-resources</phase>
  <!-- 编译项目的源代码  -->
  <phase>compile</phase>
  <!-- 对编译生成的文件进行后处理，例如对 Java 类进行字节码增强/优化 -->
  <phase>process-classes</phase>
  <!-- 生成要包含在编译阶段的任何测试源代码 -->
  <phase>generate-test-sources</phase>
  <!-- 处理测试源代码 -->
  <phase>process-test-sources</phase>
  <!-- 生成要包含在编译阶段的测试源代码 -->
  <phase>generate-test-resources</phase>
  <!-- 处理从测试代码文件编译生成的文件 -->
  <phase>process-test-resources</phase>
  <!-- 编译测试源代码 -->
  <phase>test-compile</phase>
  <!-- 处理从测试代码文件编译生成的文件 -->
  <phase>process-test-classes</phase>
  <!-- 使用合适的单元测试框架（Junit 就是其中之一）运行测试 -->
  <phase>test</phase>
  <!-- 在实际打包之前，执行任何的必要的操作为打包做准备 -->
  <phase>prepare-package</phase>
  <!-- 获取已编译的代码并将其打包成可分发的格式，例如 JAR、WAR 或 EAR 文件 -->
  <phase>package</phase>
  <!-- 在执行集成测试之前执行所需的操作。 例如，设置所需的环境 -->
  <phase>pre-integration-test</phase>
  <!-- 处理并在必要时部署软件包到集成测试可以运行的环境 -->
  <phase>integration-test</phase>
  <!-- 执行集成测试后执行所需的操作。 例如，清理环境  -->
  <phase>post-integration-test</phase>
  <!-- 运行任何检查以验证打的包是否有效并符合质量标准。 -->
  <phase>verify</phase>
  <!-- 	将包安装到本地仓库中，可以作为本地其他项目的依赖 -->
  <phase>install</phase>
  <!-- 将最终的项目包复制到远程仓库中与其他开发者和项目共享 -->
  <phase>deploy</phase>
</phases>
```

当执行 `mvn test`命令的时候，会执行从 validate 到 test 的所有阶段，这也就解释了为什么执行测试的时候，项目的代码能够自动编译。



#### clean 生命周期

clean 生命周期的目的是清理项目，共包含 3 个阶段：

1. pre-clean
2. clean
3. post-clean

```xml
<phases>
  <!--  执行一些需要在clean之前完成的工作 -->
  <phase>pre-clean</phase>
  <!--  移除所有上一次构建生成的文件 -->
  <phase>clean</phase>
  <!--  执行一些需要在clean之后立刻完成的工作 -->
  <phase>post-clean</phase>
</phases>
<default-phases>
  <clean>
    org.apache.maven.plugins:maven-clean-plugin:2.5:clean
  </clean>
</default-phases>
```

当执行 `mvn clean` 的时候，会执行 clean 生命周期中的 pre-clean 和 clean 阶段。



#### site 生命周期

site 生命周期的目的是建立和发布项目站点，共包含 4 个阶段：

1. pre-site
2. site
3. post-site
4. site-deploy

```xml
<phases>
  <!--  执行一些需要在生成站点文档之前完成的工作 -->
  <phase>pre-site</phase>
  <!--  生成项目的站点文档作 -->
  <phase>site</phase>
  <!--  执行一些需要在生成站点文档之后完成的工作，并且为部署做准备 -->
  <phase>post-site</phase>
  <!--  将生成的站点文档部署到特定的服务器上 -->
  <phase>site-deploy</phase>
</phases>
<default-phases>
  <site>
    org.apache.maven.plugins:maven-site-plugin:3.3:site
  </site>
  <site-deploy>
    org.apache.maven.plugins:maven-site-plugin:3.3:deploy
  </site-deploy>
</default-phases>
```

Maven 能够基于 `pom.xml` 所包含的信息，自动生成一个友好的站点，方便团队交流和发布项目信息。



### Maven 插件

Maven 本质上是一个插件执行框架，所有的执行过程，都是由一个一个插件独立完成的。

> 插件地址位于本地仓库下：本地仓库名\org\apache\maven\plugins



除了 Maven 自带的插件之外，还有一些三方提供的插件比如单测覆盖率插件 jacoco-maven-plugin、帮助开发检测代码中不合规范的地方的插件 maven-checkstyle-plugin、分析代码质量的 sonar-maven-plugin。

jacoco-maven-plugin 使用示例：

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.jacoco</groupId>
      <artifactId>jacoco-maven-plugin</artifactId>
      <version>0.8.8</version>
      <executions>
        <execution>
          <goals>
            <goal>prepare-agent</goal>
          </goals>
        </execution>
        <execution>
          <id>generate-code-coverage-report</id>
          <phase>test</phase>
          <goals>
            <goal>report</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

可以将 Maven 插件理解为一组任务的集合，用户可以通过命令行直接运行指定插件的任务，也可以将插件任务挂载到构建生命周期，随着生命周期运行。

Maven 插件被分为下面两种类型：

- **Build plugins**：在构建时执行。
- **Reporting plugins**：在网站生成过程中执行。



### Maven 多模块管理

多模块管理简单地来说就是将一个项目分为多个模块，每个模块只负责单一的功能实现。直观的表现就是一个 Maven 项目中不止有一个 `pom.xml` 文件，会在不同的目录中有多个 `pom.xml` 文件，进而实现多模块管理。



多模块管理除了可以更加便于项目开发和管理，还有如下好处：

1. 降低代码之间的耦合性（从类级别的耦合提升到 jar 包级别的耦合）；
2. 减少重复，提升复用性；
3. 每个模块都可以是自解释的（通过模块名或者模块文档）；
4. 模块还规范了代码边界的划分，开发者很容易通过模块确定自己所负责的内容。



多模块管理下，会有一个父模块，其他的都是子模块。父模块通常只有一个 `pom.xml`，没有其他内容。父模块的 `pom.xml` 一般只定义了各个依赖的版本号、包含哪些子模块以及插件有哪些。不过，要注意的是，如果依赖只在某个子项目中使用，则可以在子项目的 pom.xml 中直接引入，防止父 pom 的过于臃肿。

如下图所示，Dubbo 项目就被分成了多个子模块比如 dubbo-common（公共逻辑模块）、dubbo-remoting（远程通讯模块）、dubbo-rpc（远程调用模块）。



## Gradle

## Git

## Docker

## IDEA

# 框架

## Spring

## SpringBoot

## 常用注解

## MyBatis

## Netty

# 系统设计

## 基础设计

## 安全设计

## 设计模式

## Java定时任务

## Web实时消息推送

# 分布式

## 理论&算法&协议

## API网关

## 分布式ID

## 分布式锁

## RPC

## Zookeeper

## 分布式事务

## 分布式事务配置中心

# 高性能

## CDN

## 负载均衡

## 数据库优化

## 消息队列

# 高可用

## 系统设计

## 冗余设计

## 服务限流

## 熔断降级

## 超时重试

## 性能测试