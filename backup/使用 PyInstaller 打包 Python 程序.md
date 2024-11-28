首先，确保已经安装了 Python 和 pip。然后在命令行中运行以下命令来安装 PyInstaller：

```
pip install pyinstaller
```

**打包 Python 脚本**

```
pyinstaller --onefile your_script.py
```

这里的 `your_script.py` 是你要打包的 Python 文件名。`--onefile` 参数表示将所有内容打包成一个单独的 EXE 文件。

更多参数可以查看官方文档：[PyInstaller 的官方文档](https://pyinstaller.readthedocs.io/en/stable/)

常用：**添加图标**：如果你想为生成的 EXE 文件添加图标，可以使用 `--icon` 参数：

```
pyinstaller --onefile --icon=your_icon.ico your_script.py
```

**隐藏控制台窗口**：如果你的程序是 GUI 应用程序，并且不需要控制台窗口，可以使用 `--noconsole` 参数：

```
pyinstaller --onefile --noconsole your_script.py
```



### 注意事项

- 确保你的 Python 脚本在打包前能够正常运行。
- 打包的 EXE 文件可能会比较大，因为它包含了 Python 解释器和所有依赖库。
- 在某些情况下，防病毒软件可能会误报生成的 EXE 文件，确保在分发之前进行测试。







