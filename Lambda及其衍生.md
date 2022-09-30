# Lambda及其衍生

------

## 新建一个线程的两种写法

```java
package Lambda;

/**
 * @author xiaojq
 * @date 2022/9/28 9:52
 * @description 匿名内部类写法和lambda写法
 */
public class NewThread {
    /**
     *匿名内部类写法和lambda写法都可以实现相同的内容, 但是其中运行机制是完全不一样的
     *
     * 匿名内部类是通过传入一个实现Runnable接口的实例对象来实现
     *
     * lambda方式实际上是通过将函数的引用作为参数来传给Thread, Runnable是一个函数式接口
     *
     * Thread拥有了这个方法, 才能通过start()调用这个方法, 其中并不会产生新的类
     */
    public static void main(String[] args) {
//        new Thread(new Runnable() {
//            @Override
//            public void run() {
//                System.out.println("匿名内部类启动一个线程");
//            }
//        }).start();
        new Thread(()->{
            System.out.println("lambda方式启动一个线程");
        }).start();
    }

}
```

------

## 自定义方法引用

```java
package Lambda;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

/**
 * @author xiaojq
 * @date 2022/9/28 10:19
 * @description 自定义方法引用(Lambda表达式)
 */
@FunctionalInterface
interface KiteFunction<T,R,S>{
    /**
     * 定义一个双参数一个返回值的函数式接口
     * 函数式接口必须只有一个接口方法
     * 函数式接口最好写上@FunctionalInterface
     * 函数式接口必须参数个数/类型/返回值类型都对得上才能成功被接收
     * @param t
     * @param s
     * @return
     */
    R run(T t, S s);
}
public class MethodReference {
    public static void main(String[] args) {
        // 将方法传入给functionDateFormat这个方法引用中
        KiteFunction<LocalDateTime,String,String> functionDateFormat=((dateTime, patten) -> {
            DateTimeFormatter dateTimeFormatter=DateTimeFormatter.ofPattern(patten);
            return dateTime.format(dateTimeFormatter);
        });
        // functionDateFormat实际上是一个方法引用对象, 只是没有任何属性
        System.out.println(functionDateFormat instanceof Object); // true
        // 通过方法引用调用实际的方法
        String dateString= functionDateFormat.run(LocalDateTime.now(),"yyyy-MM-dd HH:mm:ss");
        System.out.println(dateString);
    }


}
```

------

对于集合操作的stream API

```java
package Lambda.Stream;

import java.util.ArrayList;
import java.util.List;
import java.util.Locale;
import java.util.Random;
import java.util.stream.Collectors;
import java.util.stream.Stream;

/**
 * @author xiaojq
 * @date 2022/9/28 11:11
 * @description Stream of api 使用
 */

/**
 * 创建流
 */
class Of{
    // 实际上是转成数组的流
    Stream<String> stringStream= Stream.of("a","b","c");
}
/**
 * 创建空的流
 */
class Empty{
    // 创建空的Stream对象
    Stream emptyStream=Stream.empty();
}
/**
 * 连接两个流
 */
class Concat{
    // 链接两个Stream, 不改变任何一个Stream对象, 返回一个新的对象
    private static void concatStream(){
        Stream<String> a=Stream.of("a","b","c");
        Stream<String> b=Stream.of("d","e");
        Stream<String> c = Stream.concat(a, b);
    }
}
/**
 * 求最大值
 */
class Max{
    public static void getMax(){
        Stream<Integer> integerStream=Stream.of(2,2,100,5);
//        // 使用Integer中的compareTo
//        Integer max=integerStream.max(Integer::compareTo).get();
//
        // 自定义comparator
        Integer max=integerStream.max((a,b)->{
            return a>b?1:-1;
        }).get();
        System.out.println(max);
    }
}
/**
 * 求最小值
 */
class Min{
    // 和求最大值一样, 略
}
/**
 * 获取Stream中的第一个元素
 */
class FindFirst{
}
/**
 * 获取stream中的某个元素
 */
class FindAny{
}
/**
 * 返回元素个数
 */
class Count{
    Stream<String> a=Stream.of("a","b","c");
    long num=a.count();
}
/**
 * 对Stream的每个元素进行对应操作,接受的Consumer<T>的函数式接口,
 * 但是不消耗Stream中的元素
 */
class Peek{
    public static void getPeek(){
        Stream<String> a=Stream.of("a","b","c");
        List<String> collect = a.peek(e -> System.out.println(e.toUpperCase()))
                .collect(Collectors.toList());
    }
}
/**
 * 和peek类似,同样接受Consumer<T>, foreach执行后,
 * Stream就没了,不能进行后序操作Stream了
 */
class ForEach{
    public static void getForEach(){
        Stream<String> a=Stream.of("a","b","c");
        a.forEach(e -> System.out.println(e.toUpperCase()));
    }
}
/**
 * 和foreach是一样的,但是forEachOrdered是保证顺序的,
 * 对Stream元素按插入顺序进行消费, 并行流的时候有用
 */
class  ForEachOrdered{
    public static void getForEachOrdered(){
        Stream<String> a = Stream.of("a", "b", "c");
        a.parallel().forEachOrdered(e->System.out.println(e.toUpperCase()));
    }
}
/**
 * 获取前n条数据, 只接收数据条数
 */
class Limit{
    private static void getLimit(){
        Stream<String> a=Stream.of("a","b","c");
        a.limit(2).forEach(System.out::println); // a, b
    }
}
/**
 * 跳过前n条数据,
 */
class Skip{
    public static void getLimit(){
        Stream<String> a=Stream.of("a","b","c");
        a.skip(2).forEach(System.out::println); // c
    }
}
/**
 * 有两个重载, 一个无参(按照自然顺序排序)
 * 一个有参: 传入自定义规则进行排序
 */
class Sorted{
    public static void getSorted(){
//        Stream<String> a=Stream.of("a","b","c");
//        a.sorted().forEach(System.out::println);
            Stream<String> a = Stream.of("a1", "c6", "b3");
            a.sorted((x,y)->{
               return Integer.parseInt( x.substring(1))>Integer.parseInt(y.substring(1))?1:-1;
            }).forEach(e->System.out.println(e));

    }
}
/**
 * 构造数据用于以下api展示
 */
class User{
    Integer userId;
    String userName;
    Integer age;
    Integer gender;
    String phone;
    String address;
    @Override
    public String toString() {
        return "User{" +
                "userId=" + userId +
                ", userName='" + userName + '\'' +
                ", age=" + age +
                ", gender=" + gender +
                ", phone='" + phone + '\'' +
                ", address='" + address + '\'' +
                '}';
    }

    public User() {
    }

    public Integer getUserId() {
        return userId;
    }

    public void setUserId(Integer userId) {
        this.userId = userId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Integer getGender() {
        return gender;
    }

    public void setGender(Integer gender) {
        this.gender = gender;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
    public static List<User> getUserData() {
        Random random = new Random();
        List<User> users = new ArrayList<>();
        for (int i = 1; i <= 10; i++) {
            User user = new User();
            user.setUserId(i);
            user.setUserName(String.format("古时的风筝 %s 号", i));
            user.setAge(random.nextInt(100));
            user.setGender(i % 2);
            user.setPhone("18812021111");
            user.setAddress("无");
            users.add(user);
        }
        return users;
    }
}
/**
 * 用于条件过滤, 筛选出符合条件的数据
 */
class Filter{
    public static void getFilter(){
        List<User> users=User.getUserData();
        users.stream().filter(user -> {
            return user.getGender().equals(0)&&user.getAge()>50;
        }).forEach(System.out::println);
    }
}
/**
 * map为映射,接收一个Function函数式接口, 通过原始的数据类型, 映射出新的数据类型
 */
class Map{
    public static void getMap(){
        List<User> users=User.getUserData();
        users.stream().map(user ->data2Dto(user)).collect(Collectors.toList());
    }
    // 这里返回值对象应该是UserDto, 懒得写了, 拿User代替了
    public static User data2Dto(User user){
        User dto = new User();
        // BeanUtils.copyProperties(user, dto);
        //其他额外处理
        return dto;
    }
}
/**
 * 另外还有:
 * 1.mapToInt
 * 2.mapToLong
 * 3.mapToDouble
 * 三个方法,用法和map相同,封装了map而已
 */

/**
 * 当Stream是以下几种结构的时候,需要用到flapMap方法, 用于将原来的二维结构扁平化
 * 1. Stream<String[]>
 * 2. Stream<Set<String>>
 * 3. Stream<List<String>>
 * 通过flapMap方法, 可以将结果转化为Stream<String>的形式
 */
class FlapMap{
    public static void getFlapMap(){
        List<User> users=User.getUserData();
        List<User> users1=User.getUserData();
        List<List<User>> usersList=new ArrayList<>();
        usersList.add(users);
        usersList.add(users1);
        // List流里面还是List, 需要继续转成流, 相当于拆子List进父List
        usersList.stream().flatMap(subList->subList.stream())
                .map(Map::data2Dto)
                .collect(Collectors.toList());
    }
}
/**
 * 如Map一般, 同样有flatMapToInt和flapMapToLong和flapMapToDouble方法
 */

/**
 * collection将流转换成List/Map等常用数据结构
 */
class Collection{
    Stream<Integer> integerStream = Stream.of(1,2,5,7,8,12,33);
    List<Integer> list = integerStream.filter(s -> s.intValue()>7).collect(Collectors.toList());
}
/**
 * Collection返回的是列表, map等, toArray返回的是数组,
 * 有两个重载, 一个空参, 返回Object[]
 *            一个接受IntFunction<R>参数
 */
class ToArray{
    public static void getToArray(){
        List<User> users = User.getUserData();
        users.stream().filter(user->user.getGender().equals(0)&&user.getAge()>50).toArray(User[]::new);
    }

}
/**
 * 每次计算的时候用到上一次计算的结果,
 * 例如求和操作, 前两个数加上第三个数的和, 在加上第四个数,
 * 一直加到最后一个数的结果, 最后返回结果
 */
class Reduce{
    public static void getReduce(){
        Stream<Integer> integerStream=Stream.of(1,2,5,7,8,12,33);
        Integer sum= integerStream.reduce(0,(x,y)->x+y);
        System.out.println(sum);
    }
}

/**
 * 为了加快数据处理的速度, 可以使用并行Stream
 * 支持的api和普通Stream是几乎一样的
 * 什么时候用并行Stream
 * 1.多核Cpu环境下
 * 2.数据量不大普通Stream就行,
 * 3.Cpu密集型适合并行Stream, I/O密集型使用并行Stream反而会更慢
 * 4.虽然计算并行可能性很快, 但是最后还是要使用collect进行合并的,如果合并代价大不建议使用并行
 * 5.有些操作例如limit/findFirst/forEachOrdered依赖元素顺序操作,都不适合并行
 */
class ParallelStream{
    public static void getParallelStream(){
        User.getUserData().parallelStream().forEach(System.out::println);
    }
}
public class StreamAPI {

    public static void main(String[] args) {

    }

}
```