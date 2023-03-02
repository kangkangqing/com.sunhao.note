# Mybatis-9.28

环境：

- JDK1.8

- Mysql 5.7

- maven 3.6.1

- IDEa

回顾：

- JDBC

- Mysql

- java基础

- Maven

- Junit

SSM框架：配置文件的

# 1.简介

### 1.1什么是Mybatis

- MyBatis 是一款优秀的**持久层框架**

- 它支持自定义 SQL、存储过程以及高级映射。

- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。

- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

  **如何获得mybatis**

- maven仓库

  ```java
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
  </dependency>
  ```

- github



### 1.2持久化

数据持久化

- 持久化就是将程序的数据在持久状态和瞬时状态转化的过程

- 内存：断电即失

- 数据库(jdbc),IO文件持久化
- 生活：冷藏，罐头

**为什么需要持久化**

有一些东西，不能让他丢掉

内存太贵了

### 1.3持久层

Dao层，Service层，Controller层

- 完成持久化工作的代码块

- 层界限十分明显

### 1.4为什么需要Mybatis

- 帮助程序员将数据存入到数据库中

- 方便

- JDBC太复杂了。简化

# 2.第一个Mybatis

思路：搭建环境，导入Mybatis，编写代码，测试

### 2.1搭建环境

搭建数据库

新建项目

导入依赖

### 2.2完成核心配置文件

### 2.3编写测试文件代码

完成增删改查

# 3.MyBatis获取参数值的两种方式

Mybatis获取参数值的两种方式：${}和#{}

**${}的本质就是字符串拼接**，**#{}的本质就是占位符赋值**

${}使用字符串拼接的方式拼接sql，若字符串类型或日期类型的字段进行赋值，**需要手动加单引号**；但是#{}使用占位符赋值的方式拼接sql，此时为字符串类型或日期类型的字段进行赋值时，**可以自动添加单引号**

## 3.1单个字面量类型的参数

若mapper接口中的方法参数为单个的字面量类型

此时可以使用${}和#{}以任意的名称获取参数的值，注意**${}需要手动加单引号**

## 3.2多个字面量类型的参数

若mapper接口中的方法参数为多个时

此时MyBatis会自动将这些参数放在一个map集合中，以**两种方式存储数据**

a.以arg0，arg1...为键，以参数为值

b.以param1，param2...为键，以参数为值

因此，只需要通过#{}和${}访问map集合的键，就可以获取相对应的值

## 3.3map集合类型的参数

只需要通过#{}和${}访问map集合的键，就可以获取对应的值，一定要注意${}的单引号问题

## 3.4若mapper接口方法的参数为实体类类型的参数（开发常用）

只需要通过#{}和${}访问实体类的属性名，就可以获取相对应的属性值 ，一般情况下#{}使用得多

##   3.5可以在mapper接口方法的的参数上设置@Param注解（开发常用）

​		此时**Mybatis会这些参数放在map中**，**以两种方式进行存储**

​	**a**.以@Param注解的value属性值为键，以参数为值

​	**b**.以param1，param2...为键，以参数为值

只需要通过#{}和${}访问map集合的键，就可以获取相对应的值，一定要注意${}的单引号问题

# 4.Mybatis的各种查询功能

## 4.1查询一个实体类对象

## 4.2查询一个list集合

若**sql语句查询的结果为多条**时，**一定不能以实体类类型作为方法的返回值**

否则会抛出异常**TooManyResultException**

若sql语句查询的结果为1条时，此时可以使用**实体类类型**或**list集合类型**作为方法的返回值

=================================

Mybatis中为java中常用的类型设置了类型别名

Integer：Integer，int

int：下划线int，下划线integer

Map：map

String：string

## 4.3查询单个数据

## 4.4查询一条数据为map集合

## 4.5查询所有的用户信息为map集合

若查询的数据有多条时，并且要将每条数据转换为map集合

此时有两种**解决方案**：

1.将mapper接口方法的返回值**设置为泛型是map的list集合**

2.可以将每条数据转换的map集合放在一个大的map中，但是必须要通过**@MapKey注解**将查询的某个字段的值作为大的map的键

# 5.特殊SQL的执行

## 5.1模糊查询

模糊查询sql语句

**SELECT \* from table where username like '%陈哈哈%'** 

模糊查询的sql语句一共有三种写法

```xml
<!--        第一种方式-->
<!--        select*from t_user where username like '%${mohu}%'-->
<!--        使用#{}大括号行不通-->
<!--        #{}和${}有本质的区别-->


<!--        第二种方式：字符串拼接-->
<!--        concat函数-->
<!--        select*from t_user where username like concat('%',#{mohu},'%')-->


<!--        第三种方式，开发常用方式-->
        select*from t_user where username like "%"#{mohu}"%"
```

## 5.2批量删除

​	1.要注意使用${}和#{}在解析到sql中时，会变成双引号还是的单引号   #{}会自动加单引号

​	2.因为sql语句中有的地方只能使用单引号或者双引号

```xml
<!--delete from  t_user where id in (5,6)-->
```

这里删除语句的传入参数是不需要加单引号的

## 5.3动态设置表名

通过传入指定参数，获取要查询的表信息

表名是不能加单引号的，否则sql语句是不符合规范的

```xml
select*from ${tableName}
```

​	1.要注意使用${}和#{}在解析到sql中时，会变成双引号还是的单引号

​	2.因为sql语句中有的地方只能使用单引号或者双引号

## 5.4添加功能获取自增的主键

```xml
<!--    void insertUser(User user);-->
    <insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
        insert into t_user values(#{id},#{username},#{password},#{age},#{gender},#{email})
<!--        useGeneratedKeys="true" 是否开启自增主键-->
<!--        keyProperty="id" 将添加的数据的自增主键为实体类类型的参数的属性赋值-->
    </insert>
```

```java
SqlSession sqlSession= SqlSessionUtil.getSqlSession();
specialSQLMapper mapper = sqlSession.getMapper(specialSQLMapper.class);
User user=new User(10,"石头","123456",45,"女","23903@qq.com");
mapper.insertUser(user);
//适用情况: 主键为自增，添加一条记录，会给id赋值为空，让它自己自增，此时你是没办法直接读取到创建的对象的id的
// 将添加的自增主键为实体类对象的属性赋值
// <!--        useGeneratedKeys="true" 是否开启自增主键-->
// <!--        keyProperty="id" 将添加的数据的自增主键为实体类类型的参数的属性赋值-->
//有了这两个属性就能查到主键，添加user，再输出user，将能输出自增后的主键的信息
```

# 6.自定义映射resultMap	

## 6.1 resultMap处理字段和属性的映射关系

```xml
<!-- 一共三种方式-->

<!-- 方式一-->
<!--            select*from t_emp-->
<!--            解决属性名与字段名不匹配造成属性名为null的情况-->
<!--            解决方式一-->
<!--            1.为查询的字段设置别名，和属性名保持一致-->
  				select emp_id empId,emp_name empName,age,gender from t_Emp


<!-- 方式二-->
<!--            2.当字段符合Mysql的使用要求_,而属性符合java的要求使用驼峰
                  此时可以在MyBatis的核心配置文件中设置一个全局配置，可以自动将下划线映射为驼峰
				  在核心配置文件中，设置settings标签
				  规则：emp_id=>empId		
-->
核心配置文件
    <settings>
<!--        标签作用：将下划线映射为驼峰-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
          

<!-- 方式三-->
    <resultMap id="empResultMap" type="Emp">
<!--        主键映射关系-->
        <id column="emp_id" property="empId"></id>
<!--        普通属性映射关系-->
        <result property="empName" column="emp_name"></result>
    </resultMap>
<!--    3.使用resultMap自定义映射处理-->
    <select id="getAllEmp" resultMap="empResultMap">
            select*from t_emp
    </select>
			
resultMap：设置自定义的映射关系
id:唯一标识
type：处理映射关系的实体类类欸行
常用的标签：
id：处理主键和实体类中实现的映射关系
result：处理主键和实体类中的映射关系
association:处理多对一的映射关系
colomn：设置映射关系中的字段名，必须是sql查询出的某个字段
property：设置映射关系中的属性的属性名，必须是处理的实体类类型中的属性名
```

## 6.2 多对一映射处理 

sql字段有多个，但是属性名只有一个

处理多对一的映射关系一共有三种方式：

​		级联方式

​		assciation处理多对一的映射关系（处理实体类类型的属性）	

​		分步查询

### 6.2.1级联方式处理映射关系

```
   <!--    1.方式一：级联方式-->
    <resultMap id="empAndDeptResultMap" type="Emp">
        <id column="emp_id" property="empId"></id>
        <result column="emp_name" property="empName"></result>
        <result column="age" property="age"></result>
        <result column="gender" property="gender"></result>
        <result column="dept_id" property="dept.deptId"></result>
        <result column="dept_name" property="dept.deptName"></result>
    </resultMap>
<!--    Emp getEmpById(@Param("empId")Integer empId);-->
    <select id="getEmpById" resultType="emp" resultMap="empAndDeptResultMap">
        select t_emp.*,t_dept.*
        from t_emp left join t_dept
        on t_emp.dept_id=t_dept.dept_id
        where t_emp.emp_id=#{empId}
    </select>
```

### 6.2.2使用association处理映射关系

```java
<resultMap id="empAndDeptResultMap" type="Emp">
        <id column="emp_id" property="empId"></id>
        <result column="emp_name" property="empName"></result>
        <result column="age" property="age"></result>
        <result column="gender" property="gender"></result>
<!--        <result column="dept_id" property="dept.deptId"></result>-->
<!--        <result column="dept_name" property="dept.deptName"></result>-->

        <!--        2.方式二:association处理多对一的映射关系-->
        <association property="dept" javaType="dept">
            <id column="dept_id" property="deptId"></id>
            <result column="dept_name" property="deptName"></result>
<!--            association:处理多对一的映射关系(处理实体类类型的属性)-->
<!--            property:设置需要处理映射关系的属性的属性名-->
<!--            javaType：设置要处理的属性的类型-->
        </association>
    </resultMap>
```

### 6.2.3分步查询

```java
<!--    Emp getEmpByStep(@Param("empId")Integer empId);-->
<!--    处理映射关系的第三种方式：-->
<!--    getEmpByStepOne-->
<!--    通过分步查询查询员工以及所对应的部门信息的第一步-->

    <resultMap id="getEmpByStepResultMap" type="emp">
        <id column="emp_id" property="empId"></id>
        <result column="emp_name" property="empName"></result>
        <association property="dept"
                     select="com.sunhao.mybatis.mapper.DeptMapper.getEmpAndDeptBystepTwo"
                     column="dept_id">
<!--            property:设置需要处理映射关系的属性的属性名-->
<!--            select:设置分布查询的sql的唯一标识-->
<!--            column：将查询出的某个字段作为分步查询的sql的条件-->
        </association>
    </resultMap>
    <select id="getEmpByStep" resultType="emp" resultMap="getEmpByStepResultMap">
        select*from t_emp where emp_id=#{empId}
    </select>
```

```java
<!--    Dept getEmpAndDeptBystepTwo(@Param("deptId") Integer deptId);-->
    <select id="getEmpAndDeptBystepTwo" resultType="Dept">
        select*from t_dept where dept_id=#{deptId}
    </select>
```

分步骤查询信息，查询部门信息用一个新的select语句写

然后再查询员工信息的语句中，通过association的属性传入查询的部门信息



分步查询其实是有**优势**的：可以实现**延迟加载**

但是必须在核心配置文件中设置全局配置信息:

lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载

aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。否则，每个属性会按需加载

此时就可以实现

```java
    <resultMap id="getEmpByStepResultMap" type="emp">
        <id column="emp_id" property="empId"></id>
        <result column="emp_name" property="empName"></result>
        <association property="dept"
                     fetchType="eager"
                     select="com.sunhao.mybatis.mapper.DeptMapper.getEmpAndDeptBystepTwo"
                     column="dept_id">
<!--            property:设置需要处理映射关系的属性的属性名-->
<!--            select:设置分布查询的sql的唯一标识-->
<!--            column：将查询出的某个字段作为分步查询的sql的条件-->
<!--            fetchType:在开启了延迟加载的环境中，通过该属性设置当前的分步查询是否使用延迟加载
                eager：立即加载||lazy延迟加载
-->
        </association>
    </resultMap>
```

## 6.3一对多映射处理

一个部门对应多个员工信息

一共两种方式：

1.Collection

2.分布查询

### 6.3.1 Collection

### 6.3.2 分步查询

​			1.查询部门信息

​			2.根据部门id查询部门中的所有员工

# 7.动态sql

Mybatis的动态sql技术时一种根据特定条件动态拼装sql语句的功能，它存在的意义是为了**解决拼接sql语句字符串**时的痛点问题

## 7.1 if

if标签可通过test属性的表达式进行判断，若表达式为true，则标签中的内容会执行；反之标签中的内容不会执行

```java
<select id="getEmpByCondition" resultType="emp">
    select*from t_emp where 1=1
    <if test="empName !=null and empName!=''">
        emp_name=#{empName}
    </if>
    <if test="age !=0 ">
        and age=#{age}
    </if>
    <if test="gender !=null and gender!=''">
        and gender=#{gender}
    </if>
</select>
```

当if不成立时，句子不执行，where后面没有语句，需要写1=1占位，后面的句子成立才能接着运行

### 7.2 where

## 7.3 trim

## 7.4 choose,when,otherwise

## 7.5 foreach

## 7.6 SQL片段