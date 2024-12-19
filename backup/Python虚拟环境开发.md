# Python虚拟环境开发指南

虚拟环境是Python开发中的最佳实践，它可以为不同项目创建独立的Python环境，避免包之间的冲突。

## 1. venv - Python内置虚拟环境工具

### 1.1 创建虚拟环境
```bash
# 基本语法
python -m venv 虚拟环境名称

# 示例
python -m venv venv
python -m venv .venv    # 隐藏文件夹
python -m venv env/project_name
```

### 1.2 激活虚拟环境

```bash
# Windows
venv\Scripts\activate
# 或
.\venv\Scripts\activate

# Linux/Mac
source venv/bin/activate
# 或
. venv/bin/activate
```

### 1.3 退出虚拟环境
```bash
deactivate
```

## 2. 包管理

### 2.1 安装包
```bash
# 安装单个包
pip install package_name

# 安装指定版本
pip install package_name==1.0.0

# 安装多个包
pip install package1 package2

# 从requirements.txt安装
pip install -r requirements.txt
```

### 2.2 导出依赖
```bash
# 导出所有包
pip freeze > requirements.txt

# 导出主要依赖（推荐）
pip-compile requirements.in

# 更新依赖
pip-compile --upgrade requirements.in
```

### 2.3 查看已安装的包
```bash
# 列出所有包
pip list

# 查看包信息
pip show package_name

# 检查过期的包
pip list --outdated
```

## 3. 项目最佳实践

### 3.1 项目结构
```
project/
├── venv/               # 虚拟环境
├── src/                # 源代码
├── tests/              # 测试文件
├── requirements.txt    # 依赖清单
├── requirements.in     # 主要依赖（可选）
└── README.md          # 项目说明
```

### 3.2 依赖管理建议
```bash
# 1. 创建requirements.in（主要依赖）
echo "requests>=2.25.0" >> requirements.in
echo "pandas" >> requirements.in

# 2. 使用pip-tools管理依赖
pip install pip-tools
pip-compile requirements.in
pip-sync requirements.txt
```

### 3.3 开发工具集成

#### VS Code配置
```json
{
    "python.defaultInterpreterPath": "${workspaceFolder}/venv/bin/python",
    "python.terminal.activateEnvironment": true
}
```

#### PyCharm配置
1. File -> Settings -> Project -> Python Interpreter
2. 选择Add Interpreter -> Add Local Interpreter
3. 选择虚拟环境路径

## 4. 常见问题解决

### 4.1 权限问题
```bash
# Windows下以管理员权限运行
# Linux/Mac下使用sudo
sudo python -m venv venv
```

### 4.2 多Python版本管理
```bash
# 指定Python版本创建虚拟环境
python3.8 -m venv venv-3.8
python3.9 -m venv venv-3.9
```

### 4.3 虚拟环境迁移
```bash
# 1. 导出依赖
pip freeze > requirements.txt

# 2. 在新环境中重建
python -m venv new_venv
source new_venv/bin/activate  # 或 new_venv\Scripts\activate
pip install -r requirements.txt
```

## 5. 进阶使用

### 5.1 使用.env文件
```bash
# 安装python-dotenv
pip install python-dotenv

# .env文件示例
DEBUG=True
API_KEY=your_api_key
```

### 5.2 开发和生产环境分离
```
requirements/
├── base.txt        # 基础依赖
├── dev.txt         # 开发环境依赖
└── prod.txt        # 生产环境依赖
```

### 5.3 自动激活虚拟环境
```bash
# 在项目根目录创建 .envrc 文件 (direnv工具)
echo "source venv/bin/activate" > .envrc
direnv allow
```

## 6. 注意事项

1. **版本控制**
   - 将venv/添加到.gitignore
   - 提交requirements.txt
   - 记录Python版本信息

2. **安全建议**
   - 定期更新依赖包
   - 使用pip-audit检查安全漏洞
   - 不要提交敏感配置

3. **性能优化**
   - 使用wheel包加快安装
   - 配置国内镜像源
   - 及时清理不用的包
