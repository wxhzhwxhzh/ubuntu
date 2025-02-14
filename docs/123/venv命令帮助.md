
# Python venv 入门指南

`venv` 是 Python 的内置模块，用于创建虚拟环境。虚拟环境可以隔离项目的依赖，避免不同项目间的包版本冲突。

## 1. 创建虚拟环境

在项目目录中，运行以下命令来创建一个名为 `myenv` 的虚拟环境：

```bash
python -m venv myenv
# 或者
python3 -m venv myenv
```

这将在当前目录创建一个名为 `myenv` 的文件夹，里面包含虚拟环境所需的所有文件。

## 2. 激活虚拟环境

激活虚拟环境使得你可以在该环境中安装和使用包。

### 在 Windows 上：

```bash
myenv\Scripts\activate
```

### 在 macOS 或 Linux 上：

```bash
source myenv/bin/activate
```

激活后，命令行提示符会变化，显示虚拟环境的名称。

## 3. 安装依赖包

在虚拟环境中使用 `pip` 安装包：

```bash
pip install 包名
```

例如，安装 `requests` 包：

```bash
pip install requests
```

## 4. 查看已安装的包

使用以下命令查看当前虚拟环境中已安装的包：

```bash
pip list
```

## 5. 退出虚拟环境

退出虚拟环境：

```bash
deactivate
```

## 6. 删除虚拟环境

删除虚拟环境，只需删除 `myenv` 文件夹：

```bash
rm -rf myenv  # macOS 或 Linux
# 或者在 Windows 上
rmdir /S /Q myenv
```

## 7. 使用 `requirements.txt` 管理依赖

生成项目的依赖列表：

```bash
pip freeze > requirements.txt
```

安装 `requirements.txt` 中列出的所有包：

```bash
pip install -r requirements.txt
```
