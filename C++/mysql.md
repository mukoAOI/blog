本文将介绍如何使用 C++ 连接 MySQL 数据库并执行 SQL 查询,以下将展示一个基础的 C++ 程序如何与 MySQL 数据库交互，进行数据查询，并给出相应的优化建议。



cmake + mysql + C++



先搞一个表

```sql
 CREATE DATABASE db;
 CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    hire_date DATE,
    salary DECIMAL(10, 2));
    
INSERT INTO employees (first_name, last_name, hire_date, salary)
VALUES
('John', 'Doe', '2022-01-15', 50000.00),
('Jane', 'Smith', '2021-10-23', 55000.00),
('Bob', 'Johnson', '2020-04-11', 60000.00),
('Alice', 'Williams', '2019-03-29', 62000.00); 
```

```bash
+----+------------+-----------+------------+----------+
| id | first_name | last_name | hire_date  | salary   |
+----+------------+-----------+------------+----------+
|  1 | John       | Doe       | 2022-01-15 | 50000.00 |
|  2 | Jane       | Smith     | 2021-10-23 | 55000.00 |
|  3 | Bob        | Johnson   | 2020-04-11 | 60000.00 |
|  4 | Alice      | Williams  | 2019-03-29 | 62000.00 |
+----+------------+-----------+------------+----------+
```

cmake配置  c api



```cmake
include_directories(/usr/include/mysql)
target_link_libraries(${PROJECT_NAME}  PRIVATE mysqlclient)
```

```c++
#include <mysql/mysql.h>
#include <iostream>
#include <cstdlib>  // For exit()

int main() {
    // 创建 MySQL 连接对象
    MYSQL* conn = mysql_init(nullptr);
    
    if (conn == nullptr) {
        std::cerr << "mysql_init failed\n";
        return EXIT_FAILURE;
    }

    // 连接数据库
    if (mysql_real_connect(conn, "localhost", "username", "password", "database_name", 3306, nullptr, 0) == nullptr) {
        std::cerr << "mysql_real_connect failed: " << mysql_error(conn) << "\n";
        mysql_close(conn);
        return EXIT_FAILURE;
    }

    // 查询 SQL 语句
    const char* query = "SELECT id, first_name, last_name, hire_date, salary FROM employees";

    // 执行查询
    if (mysql_query(conn, query)) {
        std::cerr << "SELECT query failed: " << mysql_error(conn) << "\n";
        mysql_close(conn);
        return EXIT_FAILURE;
    }

    // 获取查询结果
    MYSQL_RES* res = mysql_store_result(conn);
    if (res == nullptr) {
        std::cerr << "mysql_store_result failed: " << mysql_error(conn) << "\n";
        mysql_close(conn);
        return EXIT_FAILURE;
    }

    // 获取结果集中的字段数量
    int num_fields = mysql_num_fields(res);
    
    // 获取字段名称
    MYSQL_FIELD* fields = mysql_fetch_fields(res);
    for (int i = 0; i < num_fields; i++) {
        std::cout << fields[i].name << "\t";
    }
    std::cout << std::endl;

    // 获取每一行数据
    MYSQL_ROW row;
    while ((row = mysql_fetch_row(res))) {
        for (int i = 0; i < num_fields; i++) {
            std::cout << (row[i] ? row[i] : "NULL") << "\t";
        }
        std::cout << std::endl;
    }

    // 释放查询结果和关闭连接
    mysql_free_result(res);
    mysql_close(conn);

    return 0;
}

```



mysql官方版本

```cmake
include_directories(/usr/include/cppconn)
target_link_libraries(${PROJECT_NAME} PRIVATE mysqlcppconn)
```

```c++
#include <iostream>
#include <mysql_driver.h> // 用于获取 MySQL 驱动
#include <cppconn/statement.h> // 用于执行 SQL 语句
#include <cppconn/resultset.h> // 用于处理查询结果

int main() {
    try {
        // 获取 MySQL 驱动
        sql::mysql::MySQL_Driver* driver = sql::mysql::get_mysql_driver_instance();

        // 创建 MySQL 连接（IP地址、用户名、密码）
        std::unique_ptr<sql::Connection> conn(driver->connect("tcp://127.0.0.1:3306", "root", "490605"));

        // 选择数据库
        conn->setSchema("db");

        // 创建一个 Statement 对象来执行 SQL 查询
        std::unique_ptr<sql::Statement> stmt(conn->createStatement());

        // 执行 SQL 查询
        std::unique_ptr<sql::ResultSet> res(stmt->executeQuery("SELECT id, first_name, last_name, hire_date, salary FROM employees"));

        // 输出查询结果的字段名称
        std::cout << "id\t     first_name\t     last_name\t     hire_date\t     salary\n";

        // 遍历结果集并输出结果
        while (res->next()) {
            std::cout << res->getInt("id") << "\t     ";
            std::cout << res->getString("first_name") << "\t     ";
            std::cout << res->getString("last_name") << "\t     ";
            std::cout << res->getString("hire_date") << "\t     ";
            std::cout << res->getDouble("salary") << std::endl;
        }
    } catch (sql::SQLException& e) {
        std::cerr << "Error occurred: " << e.what() << std::endl;
    }

    return 0;
}

```

