# PyInstaller 使用指南

PyInstaller是一个流行的Python打包工具，可以将Python应用程序及其依赖项打包成一个独立的可执行文件。

## 1. 安装

```bash
pip install pyinstaller
```

## 2. 基本用法

### 2.1 最简单的打包命令

```bash
pyinstaller your_script.py
```

### 2.2 常用参数

- `-F` 或 `--onefile`: 打包成单个可执行文件
- `-w` 或 `--windowed`: 打包成GUI程序（不显示控制台窗口）
- `-n NAME`: 指定生成的可执行文件名称
- `--icon=ICON.ico`: 指定程序图标
- `-D` 或 `--onedir`: 创建包含可执行文件的单个文件夹（默认）

### 2.3 打包示例

```bash
# 基础打包
pyinstaller script.py

# 打包成单文件
pyinstaller -F script.py

# 打包GUI程序
pyinstaller -F -w --icon=app.ico script.py
```

## 3. 资源文件处理

### 3.1 添加数据文件

```bash
# Windows
pyinstaller --add-data "folder;folder" script.py

# Linux/Mac
pyinstaller --add-data "folder:folder" script.py
```

### 3.2 添加多个数据文件
```bash
# Windows
pyinstaller --add-data "config;config" --add-data "images;images" --add-data "data/file.json;data" script.py

# Linux/Mac
pyinstaller --add-data "config:config" --add-data "images:images" --add-data "data/file.json:data" script.py
```

也可以在spec文件中配置：
```python
a = Analysis(
    ['script.py'],
    datas=[
        ('config/*', 'config'),        # 添加整个config文件夹
        ('images/*.png', 'images'),    # 添加所有PNG图片
        ('data/file.json', 'data'),    # 添加单个文件
        ('*.txt', '.'),                # 添加根目录下所有txt文件
    ],
    # ...其他配置
)
```

### 3.3 在代码中访问资源文件

```python
import sys
import os

def resource_path(relative_path):
    if hasattr(sys, '_MEIPASS'):
        return os.path.join(sys._MEIPASS, relative_path)
    return os.path.join(os.path.abspath("."), relative_path)
```

## 4. 常见问题解决

### 4.1 模块导入问题
- 使用 `--hidden-import` 添加隐藏导入
- 检查 `dist` 目录下是否包含所需文件
- 确保所有依赖都已正确安装

### 4.2 文件体积优化

#### 4.2.1 使用虚拟环境
```bash
# 创建并激活虚拟环境
python -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows

# 只安装必要的包
pip install 包名==版本号

# 导出依赖
pip freeze > requirements.txt
```

#### 4.2.2 排除不需要的模块
```bash
# 排除单个模块
pyinstaller --exclude-module matplotlib script.py

# 排除多个模块
pyinstaller --exclude-module matplotlib --exclude-module numpy --exclude-module pandas script.py
```

在spec文件中配置：
```python
a = Analysis(
    ['script.py'],
    excludes=['matplotlib', 'numpy', 'pandas', 'PIL'],
)
```

#### 4.2.3 UPX压缩
```bash
# 1. 安装UPX
# Windows: 下载UPX并添加到PATH
# Linux: sudo apt-get install upx
# Mac: brew install upx

# 2. 使用UPX压缩
pyinstaller --upx-dir=/path/to/upx script.py

# 3. 设置压缩级别（0-9，默认为6）
pyinstaller --upx-dir=/path/to/upx --upx-compress=9 script.py
```

#### 4.2.4 清理编译文件
```bash
# 打包前清理
find . -type f -name "*.pyc" -delete
find . -type d -name "__pycache__" -delete
```

#### 4.2.5 其他优化技巧

1. **按需导入**
```python
# 不推荐
import numpy as np  # 程序启动就加载

# 推荐
def process_data():
    import numpy as np  # 需要时才加载
    # 处理数据
```

2. **使用轻量级替代库**
```python
# 不推荐
from PIL import Image  # 较大

# 推荐
import cv2  # 较小且更快

# 不推荐
import pandas as pd  # 较大

# 推荐
import csv  # 标准库，体积小
```

3. **分析依赖**
```bash
# 使用pipdeptree查看依赖树
pip install pipdeptree
pipdeptree

# 使用PyInstaller的依赖分析
pyi-archive_viewer dist/script.exe
```

4. **模块化打包**
- 将大型应用拆分成多个小模块
- 使用动态加载机制
- 考虑使用插件架构

5. **监控打包大小**
```bash
# 显示文件大小统计
du -h dist/script.exe

# Windows下使用dir
dir /s dist\script.exe
```

这些优化方法可以组合使用，根据具体项目需求选择合适的优化策略。优化后建议：
- 测试程序功能完整性
- 检查启动速度
- 验证在不同环境下的兼容性

## 5. spec文件配置

```python
# example.spec
a = Analysis(
    ['script.py'],        # 主脚本文件
    pathex=[],           # 额外的导入路径
    binaries=[],         # 需要包含的二进制文件
    datas=[
        ('assets/*', 'assets')  # 格式：(源文件路径, 目标文件夹)
    ],
    hiddenimports=[],    # 隐式导入的模块列表
    hookspath=[],        # 自定义钩子脚本路径
    runtime_hooks=[],    # 运行时钩子脚本
    excludes=[],         # 需要排除的模块
    noarchive=False      # True则不打包成zip文件
)

# 创建PKG归档文件(ZIP压缩包)
pyz = PYZ(
    a.pure,             # 纯Python模块
    a.zipped_data,      # 压缩的数据文件
)

# 生成可执行文件
exe = EXE(
    pyz,                # 上面创建的PYZ对象
    a.scripts,          # 脚本
    a.binaries,         # 二进制文件
    a.zipfiles,         # ZIP文件
    a.datas,           # 数据文件
    name='MyApp',       # 生成的可执行文件名
    debug=False,        # 是否包含调试信息
    strip=False,        # 是否移除符号表(减小体积)
    upx=True,          # 是否使用UPX压缩
    console=False,      # True显示控制台窗口，False则不显示
    icon='app.ico',     # 应用图标路径
    # 其他常用选项：
    # bootloader_ignore_signals=False,  # 是否忽略引导加载器信号
    # disable_windowed_traceback=False, # 是否禁用窗口模式的错误回溯
    # target_arch=None,                 # 目标架构
    # codesign_identity=None,          # 代码签名身份
    # entitlements_file=None,          # 授权文件路径
)
```

## 6. 最佳实践

1. **开发环境管理**
   - 使用虚拟环境开发
   - 记录依赖 `pip freeze > requirements.txt`
   - 定期清理不用的包

2. **打包前检查**
   - 确保程序在命令行正常运行
   - 检查所有资源文件路径
   - 验证第三方库的兼容性

3. **测试验证**
   - 在目标环境测试运行
   - 检查所有功能是否正常
   - 验证资源文件是否可以正确访问
