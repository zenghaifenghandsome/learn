# 基于xml方式注入属性

### DI：依赖注入，就是注入属性

    第一种注入方式：使用set方法进行注入

    1. 创建类，添加属性，添加set方法
    2. 在spring配置文件配置对象创建，配置属性注入

    第二种注入方式：使用有参构造进行注入

    1. 创建类，定义属性，创建属性对应构造方法
    2. 在spring配置文件配置对象创建，配置有构造方法注入
### P名称空间注入
    使用P名称空间注入，可以简化基于xml配置方式
    1. 添加p命名空间在配置文件中
        xmlns:p="http://www.springframework.org/schema/p"
    2. 进行属性注入，在bean标签里面进行操作
        <bean id="book" class="com.springLearn.spring5.Book" p:bname="九阳神功" p:bauthor="光头"></bean>
## xml注入其他类型属性
### 字面量
    1. null值
        <property name="address"><null /></property>
    2. 属性值包含特殊符号
        - 转义
        - cdata
 


