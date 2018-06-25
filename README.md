## 注释
#### 1. 文案
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
* ###### 方法描述与左侧的 * 号之间保留一个空格，与参数说明之间保留一个空行
```Java
/**
 * good comment
 *
 * @param id
 * @return
 */

/**
 *bad comment
 * @param id
 * @return
 */
```

## Java
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
```java
// good
Map<Integer, String> idAndName;

// good
Map<Integer, Set<Student>> classIdAndStudents;

// bad
Map<Integer, String> map;
```

#### 3. 在实例化集合的同时为其指定合适的初始容量，以避免在后续添加元素的过程中发生不必要的扩容
* ###### List：需要存储多少个元素，就将初始容量设置为多少
```java
final int maxNumber = 20;

// good
List<Integer> numbers = new ArrayList<Integer>(maxNumber);
// bad
// List<Integer> numbers = new ArrayList<Integer>();

for (int i = 0; i < maxNumber; i++) {
    students.add(i);
}
```
* ###### Map：Map 的初始容量计算规则为：【K-V 对数 / 0.75，得到的商数如果为偶数，那么直接使用该数；如果为奇数，那么向上取该数最接近的偶数】，所以，当 Map 里需要存储 3 对 K-V 时，容量要指定为 4
```java
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
```java
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
```java
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