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
