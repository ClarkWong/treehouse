## 一、前言
以我的理解，代码规范可以分化为两个维度：
###### 1. 模板规范，即 IDE 可以帮我们实现的自动化规范，多关注于代码格式的调整。做好此项规范是职业素养最基本的体现，如果一位开发者连按动快捷键触发下 format 都做不到，那我不认为他产出的代码是能够信赖的。
###### 2. 人为规范，必须由开发者亲力亲为的人工规范，涉及到程序的性能细节、注释质量、类成员的顺序 / 命名、方法结构，乃至方法内空行等多个方面。

<br />

下面我们来整理下，单就模板规范而言，IDE 能以及不能帮我们做的事情。
#### Can do：
###### 1. 格式化代码
* ###### 调整大括号的位置
* ###### 调整类成员间的空行
* ###### 调整行首缩进
* ###### 调整行内空格
* ###### 清除行尾空格
###### 2. 清除冗余的 import 语句 / 调整现有的 import 顺序

#### Can't do：
###### 1. 格式化注释
###### *该分类下目前只整理出了这一项，经验证，如果不做额外配置，IntelliJ IDEA 不会对注释做重新排版（但是 Eclipse 可以），所以使用 IDEA 作为开发环境的同学在做好人为规范的同时还需要额外关注【注释排版】*

<br />

## 二、注释
#### 1. 文案规范
* ###### [中文文案排版指北](https://github.com/mzlogin/chinese-copywriting-guidelines)
* ###### [写给大家看的中文排版指南](https://zhuanlan.zhihu.com/p/20506092)

#### 2. 单行注释
* ###### 注释内容与左侧的双斜线之间保留一个空格
```Java
// good comment

//  bad comment

//bad comment
```

#### 3. 文档注释
* ###### 标准的文档注释写法为 /** ... */，一定要避免将其与多行注释混淆
```Java
/**
 * good comment
 */

/*
 * bad comment
 */
```
* ###### 方法描述与左侧的 * 号之间保留一个空格，与下方的参数 / 返回 / 异常说明之间保留一个空行
```Java
/**
 * good comment
 *
 * @param id
 * @return
 * @throws
 */

/**
 *bad comment
 * @param id
 * @return
 * @throws
 */
```

## 三、Java
#### 1. 秉持最小原则，为每一个方法设置合适的访问权限
* ###### 例如：仅在本类中使用的方法，一定置为私有

#### 2. 数组与集合的名称不必使用特定后缀去标明其类型
* ###### Array、List、Set 的命名统一使用标准的名词复数
```Java
// good
Student[] students;

// bad
Student[] studentArr;



// good
List<Student> students;

// bad
List<Student> studentList;



// good
Set<Student> students;

// bad
Set<Student> studentSet;
```
* ###### Map 的命名最好是关于 key 和 value 的描述
```Java
// good
Map<Integer, String> idAndName;

// good
Map<Integer, Set<Student>> classIdAndStudents;

// bad
Map<Integer, String> map;
```

#### 3. 在实例化集合的同时为其指定合适的初始容量，以避免在后续添加元素的过程中发生不必要的扩容
* ###### List：需要存储多少个元素，就将初始容量设置为多少
```Java
final int maxNumber = 20;

// good
List<Integer> numbers = new ArrayList<Integer>(maxNumber);
// bad
// List<Integer> numbers = new ArrayList<Integer>();

for (int i = 0; i < maxNumber; i++) {
    students.add(i);
}
```
* ###### Map：Map 的初始容量计算规则为：【预期的 K-V 对数 / 0.75，向上取商数最接近 2ⁿ】，所以，当 Map 里需要存储 3 对 K-V 时，容量应指定为 4
```Java
// good
Map<Integer, String> idAndName = new HashMap<Integer, String>(4);
// bad
// Map<Integer, String> idAndName = new HashMap<Integer, String>(3);
// bad
// Map<Integer, String> idAndName = new HashMap<Integer, String>();

idAndName.put(1000, "Jack");
idAndName.put(1001, "Lisa");
idAndName.put(1002, "David");
```
* ###### Set：Set 其实是一种另类的 Map，其初始容量的计算规则与 Map 相同
```Java
// good
Set<String> names = new HashSet<String>(4);
// bad
// Set<String> names = new HashSet<String>(3);
// bad
// Set<String> names = new HashSet<String>();

names.add("Jack");
names.add("Lisa");
names.add("David");
```

#### 4. 如果某个方法的返回结果将在下文中被多次使用，则建议将其缓存为常量，避免重复方法调用造成无谓的性能损耗
```Java
boolean isContains = false;

// good
final String name = student.getName();
if (StringUtils.isNotBlank(name)) {
    isContains = name.contains("Jack");
}

// bad
if (StringUtils.isNotBlank(student.getName())) {
    isContains = student.getName().contains("Jack");
}
```