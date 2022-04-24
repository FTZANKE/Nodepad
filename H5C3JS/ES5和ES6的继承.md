# [详解ES5和ES6的继承](https://www.cnblogs.com/annika/p/9073572.html)

#### ES5继承

构造函数、原型和实例的关系：每一个构造函数都有一个原型对象，每一个原型对象都有一个指向构造函数的指针，而每一个实例都包含一个指向原型对象的内部指针，

- **原型链实现继承**

  - 基本思想：利用原型让一个引用类型继承另一个引用类型的属性和方法，即让原型对象等于另一个类型的实例
  - 基本模式：

  ```js
  function SuperType(){
  	this.property = true;
  }
  SuperType.prototype.getSuperValue = function(){
  	return this.property;
  };
  function SubType(){
  	this.subproperty = false;
  }
  \\继承了SuperType
  SubType.prototype = new SuperType();
  
  SubType.prototype.getSubValue = function(){         
  	return this.subproperty;
  };
  var instance = new SubType();
  alert(instance.getSuperValue());  \\true
  ```

  - 最终结果：instance指向SubType的原型，SubType的原型又指向SuperType的原型，getSuperValue()方法任然在SuperType.prototype中，但property则位于SubType.prototype中，这是因为property是一个实例属性，而getSuperValue是一个原型方法。此时，instance.constructor指向的是SuperType。
  - 注意事项：

    - 别忘记默认的原型，所有的引用类型都继承自Object，所有函数的默认原型都是Object的实例，因此默认原型里都有一个指针，指向object.prototype
    - 谨慎地定义方法，给原型添加方法的代码一定要放在替换原型的语句之后，不能使用对象字面量添加原型方法，这样会重写原型链
  - 原型链继承的问题
  - - 最主要的问题来自包含引用类型值的原型，它会被所有实例共享
    - 第二个问题是，创造子类型的实例时，不能向超类型的构造函数中传递参数
- **借用构造函数**

  - 基本思想：在子类型构造函数的内部调用超类型构造函数，通过使用apply()和call()方法可以在将来新创建的对象上执行构造函数

    ```
     1 function SuperType(){
     2     this.colors = ["red","blue","green"];
     3 }
     4 
     5 function SubType(){
     6     \\借调了超类型的构造函数
     7     SuperType.call(this);
     8 }
     9 
    10 var instance1 = new SubType();
    11 \\["red","blue","green","black"]
    12 instance1.colors.push("black");
    13 console.log(instance1.colors);
    14 
    15 var instance2 = new SubType();
    16 \\["red","blue","green"]
    17 console.log(instance2.colors);
    ```

    通过call或者apply方法，我们实际上是在将来新创建的SubType实例的环境下调用了SuperType构造函数。这样一来，就会在新SubType对象上执行SuperType函数中定义的所有对象初始化代码，因此，每一个SubType的实例都会有自己的colors对象的副本
  - 优势：

    - 传递参数

    ```
     1 function Supertype(name){
     2     this.name = name;
     3 }
     4 
     5 function Subtype(){
     6     Supertype.call(this,'Annika');
     7     this.age  = 21;
     8 }
     9 
    10 var instance = new Subtype;
    11 console.log(instance.name);  \\Annika
    12 console.log(instance.age);   \\29
    ```
  - 缺点：

    - 方法都在构造函数中定义，函数无法复用
    - 在超类型中定义的方法，子类型不可见，结果所有类型都只能使用构造函数模式
- **组合继承**

  - 基本思想：将原型链和借用构造函数技术组合到一起。使用原型链实现对原型属性和方法的继承，用借用构造函数模式实现对实例属性的继承。这样既通过在原型上定义方法实现了函数复用，又能保证每个实例都有自己的属性

    ```js
     function Supertype(name){
         this.name = name;
         this.colors = ["red","green","blue"];
     }
     
     Supertype.prototype.sayName = function(){
         console.log(this.name);
     };
     
     function Subtype(name,age){
         \\继承属性
         Supertype.call(this,name);
         this.age  = age;
     }
     
     \\继承方法
     Subtype.prototype = new Supertype();
     Subtype.prototype.constructor = Subtype;
     Subtype.prototype.sayAge = function(){
         console.log(this.age);
     };
     
     var instance1 = new Subtype('Annika',21);
     instance1.colors.push("black");
     \\["red", "green", "blue", "black"]
     console.log(instance1.colors); 
     instance1.sayName(); \\Annika
     instance1.sayAge();  \\21
     
     var instance2 = new Subtype('Anna',22);
     \\["red", "green", "blue"]
     console.log(instance2.colors);
     instance2.sayName();   \\Anna
     instance2.sayAge();    \\22
    ```
  - 缺点：无论在什么情况下，都会调用两次超类型构造函数，一次是在创建子类型原型的时候，一次是在子类型构造函数的内部
- **原型式继承**

  - 基本思想：不用严格意义上的构造函数，借助原型可以根据已有的对象创建新对象，还不必因此创建自定义类型，因此最初有如下函数：

    ```js
    function object(o){
        function F(){}
        F.prototype = o;
        return new F();
     }
    ```

    从本质上讲，object()对传入其中的对象执行了一次浅复制

    ```js
    var person = {
        name:'Annika',
        friendes:['Alice','Joyce']
    };
    
    var anotherPerson = object(person);
    anotherPerson.name = 'Greg';
    anotherPerson.friendes.push('Rob');
    
    var yetAnotherPerson = object(person);
    yetAnotherPerson.name = 'Linda';
    yetAnotherPerson.friendes.push('Sophia');
    
    14 console.log(person.friends);   //['Alice','Joyce','Rob','Sophia'] 
    ```
    
  在这个例子中，实际上相当于创建了person的两个副本。
  - ES5新增Object.create规范了原型式继承，接收两个参数，一个用作新对象原型的对象和（可选的）一个为新对象定义额外属性的对象，在传入一个参数的情况下，Object.create()和object()行为相同。
  
    ```js
    var person = {
        name:'Annika',
        friendes:['Alice','Joyce']
    };
    
    var anotherPerson = object.create(person,{
        name:{
            value:"Greg"
        }
    });
    
    //用这种方法指定的任何属性都会覆盖掉原型对象上的同名属性
    console.log(anotherPerson.name);   //Greg
    ```
  - 用处：创造两个相似的对象，但是包含引用类型的值的属性始终会共享响应的值
- **寄生式继承**

  - 基本思想：寄生式继承是与原型式继承紧密相关的一种思路，它创造一个仅用于封装继承过程的函数，在函数内部以某种方式增强对象，最后再返回对象。

    ```js
    function createAnother(original) {
        //通过调用函数创建一个新对象
        var clone = object(original);
        // 以某种方式来增强对象
        clone.sayHi = fuuction(){
            alert("Hi");
        };
        \\返回这个对象
        return clone;
    }
    ```
  - 缺点：使用寄生式继承来为对象添加函数，会因为做不到函数复用而降低效率，这个与构造函数模式类似
- **寄生组合式继承**

  - 基本思想：通过借用构造函数来继承属性，通过原型链的混成形式来继承方法，不必为了指定子类型的原型而调用超类型的构造函数，只需要超类型的一个副本。本质上，就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型

    ```js
    function inheritPrototype(Subtype,supertype){
        var prototype = object(supertype);   \\创建对象
        prototype.constructor = subtype;     \\增强对象
        subtype.prototype = prototype;       \\指定对象
    }
    ```

    因此，前面的例子可以改为如下的形式

    ```js
    function Supertype(name){
        this.name = name;
        this.colors = ["red","green","blue"];
    }
    
    Supertype.prototype.sayName = function(){
        console.log(this.name);
    };
    
    function Subtype(name,age){
        \\继承属性
        Supertype.call(this,name);
          this.age  = age;
         }
    
         \\继承方法
         inheritPrototype(Subtype,Supertype);
    
    Subtype.prototype.sayAge = function(){
        console.log(this.age);
    };
    ```
  - 优点：只调用了一次supertype构造函数，因此避免在subtype.prototype上创建不必要的，多余的属性，与此同时，原型链还能保持不变，还能正常使用instanceof 和isPrototypeOf()，因此，寄生组合式继承被认为是引用类型最理想的继承范式。

### 总结：

ES5的继承可以用下图来概括：

![img](https://images2018.cnblogs.com/blog/1290444/201805/1290444-20180522190257058-425523963.png)

#### ES6继承

es6的继承主要要注意的是class的继承。

- 基本用法：Class之间通过使用extends关键字，这比通过修改原型链实现继承，要方便清晰很多

```js
class Colorpoint extends Point {
    constructor(x,y,color){
        super(x,y); //调用父类的constructor(x,y)
        this.color = color
    }
    toString(){
        //调用父类的方法
        return this.color + ' ' + super.toString(); 
    }
}
```

子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工，如果不调用super方法，子类就得不到this对象。因此，只有调用super之后，才可以使用this关键字。

- prototype 和__proto__

一个继承语句同时存在两条继承链：一条实现属性继承，一条实现方法的继承

```js
class A extends B{}
A.__proto__ === B;  //继承属性
A.prototype.__proto__ == B.prototype;//继承方法
```

### 总结：

ES6的继承可以用下图来概括：

![img](https://images2018.cnblogs.com/blog/1290444/201805/1290444-20180522190423509-1503553581.png)
