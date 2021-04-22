---
title: 原生JDBC
date: 2019-08-09 09:45:17
tags:
    - 数据库
---

# 概述

- JDBC(Java Data Base Connectivity,java数据库连接)是SUN公司开发的,一种用于执行SQL语句的Java API
- JDBC是Java访问数据库的标准规范，可以为不同的关系型数据库提供统一访问，它由一组用Java语言编写的接口和类组成
- JDBC需要连接驱动，驱动是两个设备要进行通信，满足一定通信数据格式，数据格式由设备提供商规定，设备提供商为设备提供驱动软件，通过软件可以与该设备进行通信

# JDBC访问数据库的基本步骤

- 加载JDBC驱动程序

- 建立数据库连接

- 创建操作数据库SQL的对象

- 执行语句并分析执行结果

- 关闭连接

# 实例

```java
        String driverClassName = "com.mysql.jdbc.Driver";
        String url = "jdbc:oracle:thin:@127.0.0.0:1521:ORCL";
        String userName = "root";
        String passWord = "123456";
        try {
            // 加载驱动
            Class.forName(driverClassName);
            //获取连接
            Connection con = DriverManager.getConnection(url, userName, passWord);
            String sql = "select * from SRT_PRINTER b ";
            //执行sql
            PreparedStatement statement = con.prepareStatement(sql);
            //返回结果集
            ResultSet resultSet = statement.executeQuery();
            //遍历结果集
            while (resultSet.next()) {
                String id = resultSet.getString("id");
                System.out.println(id);
            }

            statement.close();
            resultSet.close();
            con.close();
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
```

