#  JavaScript 实现继承方式

![prototype](https://res.cloudinary.com/drvm6gm5e/image/upload/v1731553646/prototype_itsrcr.png)

**父类**

```js
function Person (name) {
    this.name = name
    this.say = function () {
        console.log(this.name)
    }
}

Person.prototype.sleep = function () {
    console.log(this.name + 'is sleep')
}
```



##  原型链继承

**将父类的实例作为子类的原型**

```js
function Person (name) {
    this.name = name
    this.say = function () {
        console.log(this.name)
    }
}

Person.prototype.sleep = function () {
    console.log(this.name + 'is sleep')
}

function Student (name) {
    this.name = name
}

Student.prototype = new Person() // 原型链继承
Student.prototype.constructor = Student // 保证自身原型链完整闭环

const stu = new Student('王林')
```

**优点**

- 简单以实现。
- 父类的原型方法/原型属性，子类都能访问。

**缺点**

- 创建子类实例时，不能向父类构造函数传参。
- 无法实现多继承。
- 所有的子类实例都会共享父类实例的属性，并且修改后会影响所有子类的实例。



##  借用构造函数继承

**在子类中执行父类的构造函数，通过 `call`函数修改 `this` 指向，把父类的实例属性复制到子类实例**

```js
function WebsiteMaster (site) {
    this.site = site
}

function Student (name, site, grade) {
    Person.call(this, name)
    WebsiteMaster(this, site)
    this.grade = grade
}

const stu = new Student('王林', 3, 'https://wanglin.king.com')
```

**优点**

- 创建子类实例时，可以向父类传递参数
- 可以实现多继承（call 多个对象）
- 解决子类实例共享父类引用属性问题

**缺点**

- 方法在构造函数中定义，无法服用。（call 相当于复制了一份）

- 只能击沉父类的实例属性，不能继承原型的属性和方法。

- 实例并不是父类的实例，而只是子类的实例。

  ```js
  stu instanceof Student   // true
  stu instanceof Person    // false
  ```



##  原型式继承

为父类实例添加属性、方法，作为子类实例。

> 借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型。
>
> ```js
> function object(o) {
>     function F() {}
>     F.prototype = o
>     return new F()
> }
> ```

**继承方式**

```js
function object (o) {
    function F() {}
    F.prototype = o
    return new F()
}

const person = new Person('王林')
const student = object(person)
student.grade = 3

```

**优点**

- 从已有对象衍生新对象，类似于对象的复制。（es5 内置 Object.create() 函数）

**缺点**

- 父类属性会被共享。
- 无法实现多继承。
- 代码无法复用。



##  寄生式继承

为父类实例添加属性、方法，作为子类的实例。（原理和原型式继承一样）

```js
function object(o) {
    function F(){}
    F.prototype = o
    return new F()
}

function Student(name, grade) {
    const student = object(new Person(name))
    
    student.grade = grade
    return student
}

const stu = new Student('王林', 3)
```



##  组合继承

原型链继承 + 借用构造函数继承

```js
function WebsiteMaster(site) {
    this.site = site
}

function Student (name, site, grade) {
    Person.call(this, name)
    WebsiteMaster(this, site)
    this.grade = grade
}

Student.prototype = new Person()
Student.prototype.constructor = Student

const stu = new Student('wanglin', 3, 'https;//wanglin.king.com')
```

**优点**

- 可以继承实例属性、方法，也可以继承原型属性和方法
- 可传参、可复用
- 实例即是子类的实例，也是父类的实例

**缺点**

- 调用了两次父类构造函数，耗内存。
- 需要修复构造函数指向



##  寄生组合式继承

通过 `Object.create()`来代替给子类原型赋值的过程，解决两次调用父类构造函数的问题

```js
function WebsiteMaster(site) {
    this.site = site
}

function Student (name, site, grade) {
    Person.call(this, name)
    WebsiteMaster(this, site)
    this.grade = grade
}

Student.prototype = Object.create(Person.prototype)  // new Person()
Student.prototype.constructor = Student

const stu = new Student('wanglin', 3, 'https;//wanglin.king.com')
```



###  最后的疑问

**提出疑问**：为什么不可以直接把父类原型赋值给子类原型呢?

```js
Student.prototype = Person.prototype
```



**因为直接赋值的话，就是引用关系了，如果修改子类原型上的值，则会直接影响父类原型。**

> `Object.create()` 方法创建一个新对象，使现有对象来提供新创建的对象的 `__proto__`。
>
> 所以，修改新对象上原型方法，并不会影响原有对象的原型。



参考链接：

- [JavaScript实现继承的六种方式](https://developer.aliyun.com/article/978006)
- [js中的6种继承方式](https://juejin.cn/post/7019185008527015949)