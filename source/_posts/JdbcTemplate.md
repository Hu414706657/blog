---
title: JdbcTemplate
date: 2019-08-13 10:34:08
tags:
    - java
---



# JdbcTemplate简介

Spring提供的一个操作数据库的技术JdbcTemplate，是对Jdbc的封装。语法风格非常接近DBUtils。 
JdbcTemplate可以直接操作数据库，加快效率，而且学这个JdbcTemplate也是为声明式事务做准备，毕竟要对数据库中的数据进行操纵！ 
JdbcTemplate中并没有提供一级缓存，以及类与类之间的关联关系！就像是spring提供的一个DBUtils。 
Spring对数据库的操作使用JdbcTemplate来封装JDBC，结合Spring的注入特性可以很方便的实现对数据库的访问操作。使用JdbcTemplate可以像JDBC一样来编写数据库的操作代码


# jdbcTemplate更新数据库常用方法

- update (更新数据)
- batchUpdate (批量更新数据库)
- queryForObject (查询单行)
- query （查询多行）
- queryForObject (单值查询)不同的

# 更新操作

```java
            String sql = "UPDATE student SET age = ? WHERE sid = ?";
            template.update(sql, 13,21020);//第一个是sql语句，后面的按着顺序传入参数即可，这个update方法是接收的可变参数！
```

# 批量插入

```java
        String sql="INSERT INTO employee(`emp_name`,`salary`) VALUES(?,?)";
        //执行sql语句需要传递的参数
        //      Object[][] params = new Object[3][2];
        //      params[0] = new Object[]{"Tom2015",1000};
        //      params[1] = new Object[]{"Tom2016",2000};
        //      params[2] = new Object[]{"Tom2017",3000};
        List<Object[]> list = new ArrayList<Object[]>();
        list.add(new Object[]{"Tom2015",1000});
        list.add(new Object[]{"Tom2016",2000});
        list.add(new Object[]{"Tom2017",3000});
        template.batchUpdate(sql, list);
```

# 单个查询

```java
String sql = "SELECT emp_id AS empId,emp_name AS empName,salary FROM employee WHERE emp_id=?";
//RowMapper是一个接口,这里我们使用其子类
RowMapper<Employee> rowMapper = new BeanPropertyRowMapper<Employee>(Employee.class);
//最后一个参数是可变参数，用于向sql语句中依次传递参数!
Employee employee = template.queryForObject(sql, rowMapper, 5);
```

# 多数查询

```java
//查询salary>4000的数据库记录，封装为List集合返回

//需要注意的是：sql语句中的别名要与对应实体类的属性名保持一致！
String sql = "SELECT emp_id AS empId,emp_name AS empName,salary FROM employee WHERE salary > ?";
//RowMapper是一个接口,这里我们使用其子类
RowMapper<Employee> rowMapper = new BeanPropertyRowMapper<Employee>(Employee.class);
//该query方法查询出来的是一个list列表,query方法的最后一个参数是可变参数！
List<Employee> list = template.query(sql, rowMapper, 4000);
```


```java
sb.append("select b.STATIS_DATE , b.STORE_ID , nvl(DAY_TOTAL_MONEY, 0) DAY_TOTAL_MONEY"
    + ", nvl(MONTH_TOTAL_MONEY, 0) MONTH_TOTAL_MONEY, nvl(ACC_TOTAL_MONEY, 0) ACC_TOTAL_MONEY"
    + ", nvl(CMP_ORDERS_AMNT, 0) CMP_ORDERS_AMNT, nvl(OSG_ORDERS_AMNT, 0) OSG_ORDERS_AMNT"
    + ", nvl(ORDER_AMNT, 0) ORDER_AMNT, nvl(NEW_USERS, 0) NEW_USERS, nvl(OLD_USERS, 0) OLD_USERS"
    + ", nvl(TOTAL_USERS, 0) TOTAL_USERS from tb_store_report_d@dblink_177_161 b where b.store_id = ?"
    + " and b.statis_date between ? and ? order by b.statis_date asc");
}
List<DayData> query = jdbcTemplate.query(sb.toString(), new Object[]{storeId, startDate, endDate}, new DayData());
```

# 函数使用

```java
//查询最大salary
String sql = "SELECT MAX(salary) FROM employee";
//需要指定返回值的类型,而且类型必须是包装类型
Double maxSalary = template.queryForObject(sql, Double.class);

//count()函数
String sql = "SELECT count(*) FROM SRT_SPU ";
int count= jdbcTemplate.queryForObject(sql, Integer.class);
```




