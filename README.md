# jdbc-mysql-demo

标签： java jdbc mysql demo

---
jar包 mysql connector/j 版本5.1.40

测试表
```sql
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `username` varchar(255) NOT NULL COMMENT '用户名',
  `password` varchar(255) DEFAULT NULL COMMENT '密码',
  `createdAt` datetime DEFAULT NULL COMMENT '创建时间',
  `updatedAt` datetime DEFAULT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  KEY `username` (`username`(191))
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='用户表'
```
代码
```java

import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import org.junit.Test;

public class Demo {
	@Test
	public void queryDemo() {
		// 数据库连接接对象
		Connection conn = null;
		// SQL预处理对象
		PreparedStatement stmt = null;
		// 结果集
		ResultSet res = null;

		try {
			// 注册mysql数据库jdbc连接驱动
			Class.forName("com.mysql.jdbc.Driver");
			// 连接url 格式 jdbc:数据库类型://服务器地址:端口/数据库名
			String url = "jdbc:mysql://localhost:3306/testdb";
			// 用户名
			String user = "liuwel";
			// 密码
			String password = "passwd";
			// 获取连接对象
			conn = DriverManager.getConnection(url, user, password);
			// SQL
			String sql = " select * from user where id = ? ";
			// 获取预处理对象
			stmt = conn.prepareStatement(sql);
			// 绑定参数 int型 可以用String绑定
			stmt.setString(1, "1");
			// 执行sql
			res = stmt.executeQuery();
			// 输出数据
			while (res.next()) {
				System.out.print(res.getInt("id") + "\t"
						+ res.getString("username") + "\t"
						+ res.getString("password"));
			}

		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
		    // 关闭资源 
			if (res != null) {
				try {
					res.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
			if (stmt != null) {
				try {
					stmt.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
			if (conn != null) {
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
		}
	}

	@Test
	public void executeDemo() {
		// 数据库连接接对象
		Connection conn = null;
		// SQL预处理对象
		PreparedStatement stmt = null;
		// 受影响行
		int affectedRow = 0;
		try {
			// 注册mysql数据库jdbc连接驱动
			Class.forName("com.mysql.jdbc.Driver");
			// 连接url 格式 jdbc:数据库类型://服务器地址:端口/数据库名
			String url = "jdbc:mysql://localhost:3306/testdb";
			// 用户名
			String user = "liuwel";
			// 密码
			String password = "passwd";
			// 获取连接对象
			conn = DriverManager.getConnection(url, user, password);
			// SQL
			String sql = " insert into user (id,username,password,createdAt,updatedAt) "
					+ " values (?,?,?,?,?)";
			// 预处理SQL
			stmt = conn.prepareStatement(sql);
			// 设置第一个绑定数据 id
			stmt.setInt(1, 1);
			// 设置第二个绑定数据 username
			stmt.setString(2, "liuwel");
			// 设置第三个绑定数据 password
			stmt.setString(3, "passwd");
			// 设置第四个绑定数据 createdAt
			stmt.setDate(4, new java.sql.Date(System.currentTimeMillis()));
			// 设置第五个绑定数据 updatedAt 
			stmt.setDate(5, new java.sql.Date(System.currentTimeMillis()));
			// 执行sql 返回受影响行
			affectedRow = stmt.executeUpdate();
			if (affectedRow > 0) {
				System.out.println("增加成功！");
			} else {
				System.out.println("增加失败！");
			}
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
		    // 关闭资源 
			if (stmt != null) {
				try {
					stmt.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
			if (conn != null) {
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
		}
	}
}

```
