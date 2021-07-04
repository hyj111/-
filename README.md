# -
前端笔记

| 属性名   | 数据类型    | 说明     | 主键/外键 |
| -------- | ----------- | -------- | --------- |
| id       | int         | id       | 主键      |
| name     | varchar(20) | 账号     | 否        |
| password | varchar(50) | 密码     | 否        |
| createAt | timestamp   | 创建时间 | 否        |
| updateAt | timestamp   | 修改时间 | 否        |

| 属性名    | 数据类型    | 说明     | 主键/外键 |
| --------- | ----------- | -------- | --------- |
| id        | int         | id       | 主键      |
| user_id   | int         |          | 外键      |
| device_id | varchar(50) | 设备id   | 否        |
| title     | varchar(50) | 设备名   | 否        |
| createAt  | timestamp   | 创建时间 | 否        |
| updateAt  | timestamp   | 修改时间 | 否        |