## 原码、反码和补码

> 原码，反码，补码的产生过程，就是为了解决，计算机做减法和引入符号位（正号和负号）的问题。

### 原码

> *原码：*是最简单的机器数表示法。用最高位表示符号位，‘1’表示负号，‘0’表示正号。其他位存放该数的二进制的绝对值。

### 反码

>反码：正数的反码还是等于原码；负数的反码就是他的原码除符号位外，按位取反。

### 补码

>补码：正数的补码等于他的原码 负数的补码等于反码+1。 （这只是一种算补码的方式）

### 移码

> 移码（又叫增码）是符号位取反的[补码](https://baike.baidu.com/item/补码/6854613)，一般用指数的移码减去1来做[浮点数](https://baike.baidu.com/item/浮点数/6162520)的[阶码](https://baike.baidu.com/item/阶码/7798285)，引入的目的是为了保证浮点数的[机器零](https://baike.baidu.com/item/机器零/2211171)为全0。

### 计算机加减流程及原理

> 计算机只有加法器 -> 正数和负数的加减运算 ->转化为补码进行加法运算 ->补码运算的原理在于取负数的同余数进行加法运算

### 补码运算规则

>  [原码] + [原码] -> [补码]+[补码] = [补码] -> [原码]（结果）

## 位运算核心知识点

- 位运算操作的是整数
- 计算机中，整数以机器数补码的形式存储
- 位运算操作的是整数的二进制补码

### 位运算

- 位运算只可以处理整数类型(byte、short、int、long)

- 位运算操作的是整数的二进制补码

- 位运算对byte和short进行移位操作时，在移位之前，会被转换成int类型，得到的结果也是int类型

#### 位运算规则

| 位运算符 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| `&`      | 且                                                           |
| `|`      | 或                                                           |
| `^`      | 异或                                                         |
| `～`     | 按位取反                                                     |
| `<<`     | 左移运算符，符号位不变，右侧低位补0，左侧高位舍弃。向左移动指定位数，一般情况下每移动一位都会乘以2(除符号位之外，最高位不为1的情况下，高位为1时，超过了位数限制) |
| `>>`     | 右移运算符，符号位不变，右侧低位舍弃，左侧高位正数补0，负数补1。向右移动指定位数，一般情况下每移动一位都是除以2（低位为1时，会丢失精度） |
| `>>>`    | 按位右移补零操作符，无论正负，右侧低位舍弃，左侧高位插入0    |

> 位运算符操作的都是补码，尤其是负数移位操作，应该深刻认识操作的是负数的二进制补码！！！

###   JAVA基本类型的默认值和取值范围

|         | 默认值    | 存储需求（字节） | 取值范围     | 示例               |
| ------- | --------- | ---------------- | ------------ | ------------------ |
| byte    | 0         | 1                | -2^7—2^7-1   | byte b=10;         |
| short   | 0         | 2                | -2^15—2^15-1 | short s=10;        |
| int     | 0         | 4                | -2^31—2^31-1 | int i=10;          |
| long    | 0         | 8                | -2^63—2^63-1 | long o=10L;        |
| float   | 0.0f      | 4                | IEEE754      | float f=10.0F      |
| double  | 0.0d      | 8                | IEEE754      | double d=10.0;     |
| boolean | false     | 1                | true\false   | boolean flag=true; |
| char    | ‘ \u0000′ | 2                | 0—2^16-1     | char c=’c’ ;       |

### int类型和byte数组的相互转化

```JAVA
/** 
 * int到byte[] 
 * @param i 
 * @return 
 */  
public static byte[] intToByteArray(int i) {  
    byte[] result = new byte[4];  
    // 由高位到低位  
    result[0] = (byte) ((i >> 24) & 0xFF);  
    result[1] = (byte) ((i >> 16) & 0xFF);  
    result[2] = (byte) ((i >> 8) & 0xFF);  
    result[3] = (byte) (i & 0xFF);  
    return result;  
}  
  
/** 
 * byte[]转int 
 * @param bytes 
 * @return 
 */  
public static int byteArrayToInt(byte[] bytes) {  
    int value = 0;  
    // 由高位到低位  
    for (int i = 0; i < 4; i++) {  
        int shift = (4 - 1 - i) * 8;  
        value += (bytes[i] & 0x000000FF) << shift;// 往高位游  
    }  
    return value;  
}  
  
public static void main(String[] args) {  
    byte[] b = intToByteArray(128);  
    System.out.println(Arrays.toString(b));  
      
    int i = byteArrayToInt(b);  
    System.out.println(i);  
}
```

### 位运算代码

```JAVA
	public static void testBitOperation() {
		//数值在机器中以补码的形式存储，位运算规则操作的是数值的二进制补码
		int positive = 2;  //0... 0000 0010(原码) 0... 0000 0010(反码)  0... 0000 0010(补码)
		int negative = -2; //1... 0000 0010(原码) 1... 1111 1101(反码)  1... 1111 1110(补码)
	       
		System.out.println("positive:" + positive);
		System.out.println("negative:" + negative);
		System.out.println("&:" + (positive & negative));//0... 0000 0010(补码)  0... 0000 0010(反码)  0... 0000 0010(原码) 
		System.out.println("|:" + (positive | negative));//1... 1111 1110(补码)  1... 1111 1101(反码)  1... 0000 0010(原码)
		System.out.println("~positive:" + ~positive);//1... 1111 1101(补码) 1... 1111 1100(反码)  1... 0000 0011(原码)
		System.out.println("~negative:" + ~negative);//0... 0000 0001(补码) 0... 0000 0001(反码)  0... 0000 0001(原码)   
		System.out.println("^:" + (positive ^ negative));//1... 1111 1100(补码)  1... 1111 1011(反码)  1... 0000 0100(原码)
		
		System.out.println("positive << 3:" + (positive << 3));//0... 0001 0000(补码) 0... 0001 0000(反码) 0... 0001 0000(原码)
		System.out.println("negative << 3:" + (negative << 3));//1... 1111 0000(补码) 1... 1110 1111(反码) 1... 0001 0000(原码)
		
		System.out.println("positive >> 3:" + (positive >> 3));//0... 0000 0000(补码) 0... 0000 0000(反码) 0... 0000 0000(原码)
		System.out.println("negative >> 3:" + (negative >> 3));//1... 1111 1111(补码) 1... 1111 1110(反码) 1... 0000 0001(原码)
		
		
		System.out.println("positive >>> 3:" + (positive >>> 3));//0... 0000 0000(补码) 0... 0000 0000(反码) 0... 0000 0000(原码)
		System.out.println("negative >>> 3:" + (negative >>> 3));//0001 1111 1111 1111 1111 1111 1111 1111(补码\反码\原码) 
		System.out.println("negative >>> 3:" + Integer.parseInt("00011111111111111111111111111111", 2));
	}


/**output**/
positive:2
negative:-2
&:2
|:-2
~positive:-3
~negative:1
^:-4
positive << 3:16
negative << 3:-16
positive >> 3:0
negative >> 3:-1
positive >>> 3:0
negative >>> 3:536870911
negative >>> 3:536870911
```

### float类型和double类型

![](https://iknow-pic.cdn.bcebos.com/962bd40735fae6cd520228d60bb30f2443a70fd6?x-bce-process=image/resize,m_lfit,w_600,h_800,limit_1)

|        | 符号位 | 阶码 | 尾数 | 长度 |
| ------ | ------ | ---- | ---- | ---- |
| float  | 1      | 8    | 23   | 32   |
| double | 1      | 11   | 52   | 64   |

#### float和double计算方式

![](https://s5.51cto.com/wyfs02/M02/87/C8/wKioL1fh6-LwibpzAAB7GmVzFbY425.png)

#### float和double取值范围和精度问题

- **float和double的取值范围是由指数的位数来决定的**
- float的指数范围为-2^7~2^7-1(即-128~127)，而double的指数范围为-2^10~2^10-1(即-1024~1023)
- float的范围为-2^128 ~ +2^128（-3.40E+38 ~ +3.40E+38）
- double的范围为-2^1024 ~ +2^1024（-1.79E+308 ~ +1.79E+308）
- **float和double的精度是由尾数的位数来决定的**
- float：2^23 = 8388608，一共七位，这意味着最多能有7位有效数字，即float的精度为6~7位有效数字；
- double：2^52 = 4503599627370496，一共16位，同理，double的精度为15~16位。

#### Java中数值变量的声明：

- 二进制变量的声明以0b为前缀
- 八进制变量的声明以0为前缀十六进制变量的声明以0x为前缀
- 十六进制变量的声明以0x为前缀

```java
    public static void main(String[] args)
    {
        int a = 0b11;   //声明二进制变量
        int b = 011;    //声明八进制变量
        int c = 11;     //声明十进制变量
        int d = 0x11;   //声明十六进制变量
        System.out.println("a："+a); //3
        System.out.println("b："+b); //9
        System.out.println("c："+c); //11
        System.out.println("d："+d); //17
    }

```

