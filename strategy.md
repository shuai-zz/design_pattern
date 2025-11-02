# 策略模式

## show you the code

目的：商场的收银系统，包括打折、返点等功能

```java
//现金收取抽象类，参数为原价，返回当前价
public abstract class CashSuper{
    public double acceptCash(double money);
}
```

```java
//实现类

//正常价格
public class CashNormal extends CashSuper{
    @Override
    public double acceptCash(double money){
        return money;
    }
}

//打折收费
public class CashRebate extends CashSuper{
    public double moneyRebate=1;
    public CashRebate(double moneyRebate){
        this.moneyRebate=moneyRebate;//初始化时必须输入折扣率
    }
    @Override
    public double acceptCash(double money){
        return money*moneyRebate;
    }
}

//返利收费
public class CashReturn extends CashSuper{
    private double moneyCondition=0;
    private double moneyReturn=0;
    public CashReturn(double moneyCondition,double moneyReturn){
        this.moneyCondition=moneyCondition;
        this.moneyReturn=moneyReturn;
    }
    @Override
    public double acceptCash(double money){
        double result=money;
        if(money>=moneyCondition){
            result=money-Math.Floor(money/moneyCondition)*moneyReturn;
        }
        return result;
    }
}
```

```java
//策略模式类
public class CashContext{
    CashSuper cs=null;
    public CashContext(String type){
        switch(type){
            case "正常收费":
                CashNormal cashNormal=new CashNormal();
                cs=cashNormal;
                break;
            case "满300反100":
                CashReturn cashReturn=new CashReturn(300,100);
                cs=cashReturn;
                break;
            case "打八折":
                CashRebate cashRebate=new CashRebate(0.8);
                cs=cashRebate;
                break;
        }
    }
    public double getResult(double money){
        return cs.acceptCash(money);
    }
}
```

```java
//主函数
public static void main(String[] args){
    double total=0;
    CashContext cashContext=new CashContext("打八折");
    while(...)//if there are still some goods that are not counted
    {
        double finalPrice=cashContext.getResult(..., ...);//price and number
        total+=finalPrice;
    }
    System.out.Println("total price:"+total);


}
```

## 如果写成这样呢

直接写成工厂模式不是也可以吗

```java
//抽象方法
public abstract class CashSuper{
    public double acceptCash(double money);
}

//具体实现类同上
class CashNormal extends CashSuper{
    @Override
    public double acceptCash(double money){
        return money;
    }
}

class CashRebate extends CashSuper{
    private double moneyRebate=1;
    public CashRebate(double moneyRebate){
        this.moneyRebate=moneyRebate;
    }
    @Override
    public double acceptCash(double money){
        return money*moneyRebate;
    }
}

class CashReturn extends CashSuper{
    private double moneyCondition=0;
    private double moneyReturn=0;
    public CahsReturn(double moneyCondition,double moneyReturn){
        this.moneyCondition=moneyCondition;
        this.moneyReturn=moneyReturn;
    }
    @Override
    public double acceptCash(double money){
        double result=money;
        if(money>=moneyCondition){
            result=money-Math.Floor(money/moneyCondition)*moneyReturn;
        }
        return result;
    }
}
```

```java
//工厂类
class CashFactory{
    public static CashSuper createCashAccept(String type){
        CashSuper cs=null;
        swith(type){
            case "正常收费":
                cs=new CashNormal();
                break;
            case "满300返100":
                cs=new CashReturn(300,100);
                break;
            case "打8折":
                cs=new CashRebate("0.8");
                break;

        }
        return cs;
    }
}
```

## 为什么不写成工厂模式呢

工厂模式很好地解决了对象的创建问题，但商场的收银系统会经常更改打折和促销活动，每次扩展都要改动工厂（增加switch）+新增类，代码就需要重新编译部署。

## 策略模式的好处

1. 定义一系列算法的方法，所有算法都是完成相同工作（打折），只是实现不同（满减、返点），策略模式可通过相同方式调用所有算法（CashContext），减少算法类和使用算法类之间的耦合
2. 策略模式的Strategy类层次为Context定义了一系列可供重用的算法或行为。继承有助于析出这些算法中的公共功能(getResult())
3. 简化了单元测试，每个算法都有自己的类
4. **总而言之，策略模式用来封装几乎任何类型的规则，只要在分析过程中听到需要在不同时间应用不同的业务规则时，就考虑使用策略模式处理**

```text
问题：若新增代码，仍需更改CashContext中的switch代码->解决方法：反射
```
