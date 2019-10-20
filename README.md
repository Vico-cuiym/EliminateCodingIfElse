# 几种消除代码中的if-else if方法
在代码中我们经常看到很多的if-else if-else这类不优雅的代码 , 如果还不是单独method处理这些判断 , 那么后期对判断的内容很多的争议 , 然后这段代码就变成了祖传代码
## 先说说代码中存在if-else if的场景 
* 1、代码迭代速度过快 , 文档无法跟上代码更新 , 没有优化的时间
* 2、需求不够明确 , 无法一次性写出整体业务
* 3、开发人员变动过于频繁 , 后期人员无法了解全部业务
### 总之就是"懒"
## 演示案例
```java
String strategy = "fast";
if (strategy.equals("fast")) {
  // 快速
} else if (strategy.equals("normal")) {
  // 正常
} else if (strategy.equals("smooth") or strategy.equals("slow")) {
  // 平滑/慢慢
} else {
  // 默认
}
```
## 使用switch ， 优化演示案例
```java
switch(strategy){
    case "fast" :
        //快速
        break;
    case "normal" :
        //正常
        break;
    case "smooth" :
    case "slow" :
        //平滑/慢慢
        break;
    default :
        //默认
        break;
}
```
### 优点
* 1、switch会使得代码看起来简单 , 更容易明白那些是处理对应的中间业务流程有哪些
### 缺点
* 1、switch在判断区间值或者多个值的时候要一个个列举出来 , 不是特别方便
* 2、以上的写法仅限在1.8以上版本JDK , 1.8以下版本仅限long、int、char类型
## 使用多态 + Optional ， 优化演示案例
```java
 interface Strategy {
    void doSomething() throws Exception;
 }   
 
 class FastStrategy implements Strategy{
    @Override
    public doSomething () throws Exception{
        //快速
    }
 }
 class NormalStrategy implements Strategy{
    @Override
    public doSomething () throws Exception{
        //正常
    }
 }
 class SmoothOrSlowStrategy implements Strategy{
    @Override
    public doSomething () throws Exception{
        //平缓/慢
    }
 }
 class DefaultStrategy {
    public doSomething () throws Exception{
        //默认
    }
 }
class showDoSomething{
 String flag = "fast";
 Map<String,Object> map = new HashMap<String,Object>;
 map.put("fast", new FastStrategy());
 map.put("normal", new NormalStrategy());
 map.put("smooth", new SmoothOrSlowStrategy());
 map.put("slow", new SmoothOrSlowStrategy());
 Strategy strategy = map.get(flag);
 Optional<Strategy> optional = Optional.ofNullable(strategy);
 userOptional.map((new DefaultStrategy()).doSomething()).orElse(strategy.doSomething());
}
```
### 优点
* 1、一个类处理一个对应的事件
* 2、代码的值和处理方式更清晰
### 缺点
* 1、Optional必须是JDK1.8以上才可以使用的 , 1.8以下版本只能自己写非空判断进行默认值的处理
* 2、除了默认方法 , 其余方法一定要实现一个接口类 ,并且将所有的接口类放入map中
* 3、多条件的情况下 , 要在map中存入同一对象
## 使用枚举 , 优化演示案例
```java
public enum Strategy{
    FAST{
        @Override
        void doSomething(){
            //快速
        }
    },
    NORMAL{
        @Override
        void doSomething(){
            //正常
        }
    },
    SMOOTH{
        @Override
        void doSomething(){
            //缓慢
        }
    },
    SLOW{
        @Override
        void doSomething(){
            //慢慢
        }
    };
    abstract void doSomething();
}
```
### 优点
* 1、在调用的时候 ，会很方便
* 2、很清楚地看到对应类型的处理方式，并统一地方书写，易于维护
### 缺点
* 1、进行默认值添加的处理方式的时候，还是要进行非空判断
* 2、对于多条件判断时，需要写多个同样的处理方式 ， 当然后期可以抽成共用的 , 也可以通过switch进行判断
# 🚨以下为参考文档🚨
[多态教程](https://www.runoob.com/java/java-polymorphism.html)<br>
[JAVA8新特性](https://www.runoob.com/java/java8-new-features.html)<br>
[JAVA枚举的用法](https://blog.csdn.net/zl1zl2zl3/article/details/88368284)<br>

