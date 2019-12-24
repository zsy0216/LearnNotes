# 个人博客数据库设计

## 用户表-用于登录后台

`tb_user` 

| Filed         | Type         | Null | Key     | Extra          |
| ------------- | ------------ | ---- | ------- | -------------- |
| id            | int(11)      | No   | Primary | auto_increment |
| user_name     | varchar(20)  | No   |         | 用户名         |
| user_password | varchar(50)  | No   |         | 用户密码       |
| user_email    | varchar(30)  | No   |         | 用户邮箱       |
| user_nickname | varchar(20)  | No   |         | 用户昵称       |
| user_remark   | varchar(255) |      |         | 备注           |

## 博文表-文章

`tb_blog` 

| Filed            | Type         | Null | Key     | Extra                     |
| ---------------- | ------------ | ---- | ------- | ------------------------- |
| id               | int(11)      | No   | Primark | auto_increment            |
| user_id          | int(11)      | No   |         | 用户id                    |
| blog_title       | varchar(50)  | No   |         | 文章标题                  |
| blog_subtitle    | varchar(255) |      |         | 文章副标题                |
| blog_cover_image | varchar(200) |      |         | 文章封面，文件上传        |
| blog_content     | mediumtext   | No   |         | 文章内容                  |
| blog_status      | tinyint(4)   | No   |         | 0-草稿 1-发布             |
| blog_views       | bigint(20)   | No   |         | 阅读量                    |
| blog_create_date | datetime     | No   |         | 文章创建日期              |
| blog_modify_date | datetime     | No   |         | 文章修改日期              |
| is_deleted  | tinyint(4)   |    |         | 是否删除0-未删除 1-已删除 |
| blog_remark      | varchar(255) |      |         |                           |

## 分类表-文章分类

`tb_category` 

| Filed                | Type         | Null | Key     | Extra          |
| -------------------- | ------------ | ---- | ------- | -------------- |
| id                   | int(11)      | No   | Primary | auto_increment |
| category_name        | varchar(20)  | No   |         | 分类名称       |
| category_create_time | datetime     | No   |         | 分类创建时间   |
| is_deleted  | tinyint(4)   |    |         | 是否删除0-未删除 1-已删除 |
| category_remark      | varchar(255) |      |         |                |

## 标签表-文章标签

`tb_tag` 

| Filed           | Type         | Null | Key     | Extra          |
| --------------- | ------------ | ---- | ------- | -------------- |
| id              | int(11)      | No   | Primary | auto_increment |
| tag_name        | varchar(20)  | No   |         | 标签名称       |
| tag_create_time | varchar(20)  | No   |         | 标签创建时间   |
| is_deleted  | tinyint(4)   |    |         | 是否删除0-未删除 1-已删除 |
| tag_remark      | varchar(255) |      |         |                |

## 文章-分类-关联表

`tb_blog_category` 

| Filed           | Type         | Null | Key     | Extra    |
| --------------- | ------------ | ---- | ------- | -------- |
| blog_id         | int(11)      | No   | Primary | 联合主键 |
| category_id     | int(11)      | No   | Primary | 联合主键 |
| relation_remark | varchar(255) |      |         | 备注     |

## 文章-标签-关联表

`tb_blog_tag` 

| Filed           | Type         | Null | Key     | Extra    |
| --------------- | ------------ | ---- | ------- | -------- |
| blog_id         | int(11)      | No   | Primary | 联合主键 |
| tag_id          | int(11)      | No   | Primary | 联合主键 |
| relation_remark | varchar(255) |      |         | 备注     |

## 评论表-文章评论

`tb_comment` 

| Filed | Type    | Null | Key     | Extra |
| ----- | ------- | ---- | ------- | ----- |
| id    | int(11) | No   | Primary |auto_increment       |
| blog_id | int(11) | No |  | 关联的博文的id |
| commentator | varchar(20) | No |         | 评论人 |
| email | varchar(50) | No |         | 评论人邮箱 |
| website | varchar(50) |      |         | 评论人站点，可空 |
| comment_body | varchar(255) | No |         | 评论内容 |
| comment_create_time | datetime | No |         | 评论提交时间 |
| is_deleted | tinyint(4) |      |         | 是否删除0-未删除 1-已删除      |

