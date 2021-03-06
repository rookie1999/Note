# Java接口的新特性
## 接口的默认方法
* 在JDK8以后，允许在接口中定义默认方法，为了解决接口升级的情况
* 对于接口升级的情况，由于一个接口拥有多个实现类，在接口中增加一个方法时，对于每一个接口的实现类都需要实现这个方法，是非常复杂的
* 所以允许在接口中定义默认方法，即在接口中已经实现的，实现类也继承了该方法，可以直接使用或进行重写
* 格式：  public default 返回值类型 方法名称(参数列表) { 方法体 }
    * public修饰符可以省略
* 如果一个实现类实现了多个接口，这多个接口中存在冲突的默认方法，则必须对默认方法进行重写
* 如果一个接口继承了多个接口，并且多个接口的默认方法存在冲突，则子接口必须对默认方法进行重写，并且方法仍然是default
* 如果一个子类的直接父类中有方法和该子类实现的接口中的默认方法存在冲突，直接父类的方法优先级更高
## 接口中的静态方法
* 从JDK8开始，允许在接口中定义静态方法
* 理解：对于需要对象调用的方法通过接口中的抽象方法来完成，对于共享的方法通过接口中的静态方法完成
* 格式：  public static 返回值类型 方法名称(参数列表) { 方法体 }
    * public 修饰符可以省略
* 注意：不可以通过实现类来调用接口中的静态方法（一个类可以实现多个接口，如果这多个接口中的静态方法产生了冲突，就无法进行正确调用）
## 接口中的私有方法
* 从JDK9开始，允许在接口中定义私有方法
* 对于接口中的默认方法或静态方法中存在重复代码的情况，进行抽取为一个共有方法，但是逻辑上这个共有方法是不可以被实现类访问的
* 普通私有方法（可以解决默认方法重复代码）
    * 格式：  private 返回值类型 方法名称(参数列表) { 方法体 }
* 静态私有方法（可以解决默认方法和静态方法的重复代码）
    * 格式：  private static 返回值类型 方法名称(参数列表) { 方法体 }
