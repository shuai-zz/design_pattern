# 工厂模式

## just show you the code

```java
//运算类
public class Operation{
    private double numberA = 0;
    private double numberB = 0;

    public double getNumberA() {
        return numberA;
    }

    public void setNumberA(double numberA) {
        this.numberA = numberA;
    }

    public double getNumberB() {
        return numberB;
    }

    public void setNumberB(double numberB) {
        this.numberB = numberB;
    }

    public double getResult() {
        double result = 0;
        return result;
    }
}
```

```java
// 加减乘除实现类
class OperationAdd extends Operation{
    @Override
    public double getResult(){
        return getNumberA()+getNumberB();
    }
}

class OperationSub extends Operation{
    @Override
    public double getResult(){
        return getNumberA()-getNumberB();
    }
}

class OperationMul extends Operation{
    @Override
    public double getResult(){
        return getNumberA()*getNumberB();
    }
}

class OperationDiv extends Operation{
    @Override
    public double getResult(){
        if(getNumberB==0){
            throw ArithmeticException("除数不能为0");
        }
        return getNumberA()*getNumberB();
    }
}
```

```java
//工厂类
public class OperationFactory{
    public static Operation createOperation(String operate){
        Operation oper = null;
        switch(operate){
            case "+":
                oper=new OperationAdd();
                break;
            case "-":
                oper=new OperationSub();
                break;
            case "*":
                oper=new OperationMul();
                break;
            case "/":
                oper=new OperationDiv();
                break;
        }
        return oper;
    }
}
```

```java
//主函数
public static void main(String[] args){
    Operation oper;
    oper=OperationFactory.createOperate("+");
    oper.setNumberA(1);
    oper.setNumberB(2);
    double result=oper.getResult();
}
```

## 如果写成这样呢

```java
public class Operation{
    public static double getResult(double numberA,double numberB,string operate){
        double result=0;
        switch(operate){
            case "+":
                reuslt=numberA+numberB;
                break;
            //........else operation
        }
        return result;
    }
}
```

```java
public static void main(String[] args){
    numberA=1;
    numberB=2;
    double result=Operation.getResult(1,2,"+");
}
```

## 工厂模式好处

通过对比，不难看出工厂模式有以下几个优势

1. 如果修改加法运算，则只需要编辑OperationAdd类即可，避免修改到其他运算 -> 降低耦合度，封装对象创建逻辑，依赖倒置原则
2. 增加求幂运算时，只需更改两处：新增OperationPow子类，修改OperationFactory类中的switch语句即可-> 对扩展开放，对修改封闭

UML图：
![factory](/picture/factory_pattern.png)
