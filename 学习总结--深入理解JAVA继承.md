## 深入理解JAVA继承

##  特性

- 继承是将**父类对象包含在子类对象**中,父类对象中所有的一切皆在子类对象中
- 父类private字段和方法只是对子类不可见,实际**仍旧是存在**的
- 在子类中声明与父类同名的字段时,例name,此时子类中包含两个name字段:this.name,super.name
- 当**不重写父类方法时,调用的就是父类对象的方法**,其实操作的就是父类对象
- 若**重写父类方法,根据Java多态的特性,调用的是子类重写方法**,父类方法只有通过super指针调用
- **多态只对方法起作用,不对字段起作用**

![](.\img\SouthEast.png)

## 举例

###  实例1

```Java
//父类对象
public class Person {
	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
//子类对象
public class Teacher extends Person{}


public class Solution {
	public static void main(String[] args) {
		Teacher teacher = new Teacher();
		teacher.setName("zhangsan");
		System.out.println(teacher.getName());
	}
}
//输出
zhangsan
```

### 结论

- 子类对象包含父类对象private字段(debug查看内存可以查看到name字段)
- private字段对子类不可见(即this指针无法访问到)
- 不重写父类方法时,调用的是父类对象的get/set方法
- 可通过父类的get/set方法访问到父类对象的private字段

### 实例2

```Java
//父类对象
public class Person {
	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
//子类对象
public class Teacher extends Person{
	private String name;
	
	public String getName() {
		return name;
	}

	@Override
	public String toString() {
		return "Teacher.name = " + this.getName() + " Person.name = " + super.getName();
	}
}

public class Solution {
	public static void main(String[] args) {
		Person teacher = new Teacher();
		teacher.setName("zhangsan");
		System.out.println(teacher.getName());
		System.out.println(teacher.toString());
	}
}
//输出
null
Teacher.name = null  Person.name = zhangsan
```

### 结论

- 当在子类中声明与父类同名字段时,对象中包含两个name（debug查看内存可查看到）
- 此时可分为this.name和super.name两个字段
- 未重写set方法,故调用的时父类对象的set方法,对父类对象的name赋值
- 重写了get方法,根据Java多态特性,调用子类对象的get方法

### 实例3

```Java
//父类对象
public class Person {
	private String name;
	public int age = 38;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
//子类对象
public class Teacher extends Person{
	private String name;
	public int age = 26;
	
	public String getName() {
		return name;
	}

	@Override
	public String toString() {
		return "Teacher.name = " + this.getName() + "  Person.name = " + super.getName();
	}
}

public class Solution {
	public static void main(String[] args) {
		Teacher teacher = new Teacher();
		Person person = teacher;
		teacher.setName("zhangsan");
		System.out.println(teacher.getName());
		System.out.println(teacher.toString());
		
		System.out.println(teacher.age);
		System.out.println(person.age);
	}
}
//输出
null
Teacher.name = null  Person.name = zhangsan
26
38
```

### 结论

- 当在子类中声明与父类同名字段时,对象中包含两个name（debug查看内存可查看到）
- 此时可分为this.name和super.name两个字段
- 方法存在多态特性,字段不存在多态特性