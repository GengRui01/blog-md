---
title: "JDBC连接MySQL数据库"
tags: ["JDBC", "MySQL", "Java"]
date: 2018-07-25
---

JDBC连接MySQL数据库

<!--more-->

## 连接数据库步骤

第一步：注册驱动

第二步：获取连接

第三步：获取statement对象

第四步：执行SQL语句返回结果集

第五步：遍历结果集

第六步：关闭连接释放资源

## 传统方式连接数据库

```java
public class JDBCDemo {
	/**
	 * @param args
	 * @throws Exception
	 */
	public static void main(String[] args) throws Exception {
		//1.注册驱动
		DriverManager.registerDriver(new com.mysql.jdbc.Driver());
		//2.获取连接
		Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/ssm", "root", "root");
		//3.获取Statement对象
		PreparedStatement preparedStatement = connection.prepareStatement("select * from tb_user");
		//4.执行SQL语句返回结果集
		ResultSet resultSet = preparedStatement.executeQuery();
		//5.遍历结果集
		while (resultSet.next()) {
			System.out.println(resultSet.getString("username"));
		}
		//6.释放资源
		resultSet.close();
		preparedStatement.close();
		connection.close();
	}
}
```

用这种方式连接数据库存在一个重要的问题：

注册驱动时，当前类和MySQL的驱动类有很强的依赖关系。

当我们没有驱动类的时候，连编译都不能通过。

这种调用者与被调用者之间的依赖关系，就叫做程序的耦合，耦合分为高耦合（紧密联系）和低耦合（松散联系）

我们在开发中，理想的状态应该是编译时不依赖，运行时才依赖。

要做到编译时不依赖，就需要使用反射来创建类对象。

即将注册驱动部分的代码稍作修改如下：

## 编译时不依赖的数据库连接

```java
public class JDBCDemo {
	/**
	 * @param args
	 * @throws Exception
	 */
	public static void main(String[] args) throws Exception {
		//1.注册驱动
		//DriverManager.registerDriver(new com.mysql.jdbc.Driver());
		Class.forName("com.mysql.jdbc.Driver");
		//2.获取连接
		Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/ssm", "root", "root");
		//3.获取Statement对象
		PreparedStatement preparedStatement = connection.prepareStatement("select * from tb_user");
		//4.执行SQL语句返回结果集
		ResultSet resultSet = preparedStatement.executeQuery();
		//5.遍历结果集
		while (resultSet.next()) {
			System.out.println(resultSet.getString("username"));
		}
		//6.释放资源
		resultSet.close();
		preparedStatement.close();
		connection.close();
	}
}
```

这样做的好处是，我们的类中不再依赖具体的驱动类，此时就算删除mysql的驱动jar包依然可以通过编译，只不过因为没有驱动类所以不能运行罢了。

不过，此处还有一个问题，就是我们反射类对象的全限定类名称是在java类中写死的，数据库的端口号、用户名密码也是写死的，一旦要修改就等于是要修改源码。
自己小打小闹写的代码改源码什么的还好说，但如果是上线项目，改源码势必要停服务器重新编运行。

这么看来这个问题造成的后果很严重，其实它的解决方法也很简单，使用配置文件配置数据库连接信息就可以啦。

## 使用配置文件连接数据库

### 配置文件

```ini
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm
jdbc.username=root
jdbc.password=root
```

### 代码实现

```java
public class JDBCDemoPro {
	/**
	 * @param args
	 * @throws Exception
	 */
	public static void main(String[] args) throws Exception {
		//读取配置文件db.properties
		Properties prop = new Properties();
		prop.load(new FileInputStream("db.properties"));
		 
		//获取配置文件中的相关参数值
		String driver = prop.getProperty("jdbc.driver");
		String url = prop.getProperty("jdbc.url");
		String user = prop.getProperty("jdbc.username");
		String password = prop.getProperty("jdbc.password");
		
		//1.注册驱动
		Class.forName(driver);
		//2.获取连接
		Connection connection = DriverManager.getConnection(url, user, password);
		//3.获取Statement对象
		PreparedStatement preparedStatement = connection.prepareStatement("select * from tb_user");
		//4.执行SQL语句返回结果集
		ResultSet resultSet = preparedStatement.executeQuery();
		//5.遍历结果集
		while (resultSet.next()) {
			System.out.println(resultSet.getString("username"));
		}
		//6.释放资源
		resultSet.close();
		preparedStatement.close();
		connection.close();
	}
}
```