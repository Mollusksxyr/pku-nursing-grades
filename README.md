# 北京大学2026级护理一班 · 成绩查询系统

在线成绩查询网站，基于 GitHub Pages + Supabase 构建。

## 功能

- **学生注册/登录**：使用学号注册，设置密码和密保问题
- **成绩查询**：登录后查看个人各科成绩，含平均分、等级评定
- **找回密码**：通过密保问题验证身份后重置密码
- **管理员后台**：录入/编辑成绩、添加学生、批量导入、数据导出
- **云端存储**：使用 Supabase 数据库，所有数据实时同步

## 技术栈

- 前端：纯 HTML/CSS/JavaScript（无框架依赖）
- 数据库：Supabase (PostgreSQL)
- 托管：GitHub Pages

## 部署步骤

### 1. 创建 Supabase 项目

1. 前往 [supabase.com](https://supabase.com) 注册（免费）
2. 创建一个新项目，记下 **Project URL** 和 **anon public key**

### 2. 初始化数据库

在 Supabase 控制台的 **SQL Editor** 中执行：

```sql
CREATE TABLE profiles (
  student_id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  password_hash TEXT NOT NULL,
  security_question TEXT,
  security_answer_hash TEXT,
  role TEXT DEFAULT 'student',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE grades (
  student_id TEXT NOT NULL REFERENCES profiles(student_id) ON DELETE CASCADE,
  subject TEXT NOT NULL,
  score NUMERIC(5,1),
  PRIMARY KEY (student_id, subject)
);

ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE grades ENABLE ROW LEVEL SECURITY;
CREATE POLICY "allow_all" ON profiles FOR ALL USING (true) WITH CHECK (true);
CREATE POLICY "allow_all" ON grades FOR ALL USING (true) WITH CHECK (true);
```

### 3. 配置网站

1. 打开网站页面，系统会自动显示配置向导
2. 输入 Supabase Project URL 和 anon key
3. 保存后即可使用

### 4. 管理员登录

- 账号：`admin`
- 默认密码：`admin123`（可在后台修改）

## 使用流程

1. 管理员登录后台 → 批量添加全班同学
2. 管理员为每位同学录入各科成绩
3. 学生自行注册账号 → 登录查看成绩

## License

MIT
