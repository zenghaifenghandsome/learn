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
### 注入属性--外部bean
### 注入属性--内部bean和级联赋值
### bean作用域
    1. 在spring里面，设置创建bean实例是单实例还是多实例
    2. 在spring里面，默认情况下，bean是单实例
    3. 设置单实例还是多实例
        - 在spring配置文件bean标签里面有属性（scope）用于设置单实例还是多实例
        - scope属性值：默认值-singleton，表示单实例对象，prototype，表示多实例对象
    singleton,在加载配置文件的时候就创建单例对象，prototype在调用getBean方法的时候才创建多实例对象
### bean生命周期
    1. 通过构造器创建bean实例（无参构造）
    2. 为bean的属性设置值和对其他bean引用（调用set方法）
    3. 调用bean的初始化的方法（需要进行配置）
    4. bean可以使用了（对象获取到了）
    5. 当容器关闭的时候，调用bean的销毁方法（需要进行配置销毁的方法）
### bean的后置处理器
    1. 通过构造器创建bean实例（无参构造）
    2. 为bean的属性设置值和对其他bean引用（调用set方法）
    3. 把bean实例传递bean后置处理器的方法
    4. 调用bean的初始化的方法（需要进行配置）
    5. 把bean实例传递bean后置处理器的方法
    6. bean可以使用了（对象获取到了）
    7. 当容器关闭的时候，调用bean的销毁方法（需要进行配置销毁的方法）
## xml自动装配
    1. 什么是自动装配？
        根据指定装配规则（属性名称或者属性类型），spring自动将匹配的属性值进行注入
    2. 实现自动装配
        bean标签属性autowire ，配置自动装配。
        autowire属性常用的2个值：byName根据属性名称注入，注入值bean的id值和类属性名称一样，byType根据属性类型注入
## bean管理：外部配置文件
    1. 直接配置数据库信息
        配置连接池
    2. 引入外部属性文件配置数据库连接池





























