---
title: Spring
date: 2026-06-06T14:30:19+08:00
lastmod: 2026-06-06T23:04:01+08:00
---

# Spring

# 自动装配[^1]

|阶段|核心方式|匹配规则|现状|
| --------------| ------------------| ------------------| --------------------------|
|XML 配置时代|​`<bean>`​标签的`autowire`属性|​`no`​/`byName`​/`byType`​/`constructor`|现在较少用，但原理是基础|
|注解开发时代|​`@Autowired`​/`@Resource`​/`@Inject`|​`byType`​为主，配合`byName`|目前主流开发方式|

‍

# AOP 面向切面编程[^5]

原理：动态代理

‍

# IOC 控制反转

说明：可以用来减低计算机代码之间的耦合度，将所有的bean交由spring管理，其依赖的原理是依赖注入（Dependency Injection，简称DI）优点：降低代码之间的耦合度，实现开闭原则实现：读取注解或配置文件，获取所需要的service，拿到类名使用反射，基于类名实例化对应的对象实例将对象实例，通过构造函数或者setter，传递给调用者

‍

# DI 依赖注入[^6]

依赖注入的核心是 “将依赖的控制权交给 Spring 容器”，构造器注入是最安全、最符合设计原则的方式，推荐生产环境优先使用

Spring 依赖注入（DI）主要有 **3 种核心方式**，以及 **1 种特殊场景的补充方式**：

1. ​**字段注入**（最常用，但**不推荐**生产环境使用）；

   - ​`@Autowired`​：默认 `byType`​，配合 `@Qualifier`​ 用 `byName`；
   - ​`@Resource`​：默认 `byName`​，找不到回退 `byType`；
2. ​`@Autowired`​ **构造器注入**（Spring 官方推荐）；

   - 保证依赖不可变、不为空、完全初始化
3. ​`@Autowired`​ **Setter 方法注入**；

   - 必需依赖用构造器，可选依赖用 Setter；
4. **Lookup 方法注入**（特殊场景：单例 Bean 依赖原型 Bean）。

   - **特殊场景**：单例依赖原型用  **@Lookup 方法注入**。

‍

# Bean

## Bean 作用域[^7]

## Bean 生命周期[^8]

## 单例 Bean[^9]

Spring 在多线程下，保证 Bean 是单例唯一的。

‍

## 原型 Bean[^10]

‍

## 

‍

‍

# Spring 重点类

- ​`BeanFactory`[^11]

  - ​`ApplicationContext`[^12]
- ​`FactoryBean`[^13]
- ​`ObjectFactory`[^14]
- ​`ObjectProvider`[^15]
- BeanPostProcessor
- MultipartFile[^16]

‍

# 拦截链[^17]

‍

# 注解

## 声明 Bean 注解

|注解|说明|
| ------| -----------------------------|
|​`@Component`|通用组件注解|
|​`@Service`|业务逻辑层（Service层）|
|​`@Repository`|数据访问层（DAO层）|
|​`@Controller`|控制层（MVC中的Controller）|
|​`@RestController`|=`@Controller`​+`@ResponseBody`，专用于REST API|

​`@ResponseBody`​ 作用于​**返回值**：将方法返回的对象直接写入 HTTP 响应体（JSON/XML），跳过视图解析器。

## 依赖注入注解[^6]

|注解|说明|
| ------------------| ---------------------------------|
|​`@Autowired`[^2]|Spring提供，按类型注入，**​`byType`​**|
|​`@Qualifier`|与`@Autowired`配合，按名称注入|
|​`@Primary`|同类型有多个Bean时，优先使用|
|​`@Resource`[^4]|JSR-250标准，默认按名称注入，**​`byName`​**|
|​`@Lookup`[^18]|单例 Bean 依赖原型 Bean|

- ​`@Autowired`​：默认 `byType`​，配合 `@Qualifier`​ 用 `byName`；
- ​`@Resource`​：默认 `byName`​，找不到回退 `byType`；

**推荐做法**：生产环境优先用 **构造器注入 +**   **​`@Autowired`​**（保证依赖不可变、不为空），可选依赖用 Setter 注入。

‍

## 配置类与生命周期

|注解|说明|
| ------| -------------------------------------------|
|​`@Configuration`|标记配置类|
|​`@Bean`|在配置类中声明一个Bean|
|​`@ComponentScan`|指定扫描包路径|
|​`@Scope`|设置Bean的作用域（singleton/prototype等）|
|​`@Lazy`|延迟初始化|
|​`@PostConstruct`|Bean初始化后执行|
|​`@PreDestroy`|Bean销毁前执行|

## Spring MVC / Web

|注解|说明|
| ------------| --------------------------------------------------------------------------------------------------|
|​`@RequestMapping`|映射请求路径|
|​`@GetMapping / @PostMapping /`<br />`@PutMapping / @DeleteMapping`<br />|简化HTTP方法映射|
|​`@RequestParam`|提取请求参数|
|​`@PathVariable`|提取URL路径变量|
|​`@RequestBody`|将请求体转换为对象<br />作用于**返回值**：将方法返回的对象直接写入 HTTP 响应体（JSON/XML），跳过视图解析器。<br />|
|​`@ResponseBody`|将返回值直接写入HTTP响应|
|​`@ModelAttribute`|绑定请求参数到对象|
|​`@CrossOrigin`|允许跨域请求|

## 7.5参数校验

​`@Validated`[^19]​：Spring 对 `@Valid`​ 的增强，提供了**分组**能力。

- Spring 特有注解
- 支持分组校验 `@Validated(GroupA.class)`
- 可以用在类上
- 类上的 `@Validated`​ **只对简单类型参数（**​ **​`@RequestParam`​**​ **、**​ **​`@PathVariable`​**​ **、**​**普通方法参数**[^20] **）上的约束注解（如**   **​`@Min`​**​ **、**​ **​`@NotBlank`​**​ **）生效**，对于自定义对象内部的字段校验，它无能为力。

## 7.6数据访问（Spring Data / MyBatis）

|注解|说明|
| ------| ---------------------------|
|​`@Transactional`|声明事务|
|​`@Entity`|JPA实体类|
|​`@Id`|实体主键|
|​`@Table`|映射数据库表|
|​`@Query`|Spring Data JPA自定义查询|
|​`@Param`|命名参数绑定|
|​`@Mapper`|MyBatis映射接口|

## 7.7Spring Boot 自动配置

|注解|说明|
| ------| ------------------------|
|​`@SpringBootApplication`|​`@Configuration`​ + `@EnableAutoConfiguration`​ + `@ComponentScan` 合体|
|​`@EnableAutoConfiguration`|开启自动配置|
|​`@ConditionalOnClass`|类路径存在时才创建Bean|
|​`@ConditionalOnMissingBean`|没有指定Bean时才创建|
|​`@ConfigurationProperties`|绑定配置文件属性|
|​`@Value`|注入配置文件中的单个值|

@Conditional：条件判断

- 它是在 Bean 实例化之前评估的（`Condition` 类不应与 Spring Bean 产生循环依赖）
- 不要在有 `@Conditional`​ 的配置类中使用 `@Bean`​ 方法返回 `null`，否则可能导致问题。

## 异步与调度

|注解|说明|
| ------| --------------|
|​`@Async`|异步执行方法|
|​`@EnableAsync`|开启异步支持|
|​`@Scheduled`|定时任务|
|​`@EnableScheduling`|开启调度支持|

## 测试注解

|注解|说明|
| ------| --------------------------|
|​`@SpringBootTest`|启动Spring上下文进行测试|
|​`@MockBean`|为测试创建Mock对象|
|​`@Test`|JUnit测试方法|

## 缓存

|注解|说明|
| ------| --------------|
|​`@Cacheable`|缓存返回结果|
|​`@CacheEvict`|清除缓存|
|​`@CachePut`|更新缓存|
|​`@EnableCaching`|开启缓存支持|

‍

‍

‍

[^1]: # 自动装配

    ## 1. 一、总述：Spring 自动装配的两个阶段

    |阶段|核心方式|匹配规则|现状|
    | --------------| ------------------| ------------------| --------------------------|
    |XML 配置时代|​`<bean>`​标签的`autowire`属性|​`no`​/`byName`​/`byType`​/`constructor`|现在较少用，但原理是基础|
    |注解开发时代|​`@Autowired`​/`@Resource`​/`@Inject`|​`byType`​为主，配合`byName`|目前主流开发方式|

    ## 2. 二、XML 配置时代的 `autowire` 属性（面试基础）

    在 XML 中，通过 `<bean>`​ 的 `autowire` 属性指定自动装配模式，主要有 4 种：

    ### 2.1 1. `no`（默认模式：不自动装配）

    **规则**：必须手动通过 `<property ref="...">`​ 或 `<constructor-arg ref="...">` 指定依赖，Spring 不会自动注入。

    **示例**：

    ```
     <bean id="userDao" class="com.example.dao.UserDaoImpl" />
     <bean id="userService" class="com.example.service.UserService" autowire="no">
         <!-- 必须手动写 ref -->
         <property name="userDao" ref="userDao" />
     </bean>
    ```
    ### 2.2 2. `byName`（按属性名自动装配）

    **规则**：Spring 自动找与 **属性名同名** 的 Bean，通过 Setter 方法注入。

    **示例**：

    ```
     <bean id="userDao" class="com.example.dao.UserDaoImpl" />
     <!-- autowire="byName"：找名为 "userDao" 的 Bean -->
     <bean id="userService" class="com.example.service.UserService" autowire="byName" />
    ```
    **匹配逻辑**：

    1. 解析 `UserService`​，发现有属性 `userDao`；
    2. 找容器中 `id="userDao"` 的 Bean；
    3. 调用 setUserDao() 注入。  
       注意：属性名必须和 Bean 的 id 完全一致，否则匹配失败。

    ### 2.3 3. `byType`（按属性类型自动装配）

    **规则**：Spring 自动找与 **属性类型相同** 的 Bean，通过 Setter 方法注入。

    **示例**：

    ```
     <!-- id 可以随便写，byType 不看 id -->
     <bean id="whatever" class="com.example.dao.UserDaoImpl" />
     <bean id="userService" class="com.example.service.UserService" autowire="byType" />
    ```
    **致命问题**：如果容器中有**多个同类型的 Bean**，会直接报错 `NoUniqueBeanDefinitionException`。

    **解决方案**（面试必问）：

    1. ​**​`primary="true"`​** ：标注首选 Bean；
    2. ​**​`autowire-candidate="false"`​** ：排除某些 Bean。

    ```
     <bean id="userDao1" class="com.example.dao.UserDaoImpl1" primary="true" />
     <bean id="userDao2" class="com.example.dao.UserDaoImpl2" autowire-candidate="false" />
    ```
    ### 2.4 4. `constructor`（按构造器参数类型自动装配）

    **规则**：类似 `byType`​，但针对**构造器参数的类型**，通过构造器注入。

    **示例**：

    ```
     <bean id="userDao" class="com.example.dao.UserDaoImpl" />
     <bean id="userService" class="com.example.service.UserService" autowire="constructor" />
    ```
    **注意**：如果有多个同类型的构造器参数，也会报错，解决方案同 `byType`。

    ## 3. 三、注解开发时代的自动装配（目前主流）

    现在开发基本不用 XML 了，而是用注解实现自动装配，核心是以下 3 个注解：

    ### 3.1 @Autowired[^2]（Spring 官方注解，最常用）

    ### 3.2 @Resource[^4]（JSR-250 标准注解）

    ## 4. 四、自动装配的优缺点

    ### 4.1 优点

    1. **减少配置代码**：无需手动写 `ref`，开发效率高；
    2. **解耦配置和代码**：依赖关系通过规则自动匹配，修改依赖不需要改配置。

    ### 4.2 缺点

    1. **不够明确**：依赖关系隐藏在规则中，阅读代码时不如显式配置直观；
    2. **多 Bean 时容易报错**：`byType` 模式下如果有多个同类型 Bean，需要额外处理；
    3. **可能出现意外注入**：规则匹配可能注入不符合预期的 Bean。

    ## 5. 五、面试重点总结（必背）

    1. **XML 模式**：`no`​（默认）、`byName`​（按属性名）、`byType`​（按属性类型，多 Bean 报错）、`constructor`（按构造器类型）；
    2. 注解模式：

       - ​`@Autowired`​：默认 `byType`​，配合 `@Qualifier`​ 用 `byName`；
       - ​`@Resource`​：默认 `byName`​，找不到回退 `byType`；
    3. **推荐做法**：生产环境优先用 **构造器注入 +**   **​`@Autowired`​**（保证依赖不可变、不为空），可选依赖用 Setter 注入。

    ## 6. 一句话总结

    ​`autowire`​ 自动装配是 Spring 简化依赖注入的核心机制，XML 时代通过 `byName`​/`byType`​ 规则匹配，注解时代通过 `@Autowired`​/`@Resource`​ 实现，生产环境推荐构造器注入配合 `@Autowired`，兼顾安全和效率。


[^2]: ## @Autowired

    ### 介绍

    **默认规则**：**​`byType`​**（按类型匹配）。

    **配合**  **​`@Qualifier`​**​：如果有多个同类型 Bean，用 `@Qualifier("beanName")`​ 切换为 **​`byName`​**。

    **示例**：

    ```java
     @Service
     public class UserService {
         // 1. 默认 byType：找类型为 UserDao 的 Bean
         @Autowired
         private UserDao userDao;
     ​
         // 2. byType + @Qualifier：找名为 "userDaoImpl2" 的 Bean
         @Autowired
         @Qualifier("userDaoImpl2")
         private UserDao userDao2;
     ​
         // 3. 构造器注入（Spring 推荐）
         private final RoleDao roleDao;
         @Autowired // 4.x+ 单构造器可省略
         public UserService(RoleDao roleDao) {
             this.roleDao = roleDao;
         }
     }
    ```
    ‍

    ### required 属性

    **特殊属性**：`@Autowired(required = false)` 表示依赖可选，没有就不注入（不会报错）。

    构造器注入的核心优势是**保证依赖不可变**，因此**不推荐用**  **​`@Autowired(required=false)`​** ​ **来标记单个参数可选**（语义不够清晰）。更好的做法是使用：

    **方案 1：**​`@Nullable` 注解（JSR-305 标准）

    在需要可选的参数前加 `@Nullable`，表示 “这个参数可以为 null”，Spring 会自动处理（依赖不存在时传 null，不会报错）。

    示例：`@Nullable` 标记单个参数可选

    ```java
    // Spring 内置的 @Nullable（也可以用 JSR-305 的）
    import org.springframework.lang.Nullable; 
     
    @Service
    public class UserService {
        private final UserDao userDao;
        private final RoleDao roleDao;
     
        // 单构造器：@Autowired 可省略     
    	// userDao 必须存在（默认 required=true）
        // roleDao 用 @Nullable 标记：可选，不存在时传 null
        public UserService(UserDao userDao, @Nullable RoleDao roleDao) {
            this.userDao = userDao;
            this.roleDao = roleDao;
        }
    }
    ```
    **方案 2：** Java 8+ Optional[^3] 类型

    用 `Optional<依赖类型>`​ 作为参数类型，Spring 会自动将依赖包装成 `Optional`​（依赖不存在时为 `Optional.empty()`），语义更清晰，还能避免空指针。

    示例：`Optional` 标记单个参数可选

    ```java
     import java.util.Optional;
     ​
     @Service
     public class UserService {
         private final UserDao userDao;
         private final Optional<RoleDao> roleDao;
     ​
         // 单构造器：@Autowired 可省略
         // userDao 必须存在
         // roleDao 用 Optional 包装：可选，不存在时为 Optional.empty()
         public UserService(UserDao userDao, Optional<RoleDao> roleDao) {
             this.userDao = userDao;
             this.roleDao = roleDao;
         }
     ​
         // 使用时通过 Optional 安全访问
         public void doSomething() {
             roleDao.ifPresent(dao -> {
                 // 只有 roleDao 存在时才执行
                 dao.findRoleById(1L);
             });
         }
     }
    ```

[^3]: ##  `Optional`


[^4]: ## ​`@Resource`

    ### 介绍

    > （JSR-250 标准注解）
    >

    **默认规则**：**​`byName`​**​（按属性名匹配）；如果 `byName`​ 匹配不上，会自动回退到 **​`byType`​**。

    **示例**：

    ```java
    @Service
    public class UserService {
        // 1. 默认 byName：找名为 "userDao" 的 Bean
        @Resource
        private UserDao userDao;

        // 2. 显式指定 name：找名为 "userDaoImpl2" 的 Bean
        @Resource(name = "userDaoImpl2")
        private UserDao userDao2;
    }
    ```
    **对比**  **​`@Autowired`​**：

    - ​`@Resource`​ 是 Java 标准，`@Autowired` 是 Spring 专属；
    - ​`@Resource`​ 默认 `byName`​，`@Autowired`​ 默认 `byType`；
    - ​`@Resource`​ 没有 `required`​ 属性。`@Autowired`​ 有 `required` 属性（要求依赖必须存在）


[^5]: # AOP 面向切面编程

    ## 1. AOP：面向切面编程

    Aspect Oriented Programming——AOP

    面相切面，在不修改源代码的情况下，进行功能增强。

    **AOP** 是面向切面编程，核心思想是把跨越多个模块的通用逻辑抽取出来，通过代理机制动态织入到目标方法上，避免在业务代码里到处复制粘贴。

    最典型的场景就是日志、事务、权限校验。比如你有 100 个 Service 方法都要记录调用日志，不可能每个方法里都写一遍 `log.info()`，用 AOP 定义一个切面，一行配置搞定全部

    # 2. Spring AOP 默认动态代理机制

    > JDK 动态代理 vs CGLIB
    >

    Spring AOP 默认使用 **JDK 动态代理**，但会根据被代理对象的类型**自动切换**—— 如果目标对象实现了接口，默认用 JDK 动态代理；如果目标对象没有实现接口，自动切换为 **CGLIB 动态代理**。 **(SpringBoot 2.x 直接把默认值改成了 CGLIB。)**

    ## 2.1 一、Spring AOP 的默认选择逻辑

    Spring AOP 会根据**目标对象是否实现接口**自动选择动态代理方式：

    1. **目标对象实现了接口** → 默认使用 **JDK 动态代理**；
    2. **目标对象没有实现接口** → 自动切换为 **CGLIB 动态代理**；
    3. **强制使用 CGLIB** → 通过 `@EnableAspectJAutoProxy(proxyTargetClass = true)`​ 或配置 `spring.aop.proxy-target-class=true` 强制使用 CGLIB（Spring Boot 2.x+ 默认强制开启 CGLIB）。

    ## 2.2 二、两种动态代理的核心区别（面试必背）

    |维度|JDK 动态代理|CGLIB 动态代理|
    | ------| ------------------------------------------| --------------------------------------------------------|
    |**底层原理**|基于**Java 反射**，生成实现目标接口的代理类|基于**ASM 字节码生成**，生成目标类的子类作为代理类|
    |**核心依赖**|JDK 原生`java.lang.reflect.Proxy`，无需额外依赖|需依赖`cglib`库（Spring 已内置）|
    |**代理要求**|目标对象**必须实现至少一个接口**|目标对象**无需实现接口**，但不能是`final`类 / 方法|
    |**代理对象类型**|代理对象与目标对象是 **“兄弟关系”** （都实现同一个接口）|代理对象是目标对象的 **“子类”** （继承关系）|
    |**性能对比**|JDK 1.8+ 性能与 CGLIB 相当，甚至略优|创建代理类较慢，但运行时性能与 JDK 动态代理接近|
    |**Spring 默认场景**|目标对象实现了接口时|目标对象未实现接口时，或 Spring Boot 2.x+ 默认强制使用|

    ## 2.3 三、代码示例：两种动态代理的实现

    ### 2. 1. JDK 动态代理示例

    #### 2. 核心要求：目标对象必须实现接口

    ```
     // 1. 定义接口（JDK 动态代理的前提）
     public interface UserService {
         void addUser(String name);
     }
     ​
     // 2. 目标对象：实现接口
     @Service
     public class UserServiceImpl implements UserService {
         @Override
         public void addUser(String name) {
             System.out.println("添加用户：" + name);
         }
     }
     ​
     // 3. 切面：定义增强逻辑
     @Aspect
     @Component
     public class LogAspect {
         @Before("execution(* com.example.UserService.addUser(..))")
         public void beforeAddUser() {
             System.out.println("[前置增强] 准备添加用户...");
         }
     }
     ​
     // 4. 测试：Spring 自动使用 JDK 动态代理
     @SpringBootApplication
     public class AopApplication {
         public static void main(String[] args) {
             ConfigurableApplicationContext context = SpringApplication.run(AopApplication.class, args);
             // 注意：必须用接口类型接收，不能用实现类
             UserService userService = context.getBean(UserService.class);
             userService.addUser("张三");
             // 输出代理对象类型：class com.sun.proxy.$ProxyXX（JDK 动态代理）
             System.out.println("代理对象类型：" + userService.getClass());
         }
     }
    ```
    ### 2. CGLIB 动态代理示例

    #### 2. 核心要求：目标对象无需实现接口，但不能是 final

    ```
     // 1. 目标对象：没有实现接口
     @Service
     public class OrderService {
         public void createOrder(String orderId) {
             System.out.println("创建订单：" + orderId);
         }
     }
     ​
     // 2. 切面：定义增强逻辑
     @Aspect
     @Component
     public class LogAspect {
         @Before("execution(* com.example.OrderService.createOrder(..))")
         public void beforeCreateOrder() {
             System.out.println("[前置增强] 准备创建订单...");
         }
     }
     ​
     // 3. 测试：Spring 自动使用 CGLIB 动态代理
     @SpringBootApplication
     public class AopApplication {
         public static void main(String[] args) {
             ConfigurableApplicationContext context = SpringApplication.run(AopApplication.class, args);
             // 可以直接用目标类类型接收（CGLIB 是子类）
             OrderService orderService = context.getBean(OrderService.class);
             orderService.createOrder("1001");
             // 输出代理对象类型：class com.example.OrderService$$EnhancerBySpringCGLIB$$XX（CGLIB）
             System.out.println("代理对象类型：" + orderService.getClass());
         }
     }
    ```
    ## 2.4 四、Spring Boot 2.x+ 的特殊变化

    Spring Boot 2.x 开始，**默认强制使用 CGLIB 动态代理**，即使目标对象实现了接口 —— 通过配置 `spring.aop.proxy-target-class=true` 实现（默认值为 true）。

    如果想在 Spring Boot 中恢复 “接口用 JDK 动态代理” 的传统行为，需在 `application.yml` 中配置：

    ```
     spring:
       aop:
         proxy-target-class: false # 关闭强制 CGLIB，恢复传统选择逻辑
    ```
    ## 2.5 五、面试高频考点

    ### 2. 1. 为什么 JDK 动态代理必须基于接口？

    JDK 动态代理的核心是 `Proxy.newProxyInstance()`​，它生成的代理类**已经继承了** **​`Proxy`​**​ **类**（Java 单继承），因此只能通过**实现目标接口**来定义代理逻辑，无法再继承目标类。

    ### 2. 为什么 CGLIB 不能代理 final 类 / 方法？

    CGLIB 是通过**生成目标类的子类**来实现代理的，`final`​ 类无法被继承，`final` 方法无法被重写，因此 CGLIB 无法代理。

    ### 2. 3. 如何强制使用 CGLIB？

    - **注解方式**：在配置类上加 `@EnableAspectJAutoProxy(proxyTargetClass = true)`；
    - **Spring Boot 配置**：`application.yml`​ 中设置 `spring.aop.proxy-target-class=true`（默认）。

    ## 2.6 六、总结

    1. Spring AOP 默认选择  
       ：

       - 传统 Spring：目标对象实现接口用 JDK 动态代理，否则用 CGLIB；
       - Spring Boot 2.x+：默认强制使用 CGLIB。
    2. 两者核心区别  
       ：

       - JDK 动态代理：基于接口、反射，目标必须实现接口；
       - CGLIB：基于继承、字节码生成，目标无需实现接口但不能是 final。
    3. **性能对比**：JDK 1.8+ 两者性能相当，无需纠结性能差异。

    ### 2. 一句话总结

    Spring AOP 默认根据目标对象是否实现接口选择 JDK 动态代理或 CGLIB，Spring Boot 2.x+ 强制用 CGLIB，两者核心区别是 “基于接口” 还是 “基于继承”。

    # **AspectJ**

    Spring AOP 和 AspectJ 都是 Java 平台上实现面向切面编程（AOP）的技术，但它们在实现机制、功能范围、性能和使用方式上有显著的区别。下面我将从多个维度进行详细对比。

    ### 一、核心区别概览

    |维度|Spring AOP|AspectJ|
    | ------| ------------------------------------------------| -----------------------------------------------------------------------|
    |**实现方式**|动态代理（JDK 动态代理或 CGLIB）|静态织入（编译期、编译后、类加载期）|
    |**织入时期**|运行时（Run-time）|编译期（Compile-time）、编译后（Post-compile）、类加载期（Load-time）|
    |**连接点支持**|仅支持方法执行（Method execution）|支持方法、构造器、字段访问、异常处理、静态初始化等更细粒度的连接点|
    |**代理对象**|目标对象必须实现接口（JDK）或可被继承（CGLIB）|直接修改字节码，对目标对象无侵入|
    |**性能**|运行时产生代理，相对较低（但通常可接受）|织入过程提前，运行时无额外开销，性能更高|
    |**配置复杂度**|简单，与 Spring 容器无缝集成|较复杂，需额外编译配置或启动参数|
    |**适用范围**|仅作用于 Spring 容器中的 Bean|可作用于任何 Java 对象（无论是否在容器中）|
    |**与 IDE/工具集成**|无特殊要求，纯 Java 运行|需要 AspectJ 编译器（ajc）或 AspectJ Weaver 支持|

    ### 二、Spring AOP 详解

    #### 1. 实现原理

    Spring AOP 基于**代理模式**，在运行时为目标对象创建代理对象，通过代理对象来增强目标方法。它有两种实现方式：

    - **JDK 动态代理**：要求目标类实现至少一个接口，生成的代理类也实现该接口，通过 `InvocationHandler` 拦截方法调用。
    - **CGLIB 代理**：当目标类没有实现接口时，使用 CGLIB 生成目标类的子类作为代理，通过拦截父类方法实现增强（注意 `final` 方法无法被代理）。

    #### 2. 特点

    - **运行时织入**：在 Spring 容器初始化 Bean 时，根据配置的切面通过后处理器（`BeanPostProcessor`）生成代理对象。
    - **方法级别拦截**：只支持方法连接点，无法拦截字段赋值、构造器调用等。
    - **与 IoC 容器集成紧密**：切面定义通常使用 @AspectJ 注解风格，但底层仍是 Spring AOP 代理。
    - **性能**：由于是运行时动态代理，存在一定的性能开销（反射调用），但在大多数业务场景下可忽略。

    #### 3. 示例

    java

    ```
     @Aspect
     @Component
     public class LoggingAspect {
         @Before("execution(* com.example.service.*.*(..))")
         public void logBefore(JoinPoint joinPoint) {
             // 增强逻辑
         }
     }
    ```
    虽然使用了 @AspectJ 注解，但实际仍由 Spring AOP 的代理机制实现。

    ### 三、AspectJ 详解

    #### 1. 实现原理

    AspectJ 是一个独立的 AOP 框架，通过**字节码织入**实现增强，主要有三种织入方式：

    - **编译期织入**：使用 AspectJ 编译器 `ajc`​ 将切面直接编译进目标类的 `.class` 文件中。
    - **编译后织入**：对已编译的类文件或 JAR 包进行织入，常用于增强第三方库。
    - **类加载期织入（LTW）** ：通过 Java Agent 机制，在类加载时动态织入字节码。

    #### 2. 特点

    - **连接点丰富**：支持方法、构造器、字段读/写、异常处理、静态初始化块等多种连接点。
    - **无侵入**：直接修改目标类的字节码，运行时无需代理对象，目标类本身即增强后的类。
    - **性能优越**：织入过程在编译期或类加载期完成，运行时与普通 Java 方法调用无异。
    - **独立于 Spring**：可在任何 Java 项目中使用，不依赖 Spring 容器。

    #### 3. 示例（纯 AspectJ 语法）

    aspectj

    ```
     public aspect LoggingAspect {
         pointcut publicMethod(): execution(public * *(..));
         
         before(): publicMethod() {
             System.out.println("Before method: " + thisJoinPoint.getSignature());
         }
     }
    ```
    使用 `ajc` 编译后，该切面逻辑直接嵌入目标类中。

    ### 四、Spring 对 AspectJ 的集成

    Spring 允许使用 @AspectJ 注解风格来定义切面，但默认情况下，Spring 仍使用自己的代理机制来实现这些切面（即 Spring AOP）。不过，Spring 也支持与 AspectJ 的真正集成，即利用 AspectJ 的织入能力，可以通过以下方式：

    - **使用**  **​`@Configurable`​**​：结合 AspectJ 的 LTW，为非 Spring 管理的对象（如 `new` 出来的对象）注入依赖。
    - **启用 Load-time Weaving**：在 Spring 配置中添加 `<context:load-time-weaver/>`​ 或 `@EnableLoadTimeWeaving`​，并配合 `-javaagent` 参数启动 JVM，使得 AspectJ 切面在类加载时织入。

    此时，切面逻辑由 AspectJ 实现，而不再由 Spring 代理完成，从而获得 AspectJ 的全部能力。

    ### 五、如何选择？

    - **简单场景（如日志、权限、缓存）** ，且切面只作用于 Spring Bean 的方法：优先使用 **Spring AOP**，因为配置简单，与 Spring 集成完美，学习成本低。
    - **需要拦截构造函数、字段访问**，或需要增强非 Spring 管理的对象：必须使用 **AspectJ** 的织入方式。
    - **对性能有极高要求**：AspectJ 的静态织入优于运行时动态代理，但需权衡构建和配置的复杂性。
    - **遗留系统或复杂切面需求**：AspectJ 提供更全面的支持。

    ### 六、总结

    - **Spring AOP** 是 Spring 框架自带的 AOP 实现，基于代理，简单易用，但功能受限。
    - **AspectJ** 是功能完备的 AOP 框架，通过字节码织入，功能强大，但配置相对复杂。
    - 两者并非互斥，Spring 通过 @AspectJ 注解风格和 LTW 支持，可以灵活地结合使用，既享受 Spring 的便捷，又能利用 AspectJ 的强大能力。

    在实际开发中，理解它们的区别有助于我们根据具体需求选择合适的技术，避免“一把抓”或“过度设计”。

    ‍

    ---

    ‍

    说明：AOP基于IoC基础，是对OOP的有益补充，OOP允许你定义从上到下的关系，但并不适合定义从左到右的关系。简单说就是那些与业务无关，却为多个业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性原理：基于代理模式的方式实现，在调用目标方法之前会先调用代理类的目标方法，然后由代理类返回调用结果。spring aop使用了动态代理的代理方式核心概念：切面：散落在系统各处的通用的业务逻辑代码，如上图中的日志模块，权限模块，事务模块等，切面用来装载pointcut和advice通知：所谓通知指的就是指拦截到连接点之后要执行的代码，通知分为前置、后置、异常、最终、环绕通知五类连接点：被拦截到的点，因为Spring只支持方法类型的连接点，所以在Spring中连接点指的就是被拦截到的方法，实际上连接点还可以是字段或者构造器切入点：拦截的方法，连接点拦截后变成切入点目标对象：代理的目标对象，指要织入的对象模块，如上图的模块一、二、三织入：通过切入点切入，将切面应用到目标对象并导致代理对象创建的过程AOP代理：AOP框架创建的对象，包含通知。在Spring中，AOP代理可以是JDK动态代理或CGLIB代理例子：假如业务模块一、二、三都需要日志记录，那么如果都在三个模块内写日志逻辑，那么会有两个问题：1，打破模块的封装性 2，有很多重复代码 解决重复代码问题，可以通过封装日志逻辑为一个类，然后在各个模块需要的地方通过该类来试下日志功能，但是还是不能解决影响模块封装性的问题。 那么AOP就可以解决，它使用切面，动态地织入到各模块中（实际就是使用代理来管理模块对象），这样既解决了重复代码问题又不会影响模块的封装性


[^6]: # DI 依赖注入

    > 依赖注入的核心是 “将依赖的控制权交给 Spring 容器”，构造器注入是最安全、最符合设计原则的方式，推荐生产环境优先使用
    >

    Spring 依赖注入（DI）主要有 **3 种核心方式**，以及 **1 种特殊场景的补充方式**：

    1. ​**字段注入**（最常用，但**不推荐**生产环境使用）；

       - ​`@Autowired`​：默认 `byType`​，配合 `@Qualifier`​ 用 `byName`；
       - ​`@Resource`​：默认 `byName`​，找不到回退 `byType`；
    2. ​`@Autowired`​ **构造器注入**（Spring 官方推荐）；

       - 保证依赖不可变、不为空、完全初始化
    3. ​`@Autowired`​ **Setter 方法注入**；

       - 必需依赖用构造器，可选依赖用 Setter；
    4. **Lookup 方法注入**（特殊场景：单例 Bean 依赖原型 Bean）。

       - **特殊场景**：单例依赖原型用  **@Lookup 方法注入**。

    ‍

    # 字段注入

    **定义**

    直接在 Bean 的**字段**上加 `@Autowired`​、`@Resource`​、`@Inject` 等注解，Spring 容器通过反射直接设置字段值。

    ## @Autowired

    ### 介绍

    **默认规则**：**​`byType`​**（按类型匹配）。

    **配合**  **​`@Qualifier`​**​：如果有多个同类型 Bean，用 `@Qualifier("beanName")`​ 切换为 **​`byName`​**。

    **示例**：

    ```java
     @Service
     public class UserService {
         // 1. 默认 byType：找类型为 UserDao 的 Bean
         @Autowired
         private UserDao userDao;
     ​
         // 2. byType + @Qualifier：找名为 "userDaoImpl2" 的 Bean
         @Autowired
         @Qualifier("userDaoImpl2")
         private UserDao userDao2;
     ​
         // 3. 构造器注入（Spring 推荐）
         private final RoleDao roleDao;
         @Autowired // 4.x+ 单构造器可省略
         public UserService(RoleDao roleDao) {
             this.roleDao = roleDao;
         }
     }
    ```
    ‍

    ### required 属性

    **特殊属性**：`@Autowired(required = false)` 表示依赖可选，没有就不注入（不会报错）。

    构造器注入的核心优势是**保证依赖不可变**，因此**不推荐用**  **​`@Autowired(required=false)`​** ​ **来标记单个参数可选**（语义不够清晰）。更好的做法是使用：

    **方案 1：**​`@Nullable` 注解（JSR-305 标准）

    在需要可选的参数前加 `@Nullable`，表示 “这个参数可以为 null”，Spring 会自动处理（依赖不存在时传 null，不会报错）。

    示例：`@Nullable` 标记单个参数可选

    ```java
    // Spring 内置的 @Nullable（也可以用 JSR-305 的）
    import org.springframework.lang.Nullable; 
     
    @Service
    public class UserService {
        private final UserDao userDao;
        private final RoleDao roleDao;
     
        // 单构造器：@Autowired 可省略     
    	// userDao 必须存在（默认 required=true）
        // roleDao 用 @Nullable 标记：可选，不存在时传 null
        public UserService(UserDao userDao, @Nullable RoleDao roleDao) {
            this.userDao = userDao;
            this.roleDao = roleDao;
        }
    }
    ```
    **方案 2：** Java 8+ Optional[^3] 类型

    用 `Optional<依赖类型>`​ 作为参数类型，Spring 会自动将依赖包装成 `Optional`​（依赖不存在时为 `Optional.empty()`），语义更清晰，还能避免空指针。

    示例：`Optional` 标记单个参数可选

    ```java
     import java.util.Optional;
     ​
     @Service
     public class UserService {
         private final UserDao userDao;
         private final Optional<RoleDao> roleDao;
     ​
         // 单构造器：@Autowired 可省略
         // userDao 必须存在
         // roleDao 用 Optional 包装：可选，不存在时为 Optional.empty()
         public UserService(UserDao userDao, Optional<RoleDao> roleDao) {
             this.userDao = userDao;
             this.roleDao = roleDao;
         }
     ​
         // 使用时通过 Optional 安全访问
         public void doSomething() {
             roleDao.ifPresent(dao -> {
                 // 只有 roleDao 存在时才执行
                 dao.findRoleById(1L);
             });
         }
     }
    ```
    ## ​`@Resource`

    ### 介绍

    > （JSR-250 标准注解）
    >

    **默认规则**：**​`byName`​**​（按属性名匹配）；如果 `byName`​ 匹配不上，会自动回退到 **​`byType`​**。

    **示例**：

    ```java
    @Service
    public class UserService {
        // 1. 默认 byName：找名为 "userDao" 的 Bean
        @Resource
        private UserDao userDao;

        // 2. 显式指定 name：找名为 "userDaoImpl2" 的 Bean
        @Resource(name = "userDaoImpl2")
        private UserDao userDao2;
    }
    ```
    **对比**  **​`@Autowired`​**：

    - ​`@Resource`​ 是 Java 标准，`@Autowired` 是 Spring 专属；
    - ​`@Resource`​ 默认 `byName`​，`@Autowired`​ 默认 `byType`；
    - ​`@Resource`​ 没有 `required`​ 属性。`@Autowired`​ 有 `required` 属性（要求依赖必须存在）

    ## 优点

    - **代码极其简洁**：不需要写构造器或 Setter 方法，开发效率高。

    ## 缺点

    1. **违反单一职责原则**：字段注入很容易让类的依赖越来越多，却不容易发现（如果是构造器注入，构造器臃肿了就会提醒你拆分）；
    2. **依赖不可变**：无法用 `final` 修饰，依赖可能被修改；
    3. **依赖可能为空**：没有参数校验，容易出现运行时 `NullPointerException`；
    4. **无法在没有 Spring 容器的环境下使用**：比如单元测试，必须启动 Spring 容器才能注入依赖，无法直接 `new` 对象测试；
    5. **违反依赖注入的设计初衷**：依赖注入的核心是 “类不关心依赖的来源，只关心依赖的接口”，但字段注入让类直接依赖 Spring 的注解，耦合度变高。

    ## 适用场景

    - **快速原型开发**（Demo、POC）；
    - 不推荐生产环境使用。

    ‍

    # 构造器注入

    **定义**

    通过 Bean 的**构造器参数**注入依赖，Spring 容器在实例化 Bean 时，会根据构造器参数的类型 / 名称，找到对应的依赖 Bean 传入。

    **代码示例**

    ```java
     @Service
     public class UserService {
    // 可以用 final 修饰，保证依赖不可变
         private final UserDao userDao; 
         private final RoleDao roleDao;
     ​
         // Spring 4.x 之后：如果只有一个构造器，@Autowired 可以省略
         @Autowired
         public UserService(UserDao userDao, RoleDao roleDao) {
             // 可以在这里做参数校验，保证依赖不为空
             this.userDao = Objects.requireNonNull(userDao, "UserDao 不能为空");
             this.roleDao = Objects.requireNonNull(roleDao, "RoleDao 不能为空");
         }
     }
    ```
    ## 优点

    1. **保证依赖不可变**：可以用 `final` 修饰依赖字段，防止后续被修改；
    2. **保证依赖不为空**：构造器参数如果为空，Spring 容器启动时会直接报错，避免运行时 `NullPointerException`；
    3. **保证对象完全初始化后再使用**：对象实例化时就已经注入了所有必需依赖，不会出现 “半初始化状态”；
    4. **天然避免循环依赖的设计问题**：如果是构造器注入的循环依赖，Spring 容器启动时会直接报错（`BeanCurrentlyInCreationException`），倒逼开发者优化架构（虽然 Setter 注入能解决循环依赖，但构造器注入能从设计上避免）；
    5. **便于单元测试**：不需要启动 Spring 容器，直接 `new` 对象传入依赖即可测试。

    ## 缺点

    - 如果依赖的 Bean 很多，构造器会非常臃肿（但这其实是好事，说明类的职责过多，违反了单一职责原则，应该拆分）。

    ## 适用场景

    - 依赖是**必需的**、**不可变的**；
    - 生产环境优先使用。

    ‍

    # Setter 方法注入

    **定义**

    通过 Bean 的 **Setter 方法**注入依赖，Spring 容器在实例化 Bean 后，会调用 Setter 方法设置依赖。

    **代码示例**

    ```java
     @Service
     public class UserService {
         private UserDao userDao;
         private RoleDao roleDao;
     ​
         @Autowired
         public void setUserDao(UserDao userDao) {
             this.userDao = userDao;
         }
     ​
         @Autowired(required = false) // 支持可选依赖：如果没有 RoleDao，就不注入
         public void setRoleDao(RoleDao roleDao) {
             this.roleDao = roleDao;
         }
     }
    ```
    ## 优点

    1. **灵活性高**：可以随时修改依赖（比如动态切换数据源）；
    2. **支持可选依赖**：通过 `@Autowired(required = false)`，如果容器中没有对应的 Bean，就不注入（不会报错）；
    3. **避免构造器臃肿**：如果依赖很多，Setter 方法比构造器更清晰。

    ## 缺点

    1. **依赖可能为空**：如果忘记注入，或者 `required = false`​ 但代码里没做判空，会出现运行时 `NullPointerException`；
    2. **对象可能处于半初始化状态**：先实例化 Bean，再调用 Setter 方法注入依赖，中间状态不安全；
    3. **依赖可变**：无法用 `final` 修饰，后续可能被修改，带来线程安全问题。

    ## 适用场景

    - 依赖是**可选的**；
    - 需要**动态修改依赖**；
    - 配合构造器注入使用（必需依赖用构造器，可选依赖用 Setter）。

    ‍

    # 特殊场景 Lookup 方法注入

    **定义：**

    用于解决  **“单例 Bean 依赖原型 Bean”**  的场景：

    - 单例 Bean 只会创建一次，原型 Bean 每次调用 `getBean()` 都会创建新实例；
    - Lookup 方法注入可以让单例 Bean 每次调用方法时，都获取一个新的原型 Bean。

    **代码示例：**

    ```java
     @Service
     public class SingletonService {
         // 1. 定义一个抽象方法，返回原型 Bean
         @Lookup
         public PrototypeService getPrototypeService() {
             // Spring 会在运行时动态生成子类，覆盖这个方法，返回新的原型 Bean
             return null;
         }
     ​
         public void doSomething() {
             // 2. 每次调用 getPrototypeService()，都会获取一个新的原型 Bean
             PrototypeService prototype = getPrototypeService();
             System.out.println("原型 Bean 的地址：" + prototype);
         }
     }
     ​
     @Service
     @Scope("prototype") // 原型 Bean
     public class PrototypeService {
     }
    ```
    #### 适用场景

    - 单例 Bean 需要**每次都获取新的原型 Bean**。

    # 总结：set注入 与 字段注入

    1. 字段注入在代码简洁性上完胜，这也是它最受开发者欢迎的原因。
    2. 两者都有依赖被修改的风险，Setter 注入的风险更直观，字段注入的风险更隐蔽。
    3. **依赖为空的风险：** Setter 注入可以在注入阶段就校验依赖，字段注入只能在运行时发现问题，Setter 注入更安全。
    4. 半初始化状态的风险：两者都**存在半初始化状态的风险**（对象已实例化，但依赖未注入完成），但表现一致：

       - 都是先实例化 Bean，再注入依赖；
       - 如果在 `@PostConstruct`​ 里启动异步线程或暴露对象，都可能导致半初始化对象被误用，抛出 `NullPointerException`。

         - @PostConstruct  处于Bean初始化的中间状态，还没有初始化完全
    5. 单元测试的便利性：Setter 注入的单元测试便利性远优于字段注入，这是生产环境不推荐字段注入的核心原因之一。
    6. 设计原则符合性：Setter 注入更符合设计原则，字段注入会增加类与 Spring 的耦合度。
    7. 动态修改依赖的能力：Setter 注入的灵活性更高，支持动态修改依赖。

    ‍

    ​`@PostConstruct`​ 回调**处于 Bean 初始化的中间阶段**，此时：

    - ✅ **依赖注入已完成**（`@Autowired` 字段已填充）
    - ❌ **AOP 代理尚未生成**（如果该 Bean 需要 AOP 增强，例如 `@Transactional`）
    - ❌ **​`InitializingBean.afterPropertiesSet()`​** ​ **尚未执行**
    - ❌ **自定义** **​`init-method`​**​ **尚未执行**

    因此，在 `@PostConstruct`​ 中启动异步线程或将当前对象（`this`​）暴露给外部，会导致其他线程或外部代码在**Bean 完全初始化之前**就访问该对象，从而可能触发 `NullPointerException`。


[^7]: # Bean 作用域

    Spring Bean 一共有 **6 种作用域**，分为两类：通用作用域和 Web 专用作用域。

    通用作用域有 2 种：

    1. singleton：默认作用域，整个 IoC 容器里只有一个实例，所有依赖注入拿到的都是同一个对象。适合无状态的 Service、DAO 层组件。
    2. prototype：每次从容器获取都会 new 一个新实例，容器只负责创建，不管销毁。适合有状态的对象，比如用户会话相关的数据封装。

    Web 专用作用域有 4 种，只能在 ...

    ### 1. 通用作用域（2 种）

    1. **singleton（默认）** 整个 IoC 容器中只有一个实例，所有依赖注入和 `getBean` 获取的都是同一个对象。适合无状态的 Service、DAO 层组件。容器启动时（非懒加载）即创建，容器关闭时销毁。
    2. **prototype**每次从容器获取（如 `@Autowired`​、`getBean`​）都会创建一个新实例。容器只负责创建，不负责销毁（即不会调用 `@PreDestroy`​ 或 `DisposableBean` 方法）。适合有状态的、非线程安全的对象（如用户会话数据载体）。

    ### 2. Web 专用作用域（4 种，仅在 `WebApplicationContext` 中生效）

    1. **request**每个 HTTP 请求对应一个 Bean 实例。同一请求内的多次获取返回同一个 Bean，不同请求相互独立。常用于请求上下文信息（如当前用户、请求参数包装）。
    2. **session**每个 HTTP Session 对应一个 Bean 实例。同一会话内的多次请求共享同一个 Bean，不同会话相互独立。适合存储用户登录信息、购物车等。
    3. **application**每个 `ServletContext`​（整个 Web 应用）只有一个实例。作用域比 `session` 更大，类似单例但限于 Web 环境，常用于全局配置、共享数据。
    4. **websocket**每个 WebSocket 会话对应一个 Bean 实例（Spring 4.2+ 引入）。在 WebSocket 连接生命周期内有效。

    ### 3. 自定义作用域

    Spring 还支持通过实现 `Scope`​ 接口注册自定义作用域，例如实现线程级作用域（`ThreadScope`）、租户作用域等。

    ### 4. 使用方式

    - XML：`<bean scope="prototype">`
    - 注解：`@Scope("prototype")`​ 或 `@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)`
    - 自定义：`@Scope(value = "myScope", proxyMode = ScopedProxyMode.TARGET_CLASS)`

    ### 5. 注意事项

    - **prototype 的销毁**：Spring 不管理原型 Bean 的完整生命周期，如果需要清理资源，应自行处理（如实现 `BeanPostProcessor` 或在业务代码中显式调用销毁方法）。
    - **Web 作用域代理**：若单例 Bean 注入 `request`​/`session`​ 作用域的 Bean，需使用 `<aop:scoped-proxy/>`​ 或 `@Scope(proxyMode = ScopedProxyMode.INTERFACES/TARGET_CLASS)`，否则注入的是固定对象，无法随请求/会话变化。
    - **多线程环境**：`singleton`​ 必须设计为线程安全；`prototype` 通常无需考虑共享竞争。

    > 以上是 Spring 5.x 及之前版本的作用域列表。Spring 6 基于新版本 Jakarta Servlet API 同样支持这些作用域。
    >

    ‍

    ‍

    ---

    # Spring Bean 作用域详解：6 种作用域、特点与场景

    Spring Bean 的作用域（Scope）定义了 **Bean 在 Spring 容器中的 “生命周期范围” 和 “实例数量”** —— 不同的作用域会创建不同数量的 Bean 实例，且实例的存活时间也不同。

    这是 Spring 面试的**高频基础题**，核心需掌握 **2 种核心作用域（Spring 容器通用）**  + **4 种 Web 扩展作用域**，共 6 种。

    ## 一、总览：6 种 Bean 作用域

    |作用域名称|英文|适用环境|核心含义|实例数量|
    | ------------| -------------| ----------------| ----------------------------------------------| --------------------------|
    |**单例**|singleton|所有环境|整个 Spring 容器中只有一个实例（默认作用域）|1 个|
    |**原型**|prototype|所有环境|每次获取 / 注入 Bean 时都创建一个新实例|N 个（每次调用新建）|
    |**请求**|request|Web 环境|每个 HTTP 请求创建一个新实例|每个请求 1 个|
    |**会话**|session|Web 环境|每个 HTTP Session 创建一个新实例|每个会话 1 个|
    |**应用**|application|Web 环境|整个 ServletContext 生命周期内只有一个实例|1 个（Web 应用级）|
    |**WebSocket**|websocket|WebSocket 环境|每个 WebSocket 会话创建一个新实例|每个 WebSocket 会话 1 个|

    ## 二、核心作用域详解

    （Spring 容器通用，面试必背）

    ### 1. singleton（单例，默认作用域）

    #### 核心定义

    整个 Spring IoC 容器中**只有一个 Bean 实例**，所有对该 Bean 的请求（`getBean()` 或依赖注入）都会返回同一个共享实例。

    #### 特点

    - **实例数量**：1 个；
    - 创建时机：

      - 默认：容器启动时（非懒加载）创建；
      - 懒加载（`@Lazy`​）：第一次调用 `getBean()` 或注入时创建；
    - **存活时间**：从容器启动到容器关闭；
    - **存储位置**：Spring 一级缓存 `singletonObjects`（之前讲过的单例保证机制）。

    #### 适用场景

    - 无状态的 Bean（如 Controller、Service、Repository）—— 这些 Bean 没有可变的成员变量，多线程共享不会有线程安全问题；
    - 资源消耗大的 Bean（如数据库连接池、配置管理器）—— 避免重复创建浪费资源。

    #### 代码示例

    ```
     // 默认 singleton 作用域，可省略 @Scope
     @Service
     public class UserService {
         // 无状态，天然线程安全
         public User getUserById(Long id) {
             return userDao.findById(id);
         }
     }
     ​
     // 显式指定 singleton
     @Service
     @Scope("singleton") // 或 @Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
     public class ConfigManager {
     }
    ```
    ### 2. prototype（原型）

    #### 核心定义

    每次调用 `getBean()`​ 或**依赖注入**时，都会创建一个**全新的 Bean 实例**—— 相当于每次都 `new` 一个新对象。

    #### 特点

    - **实例数量**：N 个（每次调用新建）；
    - **创建时机**：每次调用 `getBean()` 或注入时；
    - **存活时间**：从创建到被 GC 回收（容器不管理原型 Bean 的销毁）；
    - **存储位置**：不存入 Spring 缓存，每次新建后直接返回。

    #### 关键注意事项

    - **容器不管理销毁**：原型 Bean 的 `@PreDestroy`​/`destroy-method` 不会被容器调用，需开发者手动清理资源；
    - **单例依赖原型的坑**：如果单例 Bean 依赖原型 Bean，原型 Bean 只会在单例 Bean 初始化时注入一次，后续不会更新 —— 需用 `ObjectFactory`​/`Provider` 解决（之前讲过的 ObjectFactory 场景）。

    #### 适用场景

    - 有状态的 Bean（如包含可变成员变量的业务对象）—— 每个线程 / 请求使用独立实例，避免线程安全问题；
    - 需要频繁创建且销毁的对象（如临时数据容器、表单对象）。

    #### 代码示例

    ```
     // 原型作用域
     @Service
     @Scope("prototype") // 或 @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
     public class OrderForm {
         // 可变成员变量：每个实例独立
         private Long orderId;
         private List<Item> items;
     }
     ​
     // 单例依赖原型：用 ObjectFactory 解决“只注入一次”的问题
     @Service
     public class OrderService {
         @Autowired
         private ObjectFactory<OrderForm> orderFormFactory;
     ​
         public void createOrder() {
             // 每次调用 getObject() 都创建新的原型 Bean
             OrderForm form = orderFormFactory.getObject();
             form.setOrderId(1L);
         }
     }
    ```
    ## 三、Web 扩展作用域详解（仅 Web 环境生效）

    以下 4 种作用域仅在 **Web 应用环境**（如 Spring MVC、Spring Boot Web）下生效，需配合 `WebApplicationContext` 使用。

    ### 3. request（请求）

    #### 核心定义

    每个 **HTTP 请求** 创建一个新的 Bean 实例 —— 同一个请求内共享该实例，不同请求使用不同实例。

    #### 特点

    - **实例数量**：每个 HTTP 请求 1 个；
    - **创建时机**：HTTP 请求到达时；
    - **存活时间**：从请求开始到请求结束；
    - **存储位置**：`HttpServletRequest` 的属性中。

    #### 适用场景

    - 存储请求级别的数据（如当前请求的用户信息、请求参数上下文）。

    #### 代码示例

    ```
     // request 作用域
     @Component
     @Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
     // proxyMode：解决单例 Bean 依赖 request Bean 的代理问题
     public class RequestContext {
         private Long currentUserId;
         // Getter/Setter
     }
     ​
     // 单例 Controller 依赖 request Bean
     @RestController
     public class UserController {
         @Autowired
         private RequestContext requestContext;
     ​
         @GetMapping("/user")
         public User getUser() {
             Long userId = requestContext.getCurrentUserId(); // 每个请求独立
             return userService.getUserById(userId);
         }
     }
    ```
    ### 4. session（会话）

    #### 核心定义

    每个 **HTTP Session** 创建一个新的 Bean 实例 —— 同一个会话内共享该实例，不同会话使用不同实例。

    #### 特点

    - **实例数量**：每个 HTTP Session 1 个；
    - **创建时机**：Session 创建时；
    - **存活时间**：从 Session 创建到 Session 销毁（超时或手动销毁）；
    - **存储位置**：`HttpSession` 的属性中。

    #### 适用场景

    - 存储会话级别的数据（如当前登录用户信息、购物车）。

    #### 代码示例

    ```
     // session 作用域
     @Component
     @Scope(value = WebApplicationContext.SCOPE_SESSION, proxyMode = ScopedProxyMode.TARGET_CLASS)
     public class ShoppingCart {
         private List<Item> items = new ArrayList<>();
         public void addItem(Item item) {
             items.add(item);
         }
     }
    ```
    ### 5. application（应用）

    #### 核心定义

    整个 **ServletContext** 生命周期内只有一个实例 ——Web 应用级别的单例，作用范围比 Spring 容器的 `singleton` 更大（概念上，实际在一个 Web 应用中两者通常一致）。

    #### 特点

    - **实例数量**：1 个（Web 应用级）；
    - **创建时机**：Web 应用启动时；
    - **存活时间**：从 Web 应用启动到 Web 应用关闭；
    - **存储位置**：`ServletContext` 的属性中。

    #### 与 singleton 的区别

    - ​`singleton`：Spring 容器级别的单例（一个 Spring 容器只有一个）；
    - ​`application`：ServletContext 级别的单例（一个 Web 应用只有一个）；
    - 通常一个 Web 应用对应一个 Spring 容器，两者实例一致，但概念上 `application` 范围更大。

    #### 适用场景

    - 存储 Web 应用级别的全局数据（如应用配置、全局缓存）。

    ### 6. websocket（WebSocket 会话）

    #### 核心定义

    每个 **WebSocket 会话** 创建一个新的 Bean 实例 —— 同一个 WebSocket 连接内共享该实例。

    #### 特点

    - **实例数量**：每个 WebSocket 会话 1 个；
    - **创建时机**：WebSocket 连接建立时；
    - **存活时间**：从 WebSocket 连接建立到连接关闭；
    - **适用环境**：仅 Spring WebSocket 环境。

    #### 适用场景

    - 存储 WebSocket 会话级别的数据（如聊天房间信息、实时数据上下文）。

    ## 四、面试高频考点总结

    ### 1. 作用域设置方式

    - **注解方式**：`@Scope("作用域名称")`​ 或 `@Scope(ConfigurableBeanFactory.SCOPE_XXX)`；
    - **XML 方式**：`<bean id="xxx" class="xxx" scope="作用域名称" />`。

    ### 2. singleton 与 prototype 的核心区别

    |维度|singleton|prototype|
    | ----------| --------------------------------| --------------------------|
    |实例数量|1 个|N 个|
    |创建时机|容器启动（默认）/ 第一次调用|每次调用`getBean()`/ 注入|
    |销毁管理|容器管理（`@PreDestroy`生效）|容器不管理（需手动清理）|
    |线程安全|需开发者保证（无状态天然安全）|天然安全（每个实例独立）|

    ### 3. 单例依赖原型的解决方案

    - 使用 `ObjectFactory<T>`​/`Provider<T>` 延迟获取原型 Bean；
    - 使用 `@Lookup` 注解（方法注入）；
    - 直接注入 `ApplicationContext`​，每次调用 `getBean()` 获取。

    ## 五、总结

    Spring Bean 共有 **6 种作用域**：

    1. **核心通用**：singleton（单例，默认）、prototype（原型）；
    2. **Web 扩展**：request（请求）、session（会话）、application（应用）、websocket（WebSocket 会话）。

    核心需掌握 **singleton 与 prototype 的区别**、**Web 作用域的使用场景**，以及 **单例依赖原型的解决方案**—— 这是面试的高频考点。

    ### 一句话总结

    Spring Bean 作用域定义了 Bean 的生命周期范围和实例数量，默认 singleton 单例，prototype 每次新建，Web 环境下还有 request/session/application/websocket 四种扩展作用域，核心要区分 singleton 与 prototype 的使用场景。

    Spring 中 Bean 的作用域（Scope）定义了容器如何创建和管理 Bean 的实例。根据不同的应用场景，可以选择合适的作用域来控制 Bean 的生命周期和可见性。以下是 Spring 中支持的主要作用域：

    ## 一、核心作用域（在任何配置下都可用）

    ### 1. **singleton（单例）**

    - **定义**：每个 Spring 容器中只存在一个 Bean 实例，所有对该 Bean 的请求都返回同一个实例。
    - **默认值**：如果不指定作用域，默认就是 `singleton`。
    - **生命周期**：容器启动时创建（或第一次获取时创建，取决于是否懒加载），容器关闭时销毁。
    - **适用场景**：无状态的服务类、DAO、工具类等线程安全的对象。
    - **注意**：由于是单例，必须保证线程安全（尽量不要有可变成员变量）。

    ```
     @Component
     public class SingletonBean {
         private int counter = 0;
     ​
         public void increment() {
             counter++;
         }
     ​
         public int getCounter() {
             return counter;
         }
     }
    ```
    **测试验证**

    ```
     @SpringBootTest
     class SingletonBeanTest {
     ​
         @Autowired
         private SingletonBean bean1;
     ​
         @Autowired
         private SingletonBean bean2;
     ​
         @Test
         void testSingleton() {
             bean1.increment();
             bean2.increment();
             // 由于是同一个实例，counter 累加了两次
             System.out.println(bean1.getCounter()); // 输出 2
             System.out.println(bean2.getCounter()); // 输出 2
             System.out.println(bean1 == bean2);     // true
         }
     }
    ```
    **说明**：所有注入点得到的都是同一个实例，适合无状态服务。

    ### 2. **prototype（原型）**

    - **定义**：每次从容器中获取该 Bean 时，都会创建一个新的实例。
    - **生命周期**：容器只负责创建和初始化，**不负责销毁**（销毁回调方法不会被自动调用，需要手动释放资源）。
    - **适用场景**：有状态的对象、非线程安全的对象（如每次使用都需要新实例的购物车、命令对象等）。
    - **注意**：如果单例 Bean 依赖原型 Bean，原型 Bean 的行为会失效（因为单例 Bean 只会注入一次），需要通过 `@Lookup`​、`ObjectFactory`​ 或 `Provider` 等方式解决。

    **定义**

    ```
     @Component
     @Scope("prototype")
     public class PrototypeBean {
         private int counter = 0;
     ​
         public void increment() {
             counter++;
         }
     ​
         public int getCounter() {
             return counter;
         }
     }
    ```
    **测试验证**

    ```
     @SpringBootTest
     class PrototypeBeanTest {
     ​
         @Autowired
         private PrototypeBean bean1;
     ​
         @Autowired
         private PrototypeBean bean2;
     ​
         @Test
         void testPrototype() {
             bean1.increment(); // bean1 的 counter = 1
             bean2.increment(); // bean2 的 counter = 1（不同实例）
             System.out.println(bean1.getCounter()); // 1
             System.out.println(bean2.getCounter()); // 1
             System.out.println(bean1 == bean2);     // false
         }
     }
    ```
    **说明**：每次注入或 `getBean()`​ 都得到新实例，适合有状态对象。注意如果直接注入到单例 Bean 中，原型行为会失效（需要配合 `@Lookup`​ 或 `Provider` 解决）。

    ## 二、Web 作用域（仅在 Web 应用上下文中可用）

    这些作用域依赖于 `org.springframework.web.context.request.RequestScope`​ 等实现，需要在 web.xml 中配置 `RequestContextListener`​ 或 `RequestContextFilter` 才能生效。

    ### 3. **request**

    - **定义**：每个 HTTP 请求都会创建一个新的 Bean 实例，同一个请求内共享该实例，请求结束后销毁。
    - **适用场景**：与请求相关的数据，如请求参数封装、表单对象等。

    **定义**

    ```
    @Component
    @Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class RequestBean {
        private String requestId = UUID.randomUUID().toString();

        public String getRequestId() {
            return requestId;
        }
    }
    ```
    **为什么需要** **​`proxyMode`​**​ **？** 因为 `RequestBean` 的生命周期是每个请求，而注入它的 Controller 是单例的，所以 Spring 会创建一个代理对象，每次调用代理的方法时，代理再从当前请求中获取真正的实例。

    **使用示例**

    ```
    @RestController
    public class RequestController {

        @Autowired
        private RequestBean requestBean;  // 注入的是代理

        @GetMapping("/test")
        public String test() {
            // 每次请求，requestBean.getRequestId() 返回不同的 ID
            return "Request ID: " + requestBean.getRequestId();
        }
    }
    ```
    **验证**：打开浏览器访问 `/test`​，刷新页面，每次返回的 ID 都不同；但同一个请求内多次调用 `requestBean` 得到的是同一个实例。

    ### 4. **session**

    - **定义**：每个 HTTP Session 对应一个 Bean 实例，同一个 Session 内共享该实例，Session 过期或销毁时 Bean 也随之销毁。
    - **适用场景**：保存用户会话状态的对象，如购物车、用户信息等。

    **定义**

    ```
    @Component
    @Scope(value = "session", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class UserSession {
        private String userName;

        public String getUserName() { return userName; }
        public void setUserName(String userName) { this.userName = userName; }
    }
    ```
    **使用示例**

    ```
    @RestController
    public class SessionController {

        @Autowired
        private UserSession userSession;  // 代理

        @PostMapping("/login")
        public String login(@RequestParam String name) {
            userSession.setUserName(name);
            return "Logged in as " + name;
        }

        @GetMapping("/profile")
        public String profile() {
            return "Current user: " + userSession.getUserName();
        }
    }
    ```
    **验证**：先用 `/login?name=张三`​ 登录，再用同一浏览器的 `/profile` 能看到“张三”；换一个浏览器（新 session）则看到 null。同一 session 内共享同一个实例。

    ### 5. **application**

    （ServletContext 级别）

    - **定义**：在整个 `ServletContext`​ 生命周期内只存在一个 Bean 实例（类似于 `singleton`，但作用域在 ServletContext 层面，而非 Spring 容器层面）。适用于需要在多个 Servlet 之间共享的全局数据。
    - **适用场景**：应用配置信息、全局缓存等。

    **定义**

    ```
    @Component
    @Scope(value = "application", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class AppCounter {
        private int visitCount = 0;

        public int incrementAndGet() {
            return ++visitCount;
        }
    }
    ```
    **使用示例**

    ```
    @RestController
    public class AppController {

        @Autowired
        private AppCounter appCounter;

        @GetMapping("/visit")
        public String visit() {
            return "Total visits (application scope): " + appCounter.incrementAndGet();
        }
    }
    ```
    **验证**：所有用户、所有会话共享同一个计数器，每次访问 `/visit`​ 计数都会增加。因为 `application` 作用域对应整个 Web 应用生命周期。

    ### 6. **websocket**

    （需要 WebSocket 环境）

    - **定义**：每个 WebSocket 会话对应一个 Bean 实例，同一个 WebSocket 连接内共享该实例。
    - **适用场景**：WebSocket 通信中需要维持状态的对象（如实时聊天会话）。

    **定义**

    ```
    @Component
    @Scope(value = "websocket", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class WebSocketSessionInfo {
        private String sessionId = UUID.randomUUID().toString();

        public String getSessionId() {
            return sessionId;
        }
    }
    ```
    **说明**：每个 WebSocket 连接（会话）对应一个实例，同一个连接内共享。用法与 `session` 类似，但生命周期绑定到 WebSocket 会话而非 HTTP Session。

    ## 三、自定义作用域

    Spring 允许开发者实现 `org.springframework.beans.factory.config.Scope` 接口来创建自定义作用域。例如：

    - 实现线程范围内的 Bean（类似 `thread` 作用域，但 Spring 本身未提供内置的线程作用域）。
    - 实现批处理作业作用域（每个 Job 或 Step 共享一个实例）。

    自定义作用域后需要通过 `ConfigurableBeanFactory.registerScope()`​ 注册，然后使用 `@Scope("myScope")` 引用。

    ## 五、作用域选择建议

    |场景|推荐作用域|说明|
    | ------------------------------| ------------| -------------------------------------|
    |无状态服务（Service、DAO）|​`singleton`|性能最好，无需频繁创建对象|
    |有状态对象（每次使用新实例）|​`prototype`|避免线程安全问题|
    |Web 请求上下文中的数据|​`request`|请求隔离，安全|
    |用户会话数据|​`session`|用户级别隔离|
    |应用全局配置|​`application`|类似单例但生命周期在 ServletContext|

    ## 六、注意事项

    1. **单例 Bean 依赖原型 Bean 的问题**：原型 Bean 注入到单例 Bean 后，由于单例只初始化一次，原型也会被固定下来，无法每次获取新实例。解决方案包括 `@Lookup`​、`ObjectFactory`​、`Provider`​ 或 `Scoped Proxy`。
    2. **原型 Bean 的销毁**：容器不会调用原型 Bean 的 `@PreDestroy` 方法，需要自行管理资源释放。
    3. **Web 作用域需要额外配置**：在 Spring Boot 中，由于内置了 `RequestContextListener`，通常无需额外配置；但在传统 Web 应用中需要手动配置。
    4. **作用域代理**：当将一个短生命周期的 Bean（如 `request`​）注入到一个长生命周期的 Bean（如 `singleton`​）时，需要设置 `proxyMode`，否则会抛出异常。代理模式会暴露一个代理对象，每次调用代理的方法时都会从真正的目标作用域获取实例。

    了解这些作用域能帮助你在合适的场景下合理管理 Bean 的生命周期，从而提升应用性能和代码清晰度。

    ‍


[^8]: # Bean 生命周期

    Spring Bean 的生命周期是 Spring 框架的核心机制之一，它描述了 Spring 容器如何创建、初始化、使用和销毁一个 Bean 的完整过程。理解 Bean 的生命周期有助于我们更好地利用 Spring 提供的扩展点，进行自定义处理（如属性注入、代理增强等）。

    下面我将从 **宏观流程** 和 **具体扩展点** 两个维度，详细阐述 Spring Bean 的生命周期。

    实例化、属性注入、初始化、使用和销毁。Spring 容器（`ApplicationContext`​）负责管理整个过程，并在各个阶段提供扩展点（如 `BeanPostProcessor`​、`InitializingBean`​、`@PostConstruct` 等）。

    ### 常见陷阱与最佳实践

    1. **不要在**  **​`@PostConstruct`​**​ **中暴露** **​`this`​**​ **或启动异步任务**因为此时 Bean 可能尚未完成 AOP 代理生成或某些后置处理，容易导致 `NullPointerException`。
    2. ​ **​`@PostConstruct`​**​ **方法中抛出异常会导致 Bean 创建失败**如果初始化失败，Spring 会抛出 `BeanCreationException`，应用可能无法启动。
    3. **原型 Bean 的**  **​`@PreDestroy`​**​ **不会被调用**容器不管理原型 Bean 的完整生命周期，销毁需要手动处理。
    4. ​ **​`@Lazy`​**​ **只能作用于单例 Bean 的首次创建**对于原型 Bean 无效（每次创建本身就是“延迟”）。
    5. **多个生命周期方法的执行顺序**

       - 初始化：`@PostConstruct`​ → `afterPropertiesSet()`​ → 自定义`initMethod`
       - 销毁：`@PreDestroy`​ → `destroy()`​ → 自定义`destroyMethod`

    完整示例

    ```
     @Component
     public class LifecycleBean implements InitializingBean, DisposableBean {
     ​
         @PostConstruct
         public void postConstruct() {
             System.out.println("1. @PostConstruct");
         }
     ​
         @Override
         public void afterPropertiesSet() throws Exception {
             System.out.println("2. InitializingBean.afterPropertiesSet()");
         }
     ​
         public void customInit() {
             System.out.println("3. 自定义 init-method");
         }
     ​
         @PreDestroy
         public void preDestroy() {
             System.out.println("4. @PreDestroy");
         }
     ​
         @Override
         public void destroy() throws Exception {
             System.out.println("5. DisposableBean.destroy()");
         }
     ​
         public void customDestroy() {
             System.out.println("6. 自定义 destroy-method");
         }
     }
     ​
     @Configuration
     public class Config {
         @Bean(initMethod = "customInit", destroyMethod = "customDestroy")
         public LifecycleBean lifecycleBean() {
             return new LifecycleBean();
         }
     }
    ```
    **输出（初始化）** ：

    ```
     1. @PostConstruct
     2. InitializingBean.afterPropertiesSet()
     3. 自定义 init-method
    ```
    **输出（销毁）** ：

    ```
     4. @PreDestroy
     5. DisposableBean.destroy()
     6. 自定义 destroy-method
    ```
    ### 面试高频追问

    **Q：**​ **​`@PostConstruct`​**​ **和** **​`afterPropertiesSet`​**​ **的区别？** A：前者是 JSR-250 标准，后者是 Spring 特有；执行顺序上 `@PostConstruct`​ 先于 `afterPropertiesSet`​。两者都用于初始化，但 `@PostConstruct` 更通用（不依赖 Spring）。

    **Q：**​**​`BeanPostProcessor`​**​ **和** **​`BeanFactoryPostProcessor`​**​ **的区别？** A：`BeanFactoryPostProcessor`​ 操作的是 Bean 定义（`BeanDefinition`​），在 Bean 实例化前执行；`BeanPostProcessor` 操作的是 Bean 实例，在实例化后、初始化前后执行。

    **Q：AOP 代理是在哪个阶段生成的？** A：通常在 `BeanPostProcessor.postProcessAfterInitialization`​ 阶段，由 `AbstractAutoProxyCreator` 根据切点匹配决定是否生成代理。

    **Q：如果一个 Bean 实现了** **​`InitializingBean`​**​ **同时又指定了** **​`init-method`​**​ **，顺序如何？** A：先执行 `afterPropertiesSet()`​，后执行 `init-method`。

    **Q：**​**​`afterPropertiesSet()`​** ​ **和**  **​`@PostConstruct`​**​ **谁先执行？** A：`@PostConstruct`​ 先执行，然后是 `afterPropertiesSet()`​，最后是自定义 `init-method`。

    **Q：如果一个 Bean 同时实现了** **​`InitializingBean`​**​ **和使用了**  **​`@PostConstruct`​**​ **，会重复执行吗？** A：不会重复，都会执行，顺序如上述表格。

    **Q：**​ **​`@PostConstruct`​**​ **方法抛出异常会怎样？** A：Bean 创建失败，容器抛出 `BeanCreationException`​，后续的 `afterPropertiesSet()`​ 和 `init-method` 不会执行。

    **Q：**​ **​`@PreDestroy`​**​ **方法抛出异常会影响其他 Bean 的销毁吗？** A：不会，异常会被记录，但其他 Bean 的销毁继续执行。

    ### 一、宏观流程概览

    Spring Bean 的生命周期大致可以分为以下几个阶段：

    1. **实例化**：通过反射创建一个 Bean 的实例（此时对象还只是“空壳”，属性未赋值）。
    2. **属性赋值**：为 Bean 实例填充属性（依赖注入）。
    3. **初始化**：执行一些初始化逻辑，让 Bean 达到可用状态。
    4. **使用**：Bean 在容器中存活，供应用程序调用。
    5. **销毁**：容器关闭时，销毁 Bean，释放资源。

    对于**单例（Singleton）** Bean，生命周期完全由容器管理；对于**原型（Prototype）** Bean，容器只负责创建和初始化，不负责销毁，销毁由调用方负责。

    ### 二、详细步骤分解（以常用的 `ApplicationContext` 容器为例）

    假设我们有一个普通的 Bean，实现了各种 Aware 接口和自定义初始化方法，它的生命周期将经历以下步骤：

    #### 1. 实例化

    - Spring 容器通过反射（使用构造方法）创建 Bean 的实例。此时对象已经存在，但所有属性都为默认值（或 null）。

    #### 2. 属性赋值（依赖注入）

    - 容器根据配置（XML、注解或 Java Config）对 Bean 中的属性进行赋值，包括通过 Setter 方法、字段直接注入等。

    #### 3. 检查并调用一系列 Aware 接口

    如果 Bean 实现了某些 Aware 接口，Spring 会在属性赋值之后、初始化之前调用对应的回调方法，让 Bean 获取容器资源：

    - ​`BeanNameAware`​：调用 `setBeanName()`，传入 Bean 在容器中的名字。
    - ​`BeanClassLoaderAware`​：调用 `setBeanClassLoader()`，传入类加载器。
    - ​`BeanFactoryAware`​：调用 `setBeanFactory()`，传入 BeanFactory 实例（仅在 BeanFactory 容器中调用）。
    - ​`ApplicationContextAware`​：调用 `setApplicationContext()`，传入 ApplicationContext 实例（在 ApplicationContext 容器中调用，且优先级高于 BeanFactoryAware）。
    - 其他 Aware 接口：如 `EnvironmentAware`​、`ResourceLoaderAware`​、`MessageSourceAware` 等（取决于容器类型）。

    #### 4. BeanPostProcessor 的前置处理（`postProcessBeforeInitialization`）

    - 容器遍历所有注册的 `BeanPostProcessor`​，调用它们的 `postProcessBeforeInitialization` 方法。这是 Spring 对外提供的一个非常重要的扩展点，允许在初始化前对 Bean 进行包装或修改（例如生成代理对象、修改属性等）。
    - 常见应用：`ApplicationContextAwareProcessor` 就是通过这个阶段调用 Aware 接口的（实际上 Aware 接口的调用也常在此处实现）。

    #### 5. 执行初始化

    初始化阶段按照以下顺序进行：

    - ​**​`InitializingBean`​**​ **接口**：如果 Bean 实现了 `InitializingBean`​，则调用其 `afterPropertiesSet()` 方法。
    - **自定义 init-method**：如果在配置中指定了 `init-method`​（或通过 `@Bean(initMethod = "...")`），则通过反射调用该方法。

    #### 6. BeanPostProcessor 的后置处理（`postProcessAfterInitialization`）

    - 容器再次遍历所有 `BeanPostProcessor`​，调用它们的 `postProcessAfterInitialization` 方法。此时 Bean 已经初始化完成，可以在此阶段对 Bean 进行最终的代理或包装。
    - 例如 Spring AOP 就是通过 `AbstractAutoProxyCreator`（一个 BeanPostProcessor）在此阶段为 Bean 生成代理对象。

    #### 7. Bean 就绪

    - 经过上述步骤，Bean 已经完全初始化，可以正常使用了。对于单例 Bean，它会被缓存在容器中（如 `singletonObjects` 一级缓存）。

    #### 8. 销毁

    当容器关闭时，对于单例 Bean，会按照以下顺序进行销毁：

    - ​**​`DisposableBean`​**​ **接口**：如果 Bean 实现了 `DisposableBean`​，则调用其 `destroy()` 方法。
    - **自定义 destroy-method**：如果配置了 `destroy-method`​（或通过 `@Bean(destroyMethod = "...")`），则通过反射调用该方法。
    - 需要注意的是，原型 Bean 的销毁不由容器管理，容器不会调用上述销毁方法。

    ### 三、流程图展示

    以下流程图直观地展示了上述步骤：

    text

    ```
     Spring容器启动
         ↓
     实例化Bean对象（构造方法）
         ↓
     属性赋值（依赖注入）
         ↓
     检查Aware接口并调用对应方法（如BeanNameAware、ApplicationContextAware等）
         ↓
     BeanPostProcessor前置处理（postProcessBeforeInitialization）
         ↓
     执行初始化：
       ├─ 调用InitializingBean.afterPropertiesSet()
       └─ 调用自定义init-method
         ↓
     BeanPostProcessor后置处理（postProcessAfterInitialization）
         ↓
     Bean初始化完成，可供使用
         ↓
     （容器运行中...）
         ↓
     容器关闭 → 执行销毁：
       ├─ 调用DisposableBean.destroy()
       └─ 调用自定义destroy-method
    ```
    ### 四、关键扩展点详解

    #### 1. BeanPostProcessor

    - **作用**：对容器中所有 Bean 进行统一的后处理，常用于生成代理、修改 Bean 属性等。
    - **方法**：

      - ​`postProcessBeforeInitialization`：在 Bean 初始化之前调用。
      - ​`postProcessAfterInitialization`：在 Bean 初始化之后调用。
    - **典型应用**：AOP 代理生成、属性解析（如 `@Autowired`​ 的解析器 `AutowiredAnnotationBeanPostProcessor`）。

    #### 2. BeanFactoryPostProcessor

    - **作用**：在 Bean 实例化之前，对 Bean 的定义信息（`BeanDefinition`）进行修改。它与 BeanPostProcessor 不同，BeanPostProcessor 作用于 Bean 实例化之后。
    - **典型应用**：`PropertySourcesPlaceholderConfigurer`​ 解析占位符 `${...}`。

    #### 3. Aware 接口族

    - 让 Bean 感知到容器的存在，获取容器资源。
    - 包括 `BeanNameAware`​、`BeanFactoryAware`​、`ApplicationContextAware`​、`EnvironmentAware` 等。

    #### 4. 初始化与销毁回调

    - 三种实现方式（推荐顺序：注解 \> 接口 \> XML）：

      - 注解：`@PostConstruct`​、`@PreDestroy`（JSR-250 规范）。
      - 接口：`InitializingBean`​、`DisposableBean`。
      - 配置：`init-method`​、`destroy-method`。

    ### 五、作用域对生命周期的影响

    - **Singleton**：生命周期完全由容器管理，Bean 在容器启动时创建（默认饿汉式，也可配置懒加载），容器关闭时销毁。
    - **Prototype**：每次获取时容器都会创建一个新实例，但创建后容器不再管理它的生命周期，销毁由调用方负责（容器不会调用 destroy 方法）。
    - 其他作用域（如 Request、Session、Application）：生命周期与对应的 Web 域绑定，由容器负责创建和销毁。

    ### 六、总结

    Spring Bean 的生命周期是一个从实例化到销毁的完整过程，其中穿插了多个扩展点，允许开发者在不同阶段介入，实现自定义逻辑。理解这些阶段和扩展点，对于深入使用 Spring、解决复杂问题以及阅读源码都至关重要。

    在实际面试中，如果能结合一些源码细节（例如 `AbstractAutowireCapableBeanFactory`​ 的 `doCreateBean` 方法），会更显深度。以上就是我对 Spring Bean 生命周期的理解。


[^9]: # 单例 Bean

    多线程下单例bean 唯一

    Spring 早已通过**并发安全的容器 + 状态标记的原子性 + 双重检查锁** 解决了这个问题 —— 我之前的示例简化了源码，现在补全核心并发控制逻辑：

    ### 1. 单例逻辑

    1. 在一级缓存中看是否已存在对象，存在即返回，不存在则开始创建
    2. 加锁(synchronized 可重入)
    3. 再次检查是否存在一级缓存，存在返回，不存在继续创建
    4. 检查是否有创建标记，有则抛异常，无则继续创建
    5. 标记正在创建
    6. 创建对象：实例化 Bean 后（即调用构造器后）使用ObjectFactory封装，放入三级缓存，再进行属性注入初始化，生成对象（没有循环依赖和AOP时，不会再调ObjectFactory）
    7. 取消创建标记
    8. 移除三级缓存，放入一级缓存

    ```
     // 伪代码逻辑，体现核心步骤
     public Object getSingleton(String beanName) {
         // 1. 一级缓存（已完成初始化的单例池）
         Object singleton = singletonObjects.get(beanName);
         if (singleton == null) {
             synchronized (singletonObjects) {
                 // 2. 双重检查一级缓存
                 singleton = singletonObjects.get(beanName);
                 if (singleton == null) {
                     // 3. 检查是否正在创建（循环依赖检测）
                     if (singletonsCurrentlyInCreation.contains(beanName)) {
                         throw new BeanCurrentlyInCreationException(beanName);
                     }
                     // 4. 标记正在创建
                     singletonsCurrentlyInCreation.add(beanName);
                     try {
                         // 5. 实例化 Bean（调用构造器）
                         Object rawBean = createBeanInstance(beanName);
                         // 6. 将 ObjectFactory 放入三级缓存（用于提前暴露）
                         addSingletonFactory(beanName, () -> getEarlyBeanReference(rawBean));
                         
                         // 7. 属性注入（填充依赖，可能触发循环依赖，从三级缓存获取早期引用）
                         populateBean(rawBean);
                         // 8. 初始化（如 @PostConstruct, afterPropertiesSet）
                         Object initializedBean = initializeBean(rawBean);
                         
                         // 9. 移除三级缓存，将最终对象放入一级缓存
                         removeSingletonFactory(beanName);
                         singletonObjects.put(beanName, initializedBean);
                     } finally {
                         // 10. 移除创建标记
                         singletonsCurrentlyInCreation.remove(beanName);
                     }
                 }
             }
         }
         return singleton;
     }
    ```
    ### 2. 多线程安全的核心保障（3 层防护）

    #### （1）第一层：全局锁 `synchronized (this.singletonObjects)`

    ​`singletonObjects`​ 是 `ConcurrentHashMap`​，但 Spring 并没有依赖它的并发安全，而是在 `getSingleton`​ 方法外层加了 **​`synchronized`​**​ **锁**（锁对象是 `singletonObjects` 本身），保证：

    - 同一时间只有一个线程能进入 “创建 Bean” 的逻辑；
    - 即使多个线程同时调用 `getBean("userService")`，也会排队执行，不会并发创建 Bean。

    #### （2）第二层：状态标记 `singletonsCurrentlyInCreation` 的原子性

    ​`singletonsCurrentlyInCreation`​ 是 `Collections.newSetFromMap(new ConcurrentHashMap<>(16))`​—— 本质是**并发安全的 Set**，`beforeSingletonCreation(beanName)` 会原子性地将 Bean 名称加入这个 Set，作用是：

    - 防止 “循环依赖 + 多线程” 场景下的重复创建；
    - 如果线程 A 正在创建 `userService`​，线程 B 调用 `getBean("userService")`​ 时，会检测到 `singletonsCurrentlyInCreation`​ 中已有 `userService`，不会重复进入创建逻辑，而是等待线程 A 创建完成后从缓存获取。

    #### （3）第三层：双重检查锁（DCL）

    先查一级缓存 `singletonObjects`，如果已有实例直接返回 —— 即使锁释放后，后续线程也能直接从缓存获取，避免重复创建。

    ### 3. 结论：多线程下绝对安全

    Spring 通过 “全局锁 + 并发安全的状态标记 + 双重检查锁”，保证了**无论多少线程同时调用** **​`getBean()`​** ​ **，单例 Bean 只会被创建一次**，你的顾虑是多余的 —— 我之前的示例简化了锁逻辑，导致你产生了疑问，抱歉。

    ## 二、再解答：ThreadLocal 的 remove () 为什么不加 @PreDestroy？

    你问 “既然 ThreadLocal 必须 remove ()，为啥不用 @PreDestroy 清理？”—— 核心原因是： **@PreDestroy 执行的时机和 ThreadLocal 所属的线程生命周期不匹配**，用 @PreDestroy 清理 ThreadLocal 会导致 “清理无效” 甚至 “内存泄漏”。

    ### 1. 先明确：ThreadLocal 内存泄漏的根源

    ThreadLocal 的内存泄漏是因为：

    - ThreadLocal 存储在 `Thread`​ 的 `ThreadLocalMap`​ 中，`ThreadLocalMap` 的 key 是 ThreadLocal 的弱引用；
    - 如果 Thread 是**线程池中的核心线程**（如 Tomcat 的工作线程、Spring 的异步线程池），线程会长期存活，`ThreadLocalMap` 中的 value 会一直引用业务对象（如 User），导致内存泄漏；
    - 只有调用 `remove()`​ 才能彻底清除 `ThreadLocalMap` 中的 key-value，释放内存。

    ### 2. @PreDestroy 为什么不适用？

    #### （1）@PreDestroy 的执行时机

    ​`@PreDestroy`​ 是 **Bean 的销毁回调**，只有当 Bean 被销毁时（如 Spring 容器关闭）才会执行 —— 而单例 Bean 的生命周期和容器一致，容器不关闭，`@PreDestroy` 永远不会执行。

    #### （2）场景矛盾：ThreadLocal 是 “线程级”，@PreDestroy 是 “Bean 级”

    举个 Web 场景的例子：

    - Tomcat 的工作线程是线程池（核心线程长期存活）；
    - 每个 HTTP 请求对应一个线程，请求处理完成后，线程会回到线程池，不会销毁；
    - 如果你在

      ```
       UserContextService
      ```
      （单例 Bean）上加

      ```
       @PreDestroy
      ```
      清理 ThreadLocal：

      java

      运行

      ```
       @Service
       public class UserContextService {
           private static final ThreadLocal<Long> CURRENT_USER_ID = new ThreadLocal<>();
       ​
           @PreDestroy // 无效！
           public void clear() {
               CURRENT_USER_ID.remove();
           }
       }
      ```
      - 这个 `clear()`​ 方法只有当 Spring 容器关闭时才会执行，而此时 Tomcat 的线程池还在，线程还存活，`ThreadLocalMap` 中的 value 早已堆积了无数请求的用户 ID，内存泄漏已经发生；
      - 正确的清理时机是**每个请求处理完成后**，而不是 Bean 销毁时。

    ### 3. ThreadLocal 正确的清理方式（按场景分类）

    #### （1）Web 场景（最常用）：用拦截器 / 过滤器清理

    在请求处理完成后（`afterCompletion`​ 阶段）调用 `remove()`，这是 Spring 项目的标准做法：

    java

    运行

    ```
     @Component
     public class UserContextInterceptor implements HandlerInterceptor {
         @Autowired
         private UserContextService userContextService;
     ​
         @Override
         public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
             // 请求处理完成后，立即清理 ThreadLocal
             userContextService.clear();
         }
     }
    ```
    #### （2）异步线程场景：用 try-finally 强制清理

    在业务代码中，用 `try-finally` 保证无论是否异常，都会清理 ThreadLocal：

    java

    运行

    ```
     @Service
     public class AsyncService {
         @Autowired
         private UserContextService userContextService;
     ​
         @Async
         public void doAsyncTask(Long userId) {
             try {
                 userContextService.setCurrentUserId(userId);
                 // 执行业务逻辑
             } finally {
                 // 最终清理，避免内存泄漏
                 userContextService.clear();
             }
         }
     }
    ```
    #### （3）特殊场景：自定义 ThreadPoolTaskExecutor 清理

    如果是自定义线程池，可以重写 `afterExecute` 方法，在任务执行后清理：

    java

    运行

    ```
     @Bean
     public ThreadPoolTaskExecutor taskExecutor() {
         return new ThreadPoolTaskExecutor() {
             @Override
             protected void afterExecute(Runnable r, Throwable t) {
                 super.afterExecute(r, t);
                 // 清理当前线程的 ThreadLocal
                 UserContextService.clear();
             }
         };
     }
    ```
    ### 4. 补充：有没有可能用 @PreDestroy？

    只有一种极端场景适用：Bean 是**原型作用域**，且每个 Bean 对应一个独立线程，线程结束后 Bean 被销毁 —— 但这种场景几乎不存在，原型 Bean 通常是短期使用，线程也不会和 Bean 绑定，因此 `@PreDestroy` 清理 ThreadLocal 是 “典型的错误用法”。

    ## 三、总结（核心关键点）

    ### 1. 单例 Bean 多线程安全创建的核心

    Spring 通过 `synchronized (singletonObjects)`​ 全局锁 + 并发安全的状态标记 `singletonsCurrentlyInCreation` + 双重检查锁，保证多线程下单例 Bean 只会创建一次，源码层面无并发问题。

    ### 2. ThreadLocal 清理的核心原则

    - ​`@PreDestroy` 是 “Bean 销毁时” 执行，ThreadLocal 是 “线程级” 资源，两者生命周期不匹配，无法用于清理；
    - 正确的清理时机是**线程完成当前任务后**（Web 场景用拦截器、异步场景用 try-finally、线程池场景重写 afterExecute）；
    - ​`remove()` 是必须的，否则线程池中的核心线程会导致 ThreadLocal 内存泄漏。

    ## 一句话总结

    Spring 靠全局锁保证单例 Bean 只创建一次；ThreadLocal 不能用 @PreDestroy 清理，因为 Bean 销毁时机和线程生命周期不匹配，必须在每个线程任务完成后手动清理。


[^10]: # 原型 Bean

    ## 什么是Spring原型对象？

    在Spring中，**原型对象（Prototype scope）**  是指每次从容器中获取该Bean时，容器都会创建一个**全新的实例**。

    ### 作用域对比

    |作用域|行为|适用场景|
    | --------| --------------------------------------------| --------------------------------|
    |**Singleton（单例）**|容器中只存在一个实例，每次获取都返回同一个|无状态的工具类、Service、DAO|
    |**Prototype（原型）**|每次获取都创建一个新实例|有状态的对象、非线程安全的对象|

    ### 如何定义原型Bean

    ```
     @Component
     @Scope("prototype")  // 方式1：使用注解
     public class MyPrototypeBean {
         // 每次获取都是新对象
     }
     ​
     // 方式2：XML配置
     <bean id="myBean" class="com.example.MyBean" scope="prototype"/>
    ```
    ### 原型Bean的创建流程

    1. **收到获取Bean的请求**（`getBean()`）
    2. **直接创建新实例**（通过反射调用构造函数）
    3. **执行依赖注入**（如果依赖其他Bean）
    4. **执行初始化回调**（`@PostConstruct`​、`afterPropertiesSet()`等）
    5. **返回新实例**给调用者
    6. **Spring不再管理该实例**（之后的生命周期与Spring无关）即便用@PreDestroy指定了销毁方法，Spring也不会调用，需手动调用

    ### 为什么不走缓存？

    - **单例Bean**：需要保证全局唯一，所以必须用缓存存储和复用
    - **原型Bean**：设计要求就是“每次新建”，缓存反而会破坏语义
    - **源码层面**：`AbstractBeanFactory.doGetBean()`​ 中，原型Bean直接走 `createBean()` 流程，不经过缓存检查

    ### 适合使用原型的场景：

    - **有状态的对象**：如保存用户会话信息的对象
    - **非线程安全的对象**：如SimpleDateFormat
    - **每次使用都需要新实例的场景**：如数据库连接（虽然通常用连接池）

    示例：消息处理器

    ```
     @Component
     @Scope("prototype")
     public class MessageProcessor {
         private String messageId;
         private List<String> steps = new ArrayList<>();
         
         public void process(String message) {
             this.messageId = UUID.randomUUID().toString();
             steps.add("处理消息: " + message);
             // 每个消息都有自己的处理状态
         }
         
         public List<String> getSteps() {
             return steps;
         }
     }
    ```
    ### 使用建议

    1. **不要滥用原型**：高频创建对象的场景要考虑性能
    2. **轻量级对象**适合原型，重量级对象考虑池化
    3. **结合对象池**：如果原型对象创建成本高，可以自己实现 化

    # **单例Bean依赖原型Bean**

    > 可以传递参数
    >

    ​`@Lookup`​是Spring提供的解决**单例Bean依赖原型Bean**问题的标准方案。

    ​`@Lookup`的几种写法

    ```
     @Component
     public class OrderService {
         
         // 方式1：抽象方法（最推荐）
         @Lookup
         protected abstract ShoppingCart createCart();
         
         // 方式2：具体方法但返回null
         @Lookup
         protected ShoppingCart getShoppingCart() {
             return null;  // Spring会忽略这个返回值
         }
         
         // 方式3：带参数（根据参数获取不同的Bean）
         @Lookup
         protected ShoppingCart getCartForUser(String userId) {
             return null;  // 参数可以传递给Bean的创建过程
         }
     }
    ```
    除了`@Lookup`，还有几种解决单例依赖原型问题的方式：

    ## 其他解决方案对比

    ### 方案1：ObjectFactory

    ```
     @Component
     public class SingleService {
         
         @Autowired
         private ObjectFactory<PrototypeBean> prototypeBeanFactory;
         
         public void doWork() {
             PrototypeBean bean = prototypeBeanFactory.getObject(); // 每次获取新实例
             bean.doSomething();
         }
     }
     ​
     @Component
     @Scope("prototype")
     public class PrototypeBean {
         public void doSomething() {
             System.out.println("PrototypeBean instance: " + this);
         }
     }
    ```
    ### 方案2：Scoped Proxy（代理模式）

    ```
     @Component
     @Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)
     public class PrototypeBean {
         // 这样注入到单例Bean时，实际注入的是代理对象
         // 每次调用代理对象的方法，都会转发给一个新的真实实例
         public void doSomething() {
             System.out.println("PrototypeBean instance: " + this);
         }
     }
     ​
     @Component
     public class SingleService {
         
         @Autowired
         private PrototypeBean prototypeBean; // 实际注入的是代理
         
         public void doWork() {
             // 每次调用proxy.doSomething()，都会委托给一个新的PrototypeBean实例
             prototypeBean.doSomething();
         }
     }
    ```
    ### 方案3：Provider\<T\>（推荐用于复杂场景）

    ```
     @Component
     public class OrderService {  // 单例
     ​
         @Autowired
         private Provider<ShoppingCart> cartProvider;  // 注入 Provider
     ​
         public void processOrder(String userId) {
             // 每次调用 get()，都从容器获取一个新的 ShoppingCart 实例
             ShoppingCart cart = cartProvider.get();
             cart.setUserId(userId);
             cart.addItem("商品A", 100);
             // 处理订单...
         }
     }
    ```
    ​**​`Provider`​**​ **的核心特点**

    - **标准化**：属于 `javax.inject.Provider`​（或 `jakarta.inject.Provider`），代码不耦合 Spring
    - **延迟获取**：只有在调用 `get()` 时才去容器获取实例
    - **每次新实例**：默认每次 `get()` 都返回一个新的原型 Bean

    ### 方案对比

    |方案|优点|缺点|适用场景|
    | ------| --------------------------| ------------------------| ----------------------------|
    | **@Lookup**|语义清晰，Spring官方推荐|需要写抽象方法或空方法|简单场景，明确需要每次新建|
    |**ObjectFactory**|灵活，支持条件判断|多了一个依赖|需要条件创建或延迟获取|
    |**直接getBean**|最直观|侵入性强，耦合容器|不推荐，除非不得已|
    |**Scoped Proxy**|使用透明，代码无侵入|性能开销，理解成本高|需要代理复杂接口的场景|

    # Bean 2

    ### **原型Bean（Prototype Bean）的使用场景与注意事项**

    在Spring中，原型Bean（`@Scope("prototype")`​）的特点是**每次从容器中获取时都会创建一个新实例**。与单例Bean不同，原型Bean的生命周期不受Spring完全管理（如不会自动调用销毁方法），适用于需要状态隔离或动态实例化的场景。

    ### **一、原型Bean的典型使用场景**

    #### **1. 需要维护独立状态的场景**

    **场景描述**：每个Bean实例需要持有不同的状态，且状态之间需严格隔离（避免单例Bean的共享状态问题）。

    **示例**：

    ```
     @Scope("prototype")
     @Component
     public class ShoppingCart {
         private List<Item> items = new ArrayList<>();
     ​
         public void addItem(Item item) {
             items.add(item);
         }
     ​
         public List<Item> getItems() {
             return items;
         }
     }
     ​
     // 用户请求处理
     @Controller
     public class OrderController {
         @Autowired
         private Provider<ShoppingCart> shoppingCartProvider; // 使用Provider延迟获取
     ​
         @PostMapping("/addItem")
         public String addItem(Item item) {
             ShoppingCart cart = shoppingCartProvider.get(); // 每次生成新实例
             cart.addItem(item);
             return "success";
         }
     }
    ```
    **说明**：

    - 每个用户的购物车实例独立，避免多用户操作同一购物车。
    - 使用`Provider`​（或`ObjectFactory`）延迟获取原型Bean，避免直接注入导致单例Bean持有固定实例。

    #### **2. 多线程任务处理**

    **场景描述**：每个线程需要独立的Bean实例执行任务，防止并发修改共享状态。

    **示例**：

    ```
     @Scope("prototype")
     @Component
     public class TaskProcessor {
         private String taskId;
     ​
         public void process(String taskId) {
             this.taskId = taskId;
             // 执行任务逻辑（如文件处理、数据计算）
         }
     }
     ​
     // 线程池提交任务
     @Service
     public class TaskService {
         @Autowired
         private ApplicationContext context;
     ​
         public void submitTask(String taskId) {
             TaskProcessor processor = context.getBean(TaskProcessor.class); // 每次生成新实例
             executor.submit(() -> processor.process(taskId));
         }
     }
    ```
    **说明**：

    - 每个任务使用独立的`TaskProcessor`实例，避免线程间竞争。
    - 直接通过`ApplicationContext`获取原型Bean，确保每次创建新对象。

    #### **3. 动态配置或参数化实例**

    **场景描述**：需要根据运行时参数动态创建Bean，且参数无法在Bean定义时确定。

    **示例**：

    ```
    @Scope("prototype")
    @Component
    public class ReportGenerator {
        private final String reportType;

        // 构造函数接收动态参数
        public ReportGenerator(String reportType) {
            this.reportType = reportType;
        }

        public void generate() {
            // 根据reportType生成不同报表
        }
    }

    // 通过工厂方法创建原型Bean
    @Configuration
    public class AppConfig {
        @Bean
        @Scope("prototype")
        public ReportGenerator reportGenerator(@Value("#{threadLocalReportType.get()}") String reportType) {
            return new ReportGenerator(reportType);
        }
    }
    ```
    **说明**：

    - 使用`ThreadLocal`​或请求上下文传递参数（如`reportType`）。
    - 通过`@Scope("prototype")`确保每次生成不同配置的实例。

    ### **二、原型Bean的使用注意事项**

    #### **1. 生命周期管理**

    - **Spring不管理原型Bean的销毁**：原型Bean的`@PreDestroy`​方法**不会自动调用**，需手动释放资源（如数据库连接、文件句柄）。**解决方案**：

      - 显式调用销毁方法（如`close()`）。
      - 实现`DisposableBean`接口，并在使用后手动触发。

    **示例**：

    ```
    @Scope("prototype")
    @Component
    public class FileProcessor implements DisposableBean {
        private File file;

        public void process(String path) {
            this.file = new File(path);
            // 处理文件
        }

        @Override
        public void destroy() {
            if (file != null) {
                // 释放文件资源
                file.close();
            }
        }
    }

    // 使用后手动销毁
    FileProcessor processor = context.getBean(FileProcessor.class);
    processor.process("data.txt");
    ((DisposableBean) processor).destroy(); // 手动触发
    ```
    #### **2. 避免单例Bean持有原型Bean的过时引用**

    - **问题**：若单例Bean直接注入原型Bean，单例Bean仅在初始化时获取一次原型实例，后续无法获取新实例。
    - **解决方案**：使用`Provider`​、`ObjectFactory`​或方法注入（`@Lookup`）延迟获取原型Bean。

    **错误示例**：

    ```
    @Service
    public class OrderService {
        @Autowired
        private ShoppingCart cart; // 直接注入原型Bean → cart实例固定不变
    }
    ```
    **正确方案**：

    ```
    @Service
    public class OrderService {
        @Autowired
        private Provider<ShoppingCart> cartProvider; // 使用Jakarta Inject的Provider

        public void checkout() {
            ShoppingCart cart = cartProvider.get(); // 每次生成新实例
            // 处理购物车
        }
    }
    ```
    #### **3. 性能开销**

    - **频繁创建成本**：原型Bean每次创建均需执行构造器、依赖注入、初始化等流程，可能影响性能。
    - **优化建议**：

      - 仅在必要时使用原型作用域（如确实需要状态隔离）。
      - 对于无状态工具类，优先使用单例。

    #### **4. AOP代理的陷阱**

    - **代理对象生成**：若原型Bean需要AOP代理（如`@Transactional`），Spring会为每个新实例生成代理对象。
    - **潜在问题**：

      - 代理对象可能增加内存开销。
      - 需确保代理逻辑无状态（如事务管理器本身是线程安全的）。

    **示例**：

    ```
    @Scope("prototype")
    @Service
    public class PaymentService {
        @Transactional
        public void pay(Order order) {
            // 事务操作
        }
    }

    // 每次获取的PaymentService是新的代理对象
    PaymentService service1 = context.getBean(PaymentService.class);
    PaymentService service2 = context.getBean(PaymentService.class);
    assert service1 != service2; // true
    ```
    ### **三、原型Bean与其他作用域的对比**

    |**作用域**|**特点**|**适用场景**|
    | --| ----------------------| ------------------------------|
    |**Singleton**|单例，全局唯一实例|无状态工具类、配置类、服务类|
    |**Prototype**|每次请求创建新实例|需要状态隔离、动态参数化实例|
    |**Request**|每个HTTP请求一个实例|Web应用中的用户会话数据|
    |**Session**|每个用户会话一个实例|购物车、用户偏好设置|

    ### **总结**

    - **使用场景**：原型Bean适合需要独立状态、动态参数化或多线程隔离的场景。
    - **注意事项**：

      1. 手动管理资源释放。
      2. 避免单例Bean持有固定原型实例。
      3. 警惕频繁创建的性能开销。
      4. 确保AOP代理的正确性。
    - **最佳实践**：

      - 优先使用单例，仅在必要时选择原型。
      - 通过`Provider`​或`ApplicationContext`按需获取原型实例。


[^11]: # BeanFactory

    ​`BeanFactory`​ 是 Spring IoC 容器的**核心底层接口**，定义了 IoC 容器的最基本功能 ——**Bean 的创建、获取、管理和依赖注入**，是 Spring 容器的 “心脏”。

    简单来说，它就是 Spring 用来 “生产” 和 “管理” Bean 的工厂，所有 Spring 容器（如 `ApplicationContext`）本质上都是它的实现或扩展。

    ## 核心作用

    ​`BeanFactory` 的核心职责可以概括为 4 点：

    1. **Bean 的生命周期管理**：负责 Bean 的实例化、依赖注入、初始化、 ；
    2. **Bean 的获取与查询**：通过 `getBean()`​ 系列方法获取 Bean，通过 `containsBean()` 等方法查询 Bean 的状态；
    3. **Bean 作用域的维护**：管理单例（`singleton`​）、原型（`prototype`）等不同作用域的 Bean；
    4. **依赖注入与循环依赖处理**：自动解析 Bean 之间的依赖关系，完成依赖注入，并通过三级缓存解决循环依赖。

    ## 核心实现类：`DefaultListableBeanFactory`

    ​`BeanFactory`​ 只是一个接口，Spring 内部默认使用的实现是 **​`DefaultListableBeanFactory`​**，它是最完整、最常用的 BeanFactory 实现，具备以下特点：

    1. **实现了** **​`BeanDefinitionRegistry`​**​ **接口**：可以注册和管理 `BeanDefinition`（Bean 的元数据）；
    2. **支持三级缓存**：解决循环依赖的核心实现；
    3. **支持 BeanPostProcessor、BeanFactoryPostProcessor 等扩展点**：允许在 Bean 生命周期的不同阶段进行扩展；
    4. **是 ApplicationContext 的底层基础**：所有高级容器（如 `AnnotationConfigApplicationContext`​）内部都持有一个 `DefaultListableBeanFactory` 实例，实际的 Bean 管理工作都由它完成。

    ## 面试必问：`BeanFactory`​ vs `ApplicationContext`

    > 主要区别：
    >
    > 1. 是否懒加载
    > 2. ApplicationContext 集成并扩展了 BeanFactory
    >

    这是 Spring 面试的**高频基础题**，两者的核心区别如下：

    |维度|​`BeanFactory`|​`ApplicationContext`|
    | ------| ----------------------------------------------------------------------------| ------------------------------------------------------------------------------------------------|
    |**定位**|IoC 容器的**核心底层接口**，仅定义最小功能集|IoC 容器的**高级接口**，继承并扩展了`BeanFactory`|
    |**功能**|仅支持 Bean 的基本管理（创建、获取、依赖注入）|增加了企业级功能：・国际化（`MessageSource`​）・事件发布（`ApplicationEventPublisher`​）・AOP 集成・资源加载（`ResourceLoader`​）・Web 环境支持（`WebApplicationContext`）|
    |**启动方式**|**延迟加载（Lazy-init）** ：默认只在调用`getBean()`时才创建 Bean|**非懒加载（Eager-init）** ：默认容器启动时就初始化所有非懒加载的单例 Bean|
    |**扩展点**|仅支持`BeanPostProcessor`​、`BeanFactoryPostProcessor`|自动注册更多扩展点（如`BeanFactoryPostProcessor`​、`ApplicationListener`等）|
    |**适用场景**|资源受限的环境（如移动端、嵌入式设备），或需要极致控制 Bean 生命周期的场景|绝大多数企业级开发场景（Web 应用、微服务等）|

    **一句话总结**：`BeanFactory`​ 是 “精简版的 IoC 容器”，`ApplicationContext` 是 “企业级的 IoC 容器”，后者继承了前者的所有功能，并增加了大量企业级特性，是目前开发的主流选择。

    ## 五、核心扩展点：`BeanFactoryPostProcessor`

    ​`BeanFactory`​ 提供了一个重要的扩展点 ——**​`BeanFactoryPostProcessor`​**​，允许在 **Bean 实例化之前**修改 `BeanDefinition`（Bean 的元数据），比如替换  文件中的占位符、修改 Bean 的作用域等。

    **典型示例**：`PropertyPlaceholderConfigurer`​（Spring 内置的 `BeanFactoryPostProcessor`​），用于替换 XML / 注解配置中的 `${...}` 占位符：

    ```
     <!-- 配置 PropertyPlaceholderConfigurer，替换占位符 -->
     <bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
         <property name="location" value="classpath:jdbc.properties" />
     </bean>
     ​
     <!-- 使用占位符配置数据源 -->
     <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
         <property name="url" value="${jdbc.url}" />
         <property name="username" value="${jdbc.username}" />
         <property name="password" value="${jdbc.password}" />
     </bean>
    ```
    ## 六、面试重点总结（必背）

    1. **核心定位**：`BeanFactory`​ 是 Spring IoC 容器的**核心底层接口**，定义了 Bean 管理的最小功能集；
    2. **核心实现**：`DefaultListableBeanFactory`​ 是最常用的实现，也是 `ApplicationContext` 的底层基础；
    3. **核心方法**：`getBean()` 系列方法（根据名称 / 类型获取 Bean）；
    4. **与 ApplicationContext 的区别**：底层 vs 高级、延迟加载 vs 非懒加载、功能简单 vs 企业级功能；
    5. **扩展点**：`BeanFactoryPostProcessor`​ 允许在 Bean 实例化前修改 `BeanDefinition`。

    ## 一句话总结

    ​`BeanFactory`​ 是 Spring IoC 容器的 “心脏”，负责 Bean 的创建、管理和依赖注入，`DefaultListableBeanFactory`​ 是其核心实现，而 `ApplicationContext` 是它的企业级扩展，是目前开发的主流选择。


[^12]: # ApplicationContext

    > ApplicationContext 是 Spring 的**高级容器接口**，它继承了多个底层接口，把 Bean 管理、国际化、资源加载、事件发布等能力整合到一起。
    >
    > 可以把它理解为 BeanFactory 的增强版，日常开发中打交道最多的就是它。
    >

    ​`ApplicationContext`​ 是 Spring IoC 容器的**高级接口**，它继承并扩展了 `BeanFactory`​，是企业级开发的**主流选择**—— 简单来说，`BeanFactory`​ 是 “精简版的 IoC 容器”，`ApplicationContext` 是 “企业级的 IoC 容器”，后者包含前者的所有功能，并增加了大量企业级特性。

    ## 一、核心定义与定位

    ​`ApplicationContext`​ 位于 `org.springframework.context` 包下，核心定位是：

    - **继承 BeanFactory**：拥有 BeanFactory 的所有核心能力（Bean 的创建、管理、依赖注入、循环依赖解决等）；
    - **扩展企业级功能**：增加了国际化、事件发布、资源加载、AOP 集成、Web 环境支持等特性；
    - **自动注册扩展点**：自动扫描并注册 `BeanFactoryPostProcessor`​、`BeanPostProcessor`​、`ApplicationListener` 等扩展组件，无需手动调用。

    ## 二、核心功能（对比 BeanFactory 的扩展）

    ### 1. 继承 BeanFactory 的所有基础功能

    - Bean 生命周期管理（实例化→依赖注入→初始化→销毁）；
    - 依赖注入（构造器 / Setter / 字段注入）；
    - 循环依赖解决（三级缓存）；
    - Bean 作用域管理（单例 / 原型 / 请求 / 会话等）。

    ## 总结（核心关键点）

    1. **核心定位**：`ApplicationContext`​ 是 Spring 的**高级 IoC 容器**，继承并扩展 `BeanFactory`；
    2. **核心优势**：增加了国际化、事件发布、资源加载、AOP 集成、Web 支持等企业级特性；
    3. **核心实现**：`AnnotationConfigApplicationContext`​（注解）、`ClassPathXmlApplicationContext`（XML）等；
    4. **与 BeanFactory 的区别**：底层 vs 高级、延迟加载 vs 非懒加载、功能简单 vs 企业级功能；
    5. **生命周期**：启动→BeanFactoryPostProcessor→Bean 初始化→ContextRefreshedEvent→关闭→ContextClosedEvent；
    6. **结合 FactoryBean**：可直接注入 FactoryBean 生产的对象，无需手动调用 `getObject()`。

    ### 一句话总结

    ​`ApplicationContext` 是 Spring 企业级开发的核心容器，它在 BeanFactory 的基础上增加了大量企业级特性，是目前 Spring 应用的主流选择，可结合 FactoryBean 实现灵活的对象组装。


[^13]: # FactoryBean

    ​`FactoryBean`​ 是 Spring 中一个**特殊的工厂 Bean**，用于**创建其他复杂 Bean**—— 简单来说，它是一个 “生产 Bean 的 Bean”，核心作用是**封装复杂对象的创建逻辑**，让 Spring 容器能更灵活地管理那些难以通过常规方式（如构造器、反射）创建的 Bean。

    注意：**千万不要和** **​`BeanFactory`​**​ **混淆**！`BeanFactory`​ 是 Spring IoC 容器的核心底层接口，而 `FactoryBean` 是一个用于创建 Bean 的特殊 Bean，两者名字相似但定位完全不同（面试高频混淆点）。

    ## 1. 一、核心定义与接口结构

    ​`FactoryBean`​ 是一个泛型接口，位于 `org.springframework.beans.factory` 包下，源码如下（核心部分）：

    ```
     public interface FactoryBean<T> {
         // 核心方法：返回 FactoryBean 生产的 Bean 实例
         T getObject() throws Exception;
         
         // 返回生产的 Bean 的类型
         Class<?> getObjectType();
         
         // 返回生产的 Bean 是否为单例（默认 true）
         default boolean isSingleton() {
             return true;
         }
     }
    ```
    **核心逻辑**：

    - 当 Spring 容器调用 `getBean("factoryBeanName")`​ 时，**不会返回 FactoryBean 本身**，而是返回 `FactoryBean#getObject()` 生产的对象；
    - 如果想获取 FactoryBean 本身，需要在 Bean 名称前加 `&`​ 前缀，如 `getBean("&factoryBeanName")`。

    ## 2. 二、例：自定义 FactoryBean

    假设我们有一个 `User`​ 类，创建逻辑比较复杂（比如需要从配置文件读取属性、初始化连接等），我们可以用 `FactoryBean` 封装 创建逻辑：

    ### 2.1 1. 定义目标类 `User`

    ```
     public class User {
         private Long id;
         private String name;
         // 构造器、Getter/Setter 省略
     }
    ```
    ### 2.2 2. 自定义 `UserFactoryBean`

    ```
     @Component("user") // 注册为 Bean，名称为 "user"
     public class UserFactoryBean implements FactoryBean<User> {
         // 模拟复杂的创建逻辑：从配置文件读取属性
         @Value("${user.id}")
         private Long userId;
         @Value("${user.name}")
         private String userName;
     ​
         @Override
         public User getObject() throws Exception {
             // 封装复杂的创建逻辑：这里可以是反射、动态代理、第三方库调用等
             User user = new User();
             user.setId(userId);
             user.setName(userName);
             System.out.println("UserFactoryBean：生产了一个 User 对象");
             return user;
         }
     ​
         @Override
         public Class<?> getObjectType() {
             return User.class;
         }
     ​
         @Override
         public boolean isSingleton() {
             return true; // 默认单例，生产的 User 对象只会创建一次
         }
     }
    ```
    ### 2.3 3. 使用 FactoryBean 生产的 Bean

    ```
     @Service
     public class UserService {
         // 直接注入 User 对象：Spring 会自动调用 UserFactoryBean#getObject() 生产 User
         @Autowired
         private User user;
     ​
         public void printUser() {
             System.out.println("User：" + user);
         }
     }
    ```
    ### 2.4 4. 获取 FactoryBean 本身

    如果想获取 `UserFactoryBean`​ 本身，需要加 `&` 前缀：

    ```
     @Component
     public class FactoryBeanTest implements ApplicationContextAware {
         @Override
         public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
             // 1. 获取 FactoryBean 生产的 User 对象
             User user = (User) applicationContext.getBean("user");
             System.out.println("生产的 Bean：" + user);
     ​
             // 2. 获取 FactoryBean 本身（加 & 前缀）
             UserFactoryBean factoryBean = (UserFactoryBean) applicationContext.getBean("&user");
             System.out.println("FactoryBean 本身：" + factoryBean);
         }
     }
    ```
    ## 3. 三、核心使用场景

    ​`FactoryBean`​ 的核心价值是**封装复杂对象的创建逻辑**，常见场景有：

    ### 3.1 1. 创建第三方库的复杂对象

    很多第三方库的类创建逻辑非常复杂（需要设置大量参数、初始化连接池等），无法直接通过 `@Component`​ 或 `@Bean`​ 注册，此时可以用 `FactoryBean` 封装创建逻辑。

    ### 3.2 2. 动态代理对象的创建（最经典场景：MyBatis Mapper）

    MyBatis 的 Mapper 接口没有实现类，是通过动态代理生成的，Spring 就是通过 `MapperFactoryBean`（一个 FactoryBean 实现）来管理 Mapper 接口的：

    ```
     // MyBatis 的 MapperFactoryBean 简化版逻辑
     public class MapperFactoryBean<T> implements FactoryBean<T> {
         private Class<T> mapperInterface;
     ​
         @Override
         public T getObject() throws Exception {
             // 核心逻辑：通过动态代理生成 Mapper 接口的代理对象
             return (T) Proxy.newProxyInstance(
                 mapperInterface.getClassLoader(),
                 new Class[]{mapperInterface},
                 (proxy, method, args) -> {
                     // 代理逻辑：调用 MyBatis 的 SQL 执行
                     System.out.println("执行 SQL：" + method.getName());
                     return null;
                 }
             );
         }
     ​
         @Override
         public Class<?> getObjectType() {
             return mapperInterface;
         }
     }
    ```
    这就是为什么我们在 Spring 中直接注入 Mapper 接口就能使用的原因 ——Spring 内部通过 `MapperFactoryBean` 生成了动态代理对象。

    ### 3.3 3. 装饰器 / 代理模式的应用

    如果需要对原始 Bean 进行包装（如添加日志、性能监控、事务管理），可以用 `FactoryBean` 封装装饰逻辑，返回包装后的对象。

    ## 4. 四、`FactoryBean`​ vs `BeanFactory`

    这是 Spring 面试的**高频混淆点**，必须清晰区分：

    |维度|​`FactoryBean`|​`BeanFactory`|
    | ------| ------------------------------------------------| --------------------------------------------------------|
    |**定位**|一个**特殊的工厂 Bean**，用于创建其他复杂 Bean|Spring IoC 容器的**核心底层接口**，定义了容器的基本功能|
    |**作用**|封装复杂对象的创建逻辑，生产其他 Bean|管理所有 Bean 的生命周期（创建、获取、依赖注入、销毁）|
    |**返回对象**|默认返回`getObject()`​生产的对象，加`&`返回自身|直接返回容器中的 Bean|
    |**使用场景**|创建第三方库复杂对象、动态代理对象、装饰器包装|所有 Spring 应用的基础容器|
    |**典型实现**|MyBatis 的`MapperFactoryBean`​、Spring 的`ProxyFactoryBean`|​`DefaultListableBeanFactory`|

    ## 5. 五、获取 FactoryBean 本身

    记住这个关键规则：

    - ​**​`getBean("beanName")`​** ​：返回 `FactoryBean#getObject()` 生产的对象；
    - ​**​`getBean("&beanName")`​** ：返回 FactoryBean 本身。

    这个细节在调试和扩展 Spring 时非常有用，面试中也经常被问到。

    ## 6. 六、总结

    1. **核心定义**：`FactoryBean` 是一个 “生产 Bean 的 Bean”，用于封装复杂对象的创建逻辑；
    2. **核心方法**：`getObject()`​（生产 Bean）、`getObjectType()`​（返回类型）、`isSingleton()`（是否单例）；
    3. **使用场景**：创建第三方库复杂对象、动态代理对象（如 MyBatis Mapper）、装饰器包装；
    4. **关键区别**：不要和 `BeanFactory` 混淆，前者是生产 Bean 的特殊 Bean，后者是 IoC 容器的核心接口；
    5. **获取自身**：加 `&`​ 前缀，如 `getBean("&beanName")`。

    ## 7. 一句话总结

    ​`FactoryBean`​ 是 Spring 中用于封装复杂对象创建逻辑的特殊工厂 Bean，最经典的应用是 MyBatis 的 `MapperFactoryBean`，它让 Spring 能灵活管理那些难以通过常规方式创建的对象。

    ## 8. 举例：MapperFactoryBean


[^14]: # ObjectFactory

    # Spring 中的 ObjectFactory 详解

    > 此乃函数式接口。
    >
    > **应用：**
    >
    > - 三级缓存，Spring 会把对象封装成 ObjectFactory 放入三级缓存中。
    > - 单例Bean依赖原型Bean：使用  
    >   @Autowired    private ObjectFactory\<PrototypeService\> prototypeServiceFactory;  
    >   不过，建议使用 Provider\<T\>，因为他是java自带的，代码不耦合Spring
    >
    > **特点：**
    >
    > - **函数式接口**，只有一个方法
    > - 解耦，将Bean的获取，与业务逻辑解耦
    > - 延迟加载，只有调用了 getObject() 方法，才会加载
    >

    ​`ObjectFactory`​ 是 Spring 提供的**轻量级函数式工厂接口**，核心作用是**封装 Bean 的获取逻辑，实现 Bean 的延迟 / 按需获取**，是 Spring 内部解决循环依赖、单例依赖原型等核心问题的关键组件，也是工厂模式的极简实现。

    ## 一、核心定义与接口本质

    ​`ObjectFactory`​ 位于 `org.springframework.beans.factory` 包下，是一个极简的函数式接口，Spring 5.x+ 源码如下：

    ```
     @FunctionalInterface
     public interface ObjectFactory<T> {
         // 唯一核心方法：获取目标 Bean 实例
         T getObject() throws BeansException;
     }
    ```
    ### 核心特点

    1. **极致轻量**：仅定义一个方法，无额外依赖，学习和使用成本极低；
    2. **解耦时机**：把「Bean 的获取时机」和「业务逻辑」解耦，无需提前注入 Bean 实例；
    3. **延迟加载**：只有调用 `getObject()` 时，才会从 Spring 容器中获取 / 创建 Bean 实例；
    4. **容器原生集成**：Spring 会自动为注入的 `ObjectFactory` 绑定目标 Bean 的获取逻辑，无需手动实现。

    ## 二、核心使用场景

    ### 1. 核心场景：Spring 循环依赖的三级缓存

    这是 `ObjectFactory` 最核心、最底层的应用，也是你之前学习循环依赖时的核心组件。

    Spring 解决循环依赖的三级缓存 `singletonFactories`​，本质就是一个 `Map<String, ObjectFactory<?>>`。

    - Bean 实例化完成后，Spring 会调用 `addSingletonFactory()`​，把当前 Bean 封装成 `ObjectFactory` 存入三级缓存；
    - 当循环依赖发生时，依赖方会调用这个 `ObjectFactory`​ 的 `getObject()` 方法，获取 Bean 的早期引用（提前生成 AOP 代理也在这个环节），打破循环依赖的死锁。

    #### 源码级对应

    Spring 源码中 `AbstractAutowireCapableBeanFactory` 的核心代码：

    ```
     // 实例化Bean后，将Bean封装为ObjectFactory存入三级缓存
     addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
    ```
    这里的 Lambda 表达式就是一个 `ObjectFactory` 实例，是 Spring 循环依赖解决方案的核心。

    ### 2. 经典场景：单例 Bean 依赖原型 Bean

    你之前学习过：单例 Bean 只会创建一次，如果直接注入原型 Bean，原型 Bean 也会变成单例（仅注入一次）。

    ​`ObjectFactory`​ 是解决这个问题的最优方案之一，**每次调用** **​`getObject()`​** ​ **都会从容器中获取一个全新的原型 Bean 实例**。

    #### 代码示例

    ```
     // 原型Bean：每次获取都创建新实例
     @Service
     @Scope("prototype")
     public class PrototypeService {
         public void doWork() {
             System.out.println("原型Bean实例地址：" + this);
         }
     }
     ​
     // 单例Bean：通过ObjectFactory依赖原型Bean
     @Service
     public class SingletonService {
         // 注入ObjectFactory，而非直接注入原型Bean
         @Autowired
         private ObjectFactory<PrototypeService> prototypeServiceFactory;
     ​
         public void usePrototype() {
             // 每次调用getObject()，都会获取一个全新的原型Bean实例
             PrototypeService service = prototypeServiceFactory.getObject();
             service.doWork();
         }
     }
    ```
    ### 3. 常用场景：Bean 懒加载 / 延迟加载

    对于创建成本高、使用频率低的重资源 Bean，不想在容器启动时就初始化，可以用 `ObjectFactory`​ 实现**按需加载**，只有真正使用时才创建 Bean，减少启动耗时和内存占用。

    #### 代码示例

    ```
     // 重资源Bean，初始化耗时久
     @Service
     public class HeavyResourceService {
         public HeavyResourceService() {
             System.out.println("重资源Bean初始化，耗时3秒...");
             // 模拟初始化耗时
             try { Thread.sleep(3000); } catch (InterruptedException e) {}
         }
     }
     ​
     // 业务Bean，按需使用重资源Bean
     @Service
     public class BusinessService {
         @Autowired
         private ObjectFactory<HeavyResourceService> heavyServiceFactory;
     ​
         public void doBusiness() {
             // 只有真正用到时，才会初始化HeavyResourceService
             HeavyResourceService service = heavyServiceFactory.getObject();
             // 执行业务逻辑
         }
     }
    ```
    ### 4. 设计优化场景：窄化依赖，符合接口隔离原则

    如果我们只需要「获取某个特定类型 Bean」的能力，无需注入整个 `BeanFactory`​/`ApplicationContext`​（容器接口方法繁多，耦合度高），用 `ObjectFactory`​ 可以完美实现**窄化依赖**，只暴露我们需要的 Bean 获取能力，降低耦合，符合接口隔离原则。

    ## 三、易混淆接口区分

    这是面试的高频坑，必须清晰区分 `ObjectFactory`​ 和你之前学习的 `FactoryBean`​、`BeanFactory`：

    |接口|核心定位|核心作用|典型场景|关键特点|
    | ------| -------------------------------| ------------------------| --------------------------------------------| ------------------------------------------------------------------|
    |**ObjectFactory\**|轻量级函数式回调接口|封装 Bean 的获取逻辑，**延迟 / 按需获取 Bean**|循环依赖、单例依赖原型、懒加载|不注册为 Bean，仅作为获取 Bean 的回调，每次调用都从容器取实例|
    |**FactoryBean\**|特殊的工厂 Bean|封装**复杂 Bean 的创建逻辑**，生产特定 Bean|第三方复杂对象、动态代理（MyBatis Mapper）|本身会注册为 Spring Bean，默认返回`getObject()`​生产的对象，加`&`前缀返回自身|
    |**BeanFactory**|Spring IoC 容器的核心底层接口|管理**所有 Bean 的完整生命周期**|Spring 容器的基础，所有 Bean 的管理|是 Spring 容器本身，管理所有 BeanDefinition 和 Bean 实例|

    ### 补充：与 JSR-330 `Provider<T>` 的区别

    两者功能完全一致，都是函数式接口，用于延迟获取 Bean，Spring 原生支持 `Provider`，可完全互换使用：

    - ​`ObjectFactory` 是 Spring 专属接口；
    - ​`Provider` 是 JSR-330 标准接口，跨框架可移植性更好。

    ## 四、设计模式关联

    ​`ObjectFactory`​、`FactoryBean`​、`BeanFactory`​ 都是**工厂模式**的典型 Spring 实现：

    - ​`BeanFactory`：抽象工厂模式，定义了创建所有 Bean 的工厂契约；
    - ​`FactoryBean`：工厂方法模式，为特定类型的 Bean 封装创建逻辑；
    - ​`ObjectFactory`：极简工厂方法模式，仅封装单个 Bean 的获取逻辑，实现延迟加载。

    ## 五、总结

    1. ​`ObjectFactory`​ 是 Spring 轻量级函数式工厂接口，核心能力是**Bean 的延迟 / 按需获取**；
    2. 最核心的底层应用是 **Spring 循环依赖的三级缓存**，用于提前暴露 Bean 的早期引用；
    3. 经典使用场景：单例 Bean 依赖原型 Bean、重资源 Bean 懒加载、窄化容器依赖；
    4. 核心区分：不要和 `FactoryBean`​/`BeanFactory`​ 混淆，`ObjectFactory`​ 是获取 Bean 的回调，`FactoryBean`​ 是生产 Bean 的特殊 Bean，`BeanFactory` 是 Spring 容器本身。


[^15]: # ObjectProvider

    在Spring框架中，`ObjectProvider`​是一个特殊的接口，它提供了一种灵活的方式来获取bean。与直接注入bean相比，使用`ObjectProvider`有几个好处：

    1. **延迟初始化**：`ObjectProvider`允许你在需要时才获取bean，而不是在构造对象时就立即解析依赖。这对于可选依赖特别有用，因为你可以检查bean是否存在，然后再决定是否使用它。
    2. **避免循环依赖**：在某些情况下，直接注入bean可能会导致循环依赖问题。使用`ObjectProvider`​可以在构造对象之后，通过调用`getObject()`​（或`getIfAvailable()`）方法来获取bean，这有助于解决循环依赖的问题。
    3. **处理多个bean**：虽然在这个特定的例子中不相关，但`ObjectProvider`​还可以用于处理多个相同类型的bean。例如，你可以获取一个`ListableBeanFactory`​或`ApplicationContext`，然后从中检索所有特定类型的bean。


[^16]: # MultipartFile

    工作原理

    当用户上传文件时，Spring 会将文件内容存储在内存或者临时文件中，具体取决于文件大小。如果是小文件，可能直接放在内存里；大文件则会写入磁盘的临时目录。临时文件通常会在请求处理完毕后被清理，这就是为什么在异步方法中访问时可能遇到文件已删除的问题。

    如果是基于文件的流（比如 `FileInputStream`​），那么即使流被打开，如果文件被删除，读取时可能会出现问题，尤其是在不同的线程中。但如果是基于内存的流，比如 `ByteArrayInputStream`，那就不存在这个问题，因为数据已经全部加载到内存中。


[^17]: # 拦截链

    Spring 的拦截链是一个非常重要的概念，它体现了 AOP（面向切面编程）和责任链模式在框架中的典型应用。从广义上讲，Spring 中的拦截链可以指两种不同的实现：

    1. **Servlet 规范的 Filter 链**（属于 Web 容器层面）
    2. **Spring MVC 的 HandlerInterceptor 链**（属于 Spring MVC 层面）
    3. **Spring AOP 的拦截器链**（如 `MethodInterceptor`，属于 AOP 层面）

    在面试中，面试官通常更关注 **Spring MVC 的拦截器链**，因为它是日常开发中定制 Controller 请求处理最常用的手段。下面我将以 Spring MVC 的 `HandlerInterceptor` 为例，详细说明其实现原理，并简要提及其他拦截链作为补充。

    ### 一、Spring MVC 拦截器链的实现原理

    #### 1. 核心组件

    - ​**​`HandlerInterceptor`​**​ **接口**：定义了三个方法：

      - ​`preHandle(request, response, handler)`​：在处理器执行前执行，返回 `true`​ 表示继续执行后续拦截器和处理器；返回 `false` 则中断执行流程。
      - ​`postHandle(request, response, handler, modelAndView)`：在处理器执行后、视图渲染前执行。
      - ​`afterCompletion(request, response, handler, exception)`：在视图渲染后、整个请求结束后执行（常用于清理资源）。
    - ​**​`HandlerExecutionChain`​**​：负责维护一个处理器（Handler）和多个 `HandlerInterceptor` 的列表。Spring 通过它来按顺序执行拦截器。
    - ​**​`DispatcherServlet`​**：Spring MVC 的前端控制器，负责请求的分发和拦截链的调用。

    #### 2. 执行流程（结合源码）

    当一个请求进入 `DispatcherServlet`​ 的 `doDispatch` 方法时，主要步骤如下：

    1. **获取执行链**​`DispatcherServlet`​ 调用 `getHandler(processedRequest)`​ 从 `HandlerMapping`​ 中获取 `HandlerExecutionChain`​。这里会把匹配到的处理器（Controller 方法）以及所有配置的拦截器封装到 `HandlerExecutionChain` 中。
    2. **执行拦截器的** **​`preHandle`​**​ **方法**在 `HandlerExecutionChain`​ 的 `applyPreHandle`​ 方法中，会**正序**遍历拦截器列表，依次调用每个拦截器的 `preHandle`。

       - 如果某个拦截器的 `preHandle`​ 返回 `false`​，则会触发 `triggerAfterCompletion`​ 方法，逆向执行已成功执行 `preHandle`​ 的拦截器的 `afterCompletion` 方法，然后直接返回，不再调用处理器。
       - 关键代码（简化自 `HandlerExecutionChain`）：  
         java

         ```
          boolean applyPreHandle(HttpServletRequest request, HttpServletResponse response) throws Exception {
              for (int i = 0; i < this.interceptorList.size(); i++) {
                  HandlerInterceptor interceptor = this.interceptorList.get(i);
                  if (!interceptor.preHandle(request, response, this.handler)) {
                      triggerAfterCompletion(request, response, null);
                      return false;
                  }
                  this.interceptorIndex = i; // 记录成功执行 preHandle 的最后一个拦截器索引
              }
              return true;
          }
         ```
    3. **调用处理器（Controller 方法）** 如果所有 `preHandle`​ 都返回 `true`，则通过反射调用实际的处理器方法（例如 Controller 中的业务方法）。
    4. **执行拦截器的** **​`postHandle`​**​ **方法**处理器执行完毕后，会调用 `HandlerExecutionChain`​ 的 `applyPostHandle`​ 方法，此时是**倒序**遍历拦截器列表，执行 `postHandle`​。倒序的目的是为了与 `preHandle` 形成类似“栈”的行为，使得后配置的拦截器先处理后续操作（例如对 ModelAndView 的修改）。
    5. **视图渲染**​`DispatcherServlet` 渲染视图（如果存在）。
    6. **执行拦截器的** **​`afterCompletion`​**​ **方法**无论处理器执行是否抛出异常（在 finally 块中），最终都会调用 `triggerAfterCompletion`​ 方法，**倒序**执行那些成功通过 `preHandle`​ 的拦截器的 `afterCompletion` 方法，用于资源清理。

    #### 3. 配置方式

    - **Java 配置**：实现 `WebMvcConfigurer`​ 接口，重写 `addInterceptors` 方法。
    - **XML 配置**：使用 `<mvc:interceptors>` 标签。

    ### 二、其他拦截链简介

    #### 1. Servlet Filter 链

    - 基于 Servlet 规范，由 Web 容器（如 Tomcat）管理。
    - 实现 `javax.servlet.Filter`​ 接口，通过 `FilterChain` 传递调用。
    - 在 Spring Boot 中可通过 `@WebFilter`​ 或 `FilterRegistrationBean` 注册。
    - 执行顺序由 `@Order` 或配置文件中的映射顺序决定。

    #### 2. Spring AOP 拦截器链

    - 基于动态代理（JDK 动态代理或 CGLIB）和责任链模式。
    - 核心接口是 `org.aopalliance.intercept.MethodInterceptor`。
    - 通过 `ReflectiveMethodInvocation`​ 维护拦截器链，递归调用 `proceed` 方法。
    - 用于方法级别的增强，如事务、缓存、日志等。

    ### 三、设计模式的应用

    - **责任链模式**：拦截器链的每个节点（Filter/Interceptor）都可以决定是否继续执行链或中断。
    - **适配器模式**：`HandlerExecutionChain`​ 中的拦截器并不直接与 `DispatcherServlet`​ 耦合，而是通过适配（如 `MappedInterceptor`）使其能够适应各种类型的拦截器。
    - **模板方法模式**：`DispatcherServlet`​ 的 `doDispatch` 方法定义了请求处理的整体骨架，而拦截器的方法则在特定阶段插入。

    ### 四、高级点扩展

    - **拦截器与过滤器的区别**：

      - Filter 基于 Servlet 回调，作用于所有请求（包括静态资源），粒度较粗，不能获取 Spring 管理的 Controller 对象。
      - Interceptor 基于 Spring 的 AOP，作用在 Handler 前后，可以访问 Spring 上下文，粒度更细，且能操作 ModelAndView。
    - **异步请求下的拦截器**：Spring 5 引入了 `AsyncHandlerInterceptor`​ 来处理异步请求场景，新增 `afterConcurrentHandlingStarted` 方法，用于在异步处理开始时提前处理响应。
    - **自定义拦截器实践**：通常用于权限校验、日志记录、性能监控等，需要注意 `preHandle`​ 中返回 `false` 时应该设置合适的响应状态或跳转页面。

    ### 总结

    Spring MVC 的拦截器链通过 `HandlerExecutionChain` 维护多个拦截器，在请求处理的不同阶段（preHandle、postHandle、afterCompletion）按特定顺序执行。其实现巧妙结合了责任链模式与适配器模式，使得开发者可以灵活地对 Controller 请求进行横切关注点的增强。理解这一机制不仅有助于日常开发，还能在排查问题时迅速定位到拦截器导致的异常或性能瓶颈。

    以上就是我对 Spring 拦截链实现的理解，希望能解答您的疑问。如果有更具体的方向（例如 AOP 拦截链），我们也可以深入探讨。


[^18]: # 特殊场景 Lookup 方法注入

    **定义：**

    用于解决  **“单例 Bean 依赖原型 Bean”**  的场景：

    - 单例 Bean 只会创建一次，原型 Bean 每次调用 `getBean()` 都会创建新实例；
    - Lookup 方法注入可以让单例 Bean 每次调用方法时，都获取一个新的原型 Bean。

    **代码示例：**

    ```java
     @Service
     public class SingletonService {
         // 1. 定义一个抽象方法，返回原型 Bean
         @Lookup
         public PrototypeService getPrototypeService() {
             // Spring 会在运行时动态生成子类，覆盖这个方法，返回新的原型 Bean
             return null;
         }
     ​
         public void doSomething() {
             // 2. 每次调用 getPrototypeService()，都会获取一个新的原型 Bean
             PrototypeService prototype = getPrototypeService();
             System.out.println("原型 Bean 的地址：" + prototype);
         }
     }
     ​
     @Service
     @Scope("prototype") // 原型 Bean
     public class PrototypeService {
     }
    ```
    #### 适用场景

    - 单例 Bean 需要**每次都获取新的原型 Bean**。


[^19]: # @Validated

    # 1. 常用校验

    日常最常用的是 `@NotNull`​、`@NotBlank`​、`@Size`​、`@Pattern`​，配合 `@Valid`​ 或 `@Validated` 在 Controller 层自动完成参数校验。复杂场景用分组校验或自定义校验注解。

    |场景|推荐注解|触发注解|
    | --------------------------| -------------------| -----------|
    |字段不能为 null|​`@NotNull`|​`@Valid`​ / `@Validated`|
    |字符串不能为空（含空格）|​`@NotBlank`|​`@Valid`​ / `@Validated`|
    |集合不能为空|​`@NotEmpty`|​`@Valid`​ / `@Validated`|
    |数值范围|​`@Min`​ + `@Max`​ 或 `@Range`|​`@Valid`​ / `@Validated`|
    |手机号/身份证格式|​`@Pattern` 或自定义|​`@Valid`​ / `@Validated`|
    |不同场景校验不同字段|分组 + `@Validated`|​`@Validated(Group.class)`|
    |对象内部嵌套校验|​`@Valid` 标记内层对象|​`@Valid`|

    ‍

    ```java
    // 4. 使用所有常用校验注解的 DTO 类
    public class UserRegisterDto {

        @NotNull(message = "用户ID不能为空")
        private Long id;

        @NotBlank(message = "用户名不能为空且不能全是空格")
        private String username;

        @NotEmpty(message = "至少需要一个角色")
        private List<String> roles;

        @Min(value = 1, message = "年龄最小1岁")
        @Max(value = 120, message = "年龄最大120岁")
        private Integer age;

        @Range(min = 0, max = 100, message = "分数必须在0-100之间")
        private Integer score;

        @Pattern(regexp = "^1[3-9]\\d{9}$", message = "手机号格式不正确")
        private String phone;

        @ValidEnum(enumClass = UserRole.class, message = "角色必须是 ADMIN/USER/GUEST")
        private String role;

        // 为了简洁，省略 getter/setter，实际使用时需要生成
    }
    ```
    # 2. 应用场景

    ## 2.1 场景 1：普通校验（无分组）

    两者都可以，无本质差异：

    ```java
    public class UserDto {
        @NotBlank 
    	private String name;
        @Min(1) @Max(120) private Integer age;
    }

    // 请求体校验（@Valid / @Validated）
    @PostMapping("/user")
    public Result add(@Valid @RequestBody UserDTO dto) {}      // ✅

    @PostMapping("/user")
    public Result add(@Validated @RequestBody UserDTO dto) {}  // ✅
    ```
    ## 2.2 场景 2：需要分组（Group）

    只能用 `@Validated`：

    ```java
    // 定义分组接口
    public interface CreateGroup {}
    public interface UpdateGroup {}

    // 使用分组
    public class UserDto {
        @NotNull(groups = {CreateGroup.class, UpdateGroup.class})
        private Long id;
        
        @NotBlank(groups = CreateGroup.class)
        private String password;
        
        @NotBlank(groups = UpdateGroup.class)
        private String name;
    }
    ```
    校验：

    ```java
    // Controller 中指定分组
    @PostMapping
    public void create(@Validated(CreateGroup.class) @RequestBody UserDto dto) {}

    @PostMapping("/user")
    public Result add(@Validated(UpdateGroup.class) @RequestBody UserDTO dto) {}
    ```
    ## 2.3 场景 3：嵌套对象校验

    两个注解需要配合使用。即使外层用 `@Validated`​，内层也必须用 `@Valid` 才能触发递归校验：

    ```java
    public class OrderDto {
        @NotNull
        private Long orderId;

        @Valid  // ← 必须加,触发嵌套校验
        private AddressDto address;
    }

    public class AddressDto {
        @NotBlank
        private String city;
        @NotBlank
        private String street;
    }

    // 外层可以是 @Valid 或 @Validated
    @PostMapping("/order")
    public Result create(@Validated @RequestBody OrderDTO dto) {}   
    ```
    ## 2.4 场景 4：类上校验（校验类中方法的参数）

    > 当 `@Validated`​ 注解写在**类**上时，它的核心作用是**开启 Spring 的方法级别参数校验**（即 Spring 的 `MethodValidationPostProcessor` 会对该类的方法进行 AOP 代理，检查方法参数上的 Bean Validation 注解）。
    >

    只能用 `@Validated` 开启：

    ```java
    @Service
    @Validated   // ← 必须加在类上
    public class UserServiceImpl { 
        public void update(@NotNull @Min(1) Long id, @NotBlank String name) {
            // 方法参数校验会生效
        }
    }

    // GET 请求参数校验（@RequestParam + @Validated）
    @RestController
    @Validated   // 必须加在类上
    public class QueryController {
        @GetMapping("/list")
        public Result list(@RequestParam @Min(1) Integer page,
                           @RequestParam @Max(100) Integer size) {
            // page>=1, size<=100
            return Result.ok();
        }
    }

    // 路径变量校验（@PathVariable）
    @RestController
    @Validated
    public class UserController {
        @GetMapping("/user/{id}")
        public Result getUser(@PathVariable @Min(1) Long id) {
            // id >= 1
            return Result.ok();
        }
    }

    // 注意：类上的 @Validated 不能替代参数前的 @Valid / @Validated。
    @RestController
    @Validated
    public class UserController {
        @PostMapping("/user")
        public Result create(@Valid @RequestBody UserDto dto) {  // 仍需 @Valid
            // 必须加 @Valid，否则 UserDto 内部的 @NotNull 等不会校验
            return Result.ok();
        }
    }

    // 类上的 @Validated 可以指定分组，那么该类中所有方法参数校验都将使用该分组（除非方法上覆盖）
    @RestController
    @Validated(CreateGroup.class)
    public class UserController {

        @PostMapping("/user")
        public Result create(@Validated(CreateGroup.class) @RequestBody UserDto dto) {
            // dto 使用 CreateGroup 分组校验
        }
    }
    ```
    ## 2.5 场景5：自定义校验注解

    ​**场景**：校验枚举值、手机号、身份证等。

    1. 定义注解：

    ```java
    @Target({FIELD})
    @Retention(RUNTIME)
    @Constraint(validatedBy = EnumValidator.class)
    public @interface ValidEnum {
        String message() default "枚举值无效";
        Class<?>[] groups() default {};
        Class<? extends Enum<?>> enumClass();
    }
    ```
    2. 实现校验器：

    ```java
    public class EnumValidator implements ConstraintValidator<ValidEnum, String> {
        private Set<String> allowed;
        @Override
        public void initialize(ValidEnum constraint) {
            allowed = Arrays.stream(constraint.enumClass().getEnumConstants())
                    .map(Enum::name).collect(Collectors.toSet());
        }
        @Override
        public boolean isValid(String value, ConstraintValidatorContext ctx) {
            return value == null || allowed.contains(value);
        }
    }
    ```
    3. 使用：

    ```java
    public class OrderDto {
        @ValidEnum(enumClass = StatusEnum.class)
        private String status;
    }
    ```
    # 3. 失效场景（常见坑）

    1. 嵌套对象 **只加**   **​`@Validated`​**​ **，内层不加**   **​`@Valid`​**：嵌套对象不会被递归校验。
    2. ​**类上没加**   **​`@Validated`​**​ **，但方法参数写了**   **​`@Valid`​**：Spring 普通方法（非 Controller）不会自动触发校验。
    3. ​**分组校验时，字段没指定**  **​`groups`​**：该字段在任何分组下都会校验，可能导致不该校验的字段也被检查。
    4. **用**   **​`@Valid`​**​  **但字段约束没有**   **​`@NotNull`​**​  **等注解**：当然没效果——`@Valid` 只负责“开启校验”，不定义规则。
    5. **如果类上标注了**   **​`@Validated`​**​ **，方法参数不加**   **​`@Valid`​**​  **会生效吗？**   
       不会。类级别的 `@Validated`​ 用于开启方法参数上的约束校验（如 `@RequestParam`​、`@PathVariable`​），但对 `@RequestBody`​ 对象内部字段的校验**仍然需要**在参数前写 `@Valid`。
    6. 类上的 `@Validated`​ **只对简单类型参数（**​ **​`@RequestParam`​**​ **、**​ **​`@PathVariable`​**​ **、** 普通方法参数[^20] **）上的约束注解（如**   **​`@Min`​**​ **、**​ **​`@NotBlank`​**​ **）生效**，对于自定义对象内部的字段校验，它无能为力。
    7. **能全局强制对所有**   **​`@RequestBody`​**​  **参数校验吗？**   
       不能。Spring 默认不开启，必须显式添加 `@Valid`​ 或 `@Validated`。可以通过自定义参数解析器实现类似效果，但一般不推荐。

    ‍

    # 4. 完整示例

    ```java
    import jakarta.validation.Constraint;
    import jakarta.validation.ConstraintValidator;
    import jakarta.validation.ConstraintValidatorContext;
    import jakarta.validation.Payload;
    import jakarta.validation.constraints.*;
    import org.hibernate.validator.constraints.Range;

    import java.lang.annotation.*;
    import java.util.Arrays;
    import java.util.List;
    import java.util.Set;
    import java.util.stream.Collectors;

    // 1. 枚举示例
    enum UserRole {
        ADMIN, USER, GUEST
    }

    // 2. 自定义注解 @ValidEnum
    @Target({ElementType.FIELD})
    @Retention(RetentionPolicy.RUNTIME)
    @Constraint(validatedBy = EnumValidator.class)
    public @interface ValidEnum {
        String message() default "枚举值无效";
        Class<?>[] groups() default {};
        Class<? extends Payload>[] payload() default {};
        Class<? extends Enum<?>> enumClass();
    }

    // 3. 自定义校验器实现
    class EnumValidator implements ConstraintValidator<ValidEnum, String> {
        private Set<String> allowedValues;

        @Override
        public void initialize(ValidEnum constraintAnnotation) {
            allowedValues = Arrays.stream(constraintAnnotation.enumClass().getEnumConstants())
                    .map(Enum::name)
                    .collect(Collectors.toSet());
        }

        @Override
        public boolean isValid(String value, ConstraintValidatorContext context) {
            return value == null || allowedValues.contains(value);
        }
    }

    // 4. 使用所有常用校验注解的 DTO 类
    public class UserRegisterDto {

        @NotNull(message = "用户ID不能为空")
        private Long id;

        @NotBlank(message = "用户名不能为空且不能全是空格")
        private String username;

        @NotEmpty(message = "至少需要一个角色")
        private List<String> roles;

        @Min(value = 1, message = "年龄最小1岁")
        @Max(value = 120, message = "年龄最大120岁")
        private Integer age;

        @Range(min = 0, max = 100, message = "分数必须在0-100之间")
        private Integer score;

        @Pattern(regexp = "^1[3-9]\\d{9}$", message = "手机号格式不正确")
        private String phone;

        @ValidEnum(enumClass = UserRole.class, message = "角色必须是 ADMIN/USER/GUEST")
        private String role;

        // 为了简洁，省略 getter/setter，实际使用时需要生成
    }
    ```

[^20]: # 普通方法参数

    在 Spring MVC 中，**普通方法参数**指的是**没有标注任何 Spring 特定注解**（如 `@RequestParam`​、`@PathVariable`​、`@RequestBody`​、`@ModelAttribute`）的参数。例如：

    ```java
    @GetMapping("/hello")
    public String hello(String name, Integer age) {   // name 和 age 就是普通方法参数
        // Spring 会自动从请求参数（query string 或表单字段）中按名称绑定值
    }
    ```
    - 当请求为 `/hello?name=John&age=25`​ 时，`name="John"`​，`age=25`。
    - 这类参数支持类型转换（如字符串转数字），但​**不能直接绑定复杂对象**（除非自定义）。

    **类上加了**   **​`@Validated`​**​  **后**，可以在这些普通参数上直接使用 `@Min`​、`@NotBlank` 等校验注解：

    ```java
    @RestController
    @Validated
    public class TestController {

        @GetMapping("/test")
        public String test(@NotBlank String name, @Min(1) Integer age) {
            // 校验会自动生效，不通过抛 ConstraintViolationException
            return "ok";
        }
    }
    ```
