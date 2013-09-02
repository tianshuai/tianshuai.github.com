---
layout: post
title: "ruby之面向对象"
date: 2013-08-30 17:05
comments: true
categories: ruby
---

####面向对象的三个基本特征是：封装、继承、多态。
-> ![o_OOBase.gif](/images/ruby/o_OOBase.gif) <-

####封装
*封装，保证对象自身数据的完整性、安全性*

####继承
*建立类之间的关系，实现代码复用、方便系统的扩展*
 
* 继承概念的实现方式有三类：实现继承、接口继承和可视继承。  
Ø	实现继承是指使用基类的属性和方法而无需额外编码的能力；  
Ø	接口继承是指仅使用属性和方法的名称、但是子类必须提供实现的能力；  
Ø	可视继承是指子窗体（类）使用基窗体（类）的外观和实现代码的能力。  
* 在考虑使用继承时，有一点需要注意，那就是两个类之间的关系应该是“属于”关系。例如，Employee 是一个人，Manager 也是一个人，因此这两个类都可以继承 Person 类。但是 Leg 类却不能继承 Person 类，因为腿并不是一个人。  
* 抽象类仅定义将由子类创建的一般属性和方法，创建抽象类时，请使用关键字 Interface 而不是 Class。  
* OO开发范式大致为：划分对象→抽象类→将类组织成为层次化结构(继承和合成) →用类与实例进行设计和实现几个阶段。

####多态
*相同的方法调用可实现不同的实现方式*

* 实现多态，有二种方式，覆盖，重载。  
* 覆盖，是指子类重新定义父类的虚函数的做法。  
* 重载，是指允许存在多个同名函数，而这些函数的参数表不同（或许参数个数不同，或许参数类型不同，或许两者都不同）。
<!--more-->

&emsp;&emsp;其实，重载的概念并不属于“面向对象编程”，重载的实现是：编译器根据函数不同的参数表，对同名函数的名称做修饰，然后这些同名函数就成了不同的函数（至少对于编译器来说是这样的）。如，有两个同名函数：function func(p:integer):integer;和function func(p:string):integer;。那么编译器做过修饰后的函数名称可能是这样的：int_func、str_func。对于这两个函数的调用，在编译器间就已经确定了，是静态的（记住：是静态）。也就是说，它们的地址在编译期就绑定了（早绑定），因此，重载和多态无关！真正和多态相关的是“覆盖”。当子类重新定义了父类的虚函数后，父类指针根据赋给它的不同的子类指针，动态（记住：是动态！）的调用属于子类的该函数，这样的函数调用在编译期间是无法确定的（调用的子类的虚函数的地址无法给出）。因此，这样的函数地址是在运行期绑定的（晚邦定）。结论就是：重载只是一种语言特性，与多态无关，与面向对象也无关！引用一句Bruce Eckel的话：“不要犯傻，如果它不是晚邦定，它就不是多态。”  

&emsp;&emsp;那么，多态的作用是什么呢？我们知道，封装可以隐藏实现细节，使得代码模块化；继承可以扩展已存在的代码模块（类）；它们的目的都是为了——代码重用。而多态则是为了实现另一个目的——接口重用！多态的作用，就是为了类在继承和派生的时候，保证使用“家谱”中任一类的实例的某一属性时的正确调用。
其实，重载的概念并不属于“面向对象编程”，重载的实现是：编译器根据函数不同的参数表，对同名函数的名称做修饰，然后这些同名函数就成了不同的函数（至少对于编译器来说是这样的）。如，有两个同名函数：function func(p:integer):integer;和function func(p:string):integer;。那么编译器做过修饰后的函数名称可能是这样的：int_func、str_func。对于这两个函数的调用，在编译器间就已经确定了，是静态的（记住：是静态）。也就是说，它们的地址在编译期就绑定了（早绑定），因此，重载和多态无关！真正和多态相关的是“覆盖”。当子类重新定义了父类的虚函数后，父类指针根据赋给它的不同的子类指针，动态（记住：是动态！）的调用属于子类的该函数，这样的函数调用在编译期间是无法确定的（调用的子类的虚函数的地址无法给出）。因此，这样的函数地址是在运行期绑定的（晚邦定）。结论就是：重载只是一种语言特性，与多态无关，与面向对象也无关！引用一句Bruce Eckel的话：“不要犯傻，如果它不是晚邦定，它就不是多态。”
那么，多态的作用是什么呢？我们知道，封装可以隐藏实现细节，使得代码模块化；继承可以扩展已存在的代码模块（类）；它们的目的都是为了——代码重用。而多态则是为了实现另一个目的——接口重用！多态的作用，就是为了类在继承和派生的时候，保证使用“家谱”中任一类的实例的某一属性时的正确调用。

***

###ruby中的面向对象

####封装
* 封装没什么可说的

####继承与多态

```ruby

class Person 
  def initialize( name,age=18 ) 
    @name = name 
    @age = age 
    @motherland = "China" 
  end 

  def talk 
    puts "my name is "+@name+", age is "+@age.to_s 
    if @motherland == "China" 
      puts "I am a Chinese." 
    else 
      puts "I am a foreigner." 
    end 
  end 

  attr_writer :motherland 

end 

class Student < Person 
  def talk 
    puts "I am a student. my name is "+@name+", age is "+@age.to_s 
  end 
end 

class Worker < Person  
  def talk 
    puts "I am a worker. my name is "+@name+", age is "+@age.to_s 
  end 
end 

p3=Student.new("kaichuan",25);p3.talk 

p4=Student.new("Ben");p4.talk 


p5=Worker.new("kaichuan",30);p5.talk 
p6=Worker.new("Ben");p6.talk 

```
&emsp;&emsp;Worker 类与 Student 类同样继承自 Person 类，亲缘关系是兄弟，当他们 talk 
时，能准确表明自己身份，因为他们都重写了各自的 talk方法。 不同的子类继承一个父类，不仅子类和父类的行为有变异，而且子类彼此的行为也有差异，这就是多态。 

&emsp;&emsp;用“ < ”表示 Student 类是 Person 类的子类。Person 类的一切，Student 类都能 
继承。但是 Student 类重写了 talk 方法，所以我们看到了不同的运行结果。子类继 
承父类的时候，除了重写方法；也可以添加一些新的方法；或是增强父类的方法(用 
关键字 super指明)。 

&emsp;&emsp;Ruby语言只支持单继承，每一个类都只能有一个直接父类。这样避免了多继承的复杂度。但同时，Ruby提供了mixin的机制可以用来实现多继承。 

&emsp;&emsp;可以使用super关键字调用对象父类的方法，当super省略参数时，将使用当前方法的参数来进行调用。

```ruby
class Base 
  def meth(info) 
    puts "This is Base #{info}" 
  end 
end 

class Derived < Base 
  def meth(info) 
    puts "This is derived #{info}" 
    super 
  end 
end 

obj1 = Derived.new 

obj1.meth("test") 

执行结果为： 

This is derived test 

This is Base test 

```
如果传入的参数被修改再调用super的话，那么将会使用使用修改后的值。 
```ruby

class Base 
  def meth(info) 
    puts "This is Base #{info}" 
  end 
end 

class Derived < Base 
  def meth(info) 
    puts "This is derived #{info}" 
    info = "over" 
    super 
  end 
end 

obj1 = Derived.new 

obj1.meth("test") 

```
执行结果为： 

```
This is derived test  
This is Base over
```

**ruby有重载吗？ **  
先看一个例子，实现一个类属性的可读写：
```ruby
class A  
  def para(a)  
    @para = a  
  end  
  def para  
    @para  
  end  
end  
a = A.new  
a.para(1)  
puts a.para  
```
有错： 

>ruby test.rb 
>test.rb:10:in 'para': wrong number of arguments (1 for 0) (ArgumentError) 
>from test.rb:10 
>Exit code: 1 

很明显的，ruby用para方法覆盖了para(a)   
稍微把para(a)和para方法定义的位置对调一下：

```ruby
class A  
  def para  
    @para  
  end  
  def para(a)  
    @para = a  
  end  
end  
a = A.new  
a.para(1)  
puts a.para 
```

同样有问题： 
>ruby test.rb 
>test.rb:11:in `para': wrong number of arguments (0 for 1) (ArgumentError) 
>from test.rb:11 
>Exit code: 1 

为什么会这样呢？ 
ruby支持可变参数，根本就没有必要定义两个同名的方法，只用在参数上面做文章就好了。

```ruby
class A  
  def para(*a)  
    @para = a.first if a.size == 1  
    @para  
  end  
end  
a = A.new  
a.para(2)  
puts a.para  
```
结果： 
>ruby test.rb 
>2 
>Exit code: 0 
难道这个就是java中的重载在ruby中的实现？ 

**那么重写呢？ **  
看例子： 
```ruby
class A  
  def ow  
    puts "Father"  
  end  
end  
class B < A  
  def ow  
    puts "I am!"  
  end  
end  
b = B.new  
b.ow  
```

结果： 
>ruby test.rb 
>I am! 
>Exit code: 0 

但是有个东西就有点纠结了！ 

```ruby
class A  
  def ow  
    puts "Father"  
  end  
end  
class B < A  
  def ow(name)  
    puts "I am!" + name  
  end  
end  
b = B.new  
b.ow("Jack")  
b.ow  
```
结果： 
>ruby test.rb 
>test.rb:13:in `ow': wrong number of arguments (0 for 1) (ArgumentError) 
>from test.rb:13 
>I am!Jack 
>Exit code: 1 

 在重载的时候时候也提到了，ruby是看"名字”来认识的，后面ow(name)方法已经把ow方法覆盖了！ 

***由上可知，Ruby语言，只有重写（override），没有其它语言具有的严格意义上的重载（overload）***

[参考-面向对象的三个基本特征（讲解）](http://www.cnitblog.com/Lily/archive/2013/01/03/6860.html)  
[参考－ruby的继承和多态](http://selfcontroller.iteye.com/blog/1286428)  
[参考－ruby 重载？ 重写？](http://dtzq01.iteye.com/blog/1010557)
