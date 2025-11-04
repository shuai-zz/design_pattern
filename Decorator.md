# 装饰模式

## just show you the code

一个装扮游戏，人物能够自由穿服装

```java
// Person类
class Person{
    private String name; 
    public Person(){}
    public Person(String name){
        this.name=name;
    }
    
    public void show(){
        System.out.println("装扮的"+name);
    }
}
```

```java
//服饰类
class Finery extends Person{
    protected Person component;

    //打扮
    public void Decorate(Person component){
        this.component=component
    }

    @Override
    public void show(){
        if(component!=null){
            component.show();
        }
    }
}
```

```java
//具体服饰类
class TShirts extends Finery{
    @Override
    public void show(){
        System.out.println("T恤");
        super.show();
    }
}

class BigTrousers extends Finery{
    @Override
    public void show(){
        System.out.println("垮裤");
        super.show();
    }
}
//其余类似，省略
```

```java
//主函数
public static void main(String[] args){
    Person xc=new Person("小菜");

    System.out.println("第一种装扮");
    Sneakers nike=new Sneakers();
    BigTrouser jeans=new BigTrousers();
    TShirts whiteT=new Tshirts();

    //装饰过程
    nike.Decorate(xc);
    jeans.Decorate(nike);
    whiteT.Decorate(jeans);
    whiteT.show();

    System.out.println("第二种装扮");
    LeatherShoes derbys=new LeatherShoes();
    Tie skinny=new Tie();
    Suit italian=new Suit();

    //装饰过程
    derbys.Decorate(xc);
    skinny.Decorate(derbys);
    italian.Decorate(skinny);
    italian.show();
}
```

## 该项目的UML图如下

![Decorator_dressing](/picture/Decorator_dressing.png)

## 为什么不写成这样

```java
//Person类同上
class Person{
    private String name; 
    public Person(){}
    public Person(String name){
        this.name=name;
    }
    
    public void show(){
        System.out.println("装扮的"+name);
    }
}

//抽象服饰类
abstract class Finery{
    public void show();
}

//具体服饰类
class TShirts extends Finery{
    @Override
    public void show(){
        System.out.println("T恤");
    }
}
class BigTrousers extends Finery{
    @Override
    public void show(){
        System.out.println("垮裤");
    }
}
//其他类似，省略
```

```java
//主函数
public static void main(String[] args){ 
    Person xc=new Person("小菜");
    System.out.println("第一种装扮");
    Finery nike=new Sneakers();
    Finery jeans=new BigTrouser();
    Finery whiteT=new Tshirts();

    nike.show();
    jeans.show();
    whiteT.show();
    xc.show();
    //剩余同上
}
```

相较于这种方式，装饰模式更加灵活

## 装饰模式详解

### 先看类图

![Decorator_pattern](/picture/Decorator_pattern.png)

### 代码实现

```java
//Component类
abstract class Component{
    public abstract void operation();
}
```

```java
//ConcreteComponent类
class ConcreteComponent extends Component{
    @Override
    public void operation(){
        System.out.println("具体组件功能");
    }
}
```

```java
//Decorator类
abstract class Decorator extends Component{
    protected Component component;
    //设置Component
    public void setComponent(Component component){
        this.component=component;
    }
    //重写operation(),实际执行的是component的operation()
    @Override
    public void operation(){}
    if(component!=null){
        component.operation();
    }
}
```

```java
//ConcreteDecoratorA类
class ConcreteDecoratorA extends Decorator{
    //本类独有功能，以区别ConcreteDecoratorB
    private String addedState;

    @Override
    public void operation(){
        /**先执行原Component的operation()，再执行本类的功能，如addedState，相当于对原Component进行了装饰**/
        super.operation();
        addedState="New State";
        System.out.println("具体装饰对象A的操作");
    }
}

//ConcreteDecoratorB类
class ConcreteDecoratorB extends Decorator{
    @Override
    public void operation(){
        /**先执行原Component的operation()，再执行本类的功能，如AddedBehavior()，相当于对原Component进行了装饰**/
        
        super.operation();
        AddedBehavior();
        System.out.println("具体装饰对象B的操作");
    }
    
    //本类都有方法，以区别ConcreteDecoratorA
    private void AddedBehavior(){}
}
```

```java
//客户端代码
public static void main(String[] args){
    ConcreteComponent component=new ConcreteComponent();
    ConcreteDecoratorA decoratorA=new ConcreteDecoratorA();
    ConcreteDecoratorB decoratorB=new ConcreteDecoratorB();

    decoratorA.setComponent(component);
    decoratorB.setComponent(decoratorA);
    decoratorB.operation();
    /**
     * 装饰的方法是：首先用 ConcreteComponent 实例化对象 component，然后用 ConcreteDecoratorA 的实例化对象 decoratorA 来包装 component，再用 ConcreteDecoratorB 的对象 decoratorB 包装 decoratorA，最终执行 decoratorB 的 Operation ()
     **/
}
```

### 也就是说  

- Component是定义一个接口，可以给这些对象动态地添加职责
- ConcreteComponent是定义了一个具体的对象，也可以给这个对象添加一些职责
- ConcreteComponent是定义了一个具体的对象，也可以给这个对象添加一些职责
- ConcreteComponent是定义了一个具体的对象，也可以给这个对象添加一些职责

## 装饰模式好处

- 把类中的装饰功能从类中搬移去除，这样可以简化原有的类 -> 
- 有效地把类的核心职责和装饰功能区分开了。而且可以去除相关类中重复的装饰逻辑