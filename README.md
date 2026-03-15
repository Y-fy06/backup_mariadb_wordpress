# WordPress + MariaDB 备份管理系统

一个轻量级、模块化的 WordPress 网站与 MariaDB 数据库自动化备份解决方案。

## 功能特性

- **交互式菜单**：友好的菜单界面，操作简单直观
- **环境检测**：自动检查所需命令、目录权限和数据库连接
- **数据库备份**：使用 mysqldump 导出 MariaDB 数据库
- **文件备份**：打包 WordPress 网站文件
- **日志记录**：详细的操作日志，便于问题排查
- **自动清理**：根据配置自动清理旧备份

## 快速开始

### 前置要求

- Linux 操作系统（CentOS、Ubuntu、Debian 等）
- MariaDB 或 MySQL 数据库
- 必要命令：tar、gzip、mysqldump

### 安装与配置

1. 克隆或下载项目

```bash
git clone https://github.com/Y-fy06/backup_mariadb_wordpress.git
cd backup_mariadb_wordpress
```

2. 配置参数

```bash
vim data.conf
```

根据您的环境修改以下配置项：

```bash
# WordPress 安装目录
WP_ROOT="/var/www/wordpress"

# MariaDB 数据库配置
DB_ROOT_PASS="your_root_password"
DB_NAME="your_database_name"
DB_USER="your_database_user"
DB_USER_PASS="your_user_password"
DB_HOST="localhost"
DB_PORT="3306"

# 备份保留天数
KEEP_BACKUP_DAYS="7"
```

3. 设置执行权限

```bash
chmod +x start.sh modules/path.sh modules/backup.sh
```

### 使用方法

#### 交互式菜单模式（推荐）

直接运行脚本，会显示交互式菜单：

```bash
./start.sh
```

菜单选项：

- **1. 环境检测** - 检查系统环境是否满足备份要求
- **2. 执行备份** - 备份数据库和 WordPress 文件
- **3. 帮助信息** - 显示使用说明
- **0. 退出** - 退出脚本

#### 命令行模式

也支持通过参数直接执行：

```bash
./start.sh check    # 环境检测
./start.sh backup  # 执行备份
./start.sh help    # 帮助信息
```

#### 子脚本独立执行

各模块也可以单独执行：

```bash
./modules/path.sh       # 单独运行环境检测
./modules/backup.sh    # 单独执行备份
```

备份文件将保存在 `backup/` 目录，文件名格式为：`backup_YYYYMMDD_HHMMSS.tar.gz`

### 定时任务（可选）

设置自动备份任务，每天凌晨 3 点执行：

```bash
crontab -e
```

添加以下行：

```bash
0 3 * * * /path/to/backup_mariadb_wordpress/start.sh backup >> /path/to/backup_mariadb_wordpress/log/cron.log 2>&1
```

## 目录结构

```
backup_mariadb_wordpress/
├── start.sh              # 主入口脚本（交互式菜单）
├── data.conf             # 配置文件（敏感，请勿提交）
├── data.conf.example     # 配置文件模板
├── .gitignore            # Git 忽略规则
├── LICENSE               # MIT 许可证
├── README.md             # 项目说明文档
├── docs/
│   └── recovery.md       # 数据恢复指南
├── backup/               # 备份文件存放目录（已忽略）
├── log/                  # 日志文件存放目录（已忽略）
└── modules/
    ├── path.sh           # 环境检测模块
    └── backup.sh         # 备份模块
```

## 数据恢复

当需要恢复数据时，请参考 [docs/recovery.md](docs/recovery.md)

## 安全性说明

- `data.conf` 文件包含数据库密码等敏感信息，已加入 `.gitignore`
- 请勿将包含真实密码的配置文件提交到版本控制系统
- 定期更新数据库密码
- 备份文件应定期转移到安全的位置

## 许可证

本项目采用 MIT 许可证开源，详情请查看 [LICENSE](LICENSE) 文件。

## 贡献

欢迎提交 Issue 和 Pull Request。
