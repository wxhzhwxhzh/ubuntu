
# 使用 Podman 构建一个简单的 Web 服务

本文将带你通过几个简单的步骤，使用 **Podman** 和 **Flask** 构建一个简单的 Web 服务，页面上显示 "Hello World"。

## 1. 安装虚拟环境工具

确保你的系统中安装了 `python3-venv`，用于创建虚拟环境：

```bash
sudo apt install python3-venv
```

## 2. 创建项目目录

创建一个新的目录来存放项目文件：

```bash
mkdir hello-world-web
cd hello-world-web
```

## 3. 创建虚拟环境

使用 `python3 -m venv` 命令创建虚拟环境：

```bash
python3 -m venv venv
```

## 4. 激活虚拟环境

激活虚拟环境：

```bash
source venv/bin/activate
```

激活后，命令行提示符将显示虚拟环境的名称。

## 5. 安装 Flask

在虚拟环境中安装 Flask：

```bash
pip install flask
```

## 6. 创建 Web 服务

在项目目录下创建一个名为 `app.py` 的 Python 文件，并写入以下内容：

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return "Hello World!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

## 7. 运行 Web 服务

确保虚拟环境已激活，然后运行 Flask 应用：

```bash
python app.py
```

此时，Flask 应用将在 `5000` 端口运行。你可以在浏览器中访问 http://localhost:5000  来查看 "Hello World" 页面。

## 8. 创建 `Dockerfile`

为了容器化应用，在项目目录下创建 `Dockerfile`，内容如下：

```Dockerfile
# 使用官方 Python 3.9 镜像作为基础镜像
FROM python:3.9-slim

# 设置工作目录
WORKDIR /app

# 复制当前目录的内容到容器的 /app 目录
COPY . /app

# 安装依赖
RUN pip install --no-cache-dir -r requirements.txt

# 暴露 5000 端口
EXPOSE 5000

# 设置容器启动时的命令
CMD ["python", "app.py"]
```

## 9. 创建 `requirements.txt`

在项目目录下创建 `requirements.txt` 文件，内容如下：

```
flask
```

## 10. 构建容器镜像

使用 Podman 构建镜像：

```bash
podman build -t hello-world-web .
```

## 11. 运行容器

构建完成后，使用以下命令运行容器，并将容器的 5000 端口映射到主机的 8080 端口：

```bash
podman run -d -p 8080:5000 hello-world-web
```

通过访问 http://localhost:8080 来查看 "Hello World" 页面。

## 12. 停止和删除容器

如果完成测试后，可以停止并删除容器：

```bash
podman ps -a  # 查看所有容器
podman stop <容器ID>  # 停止容器
podman rm <容器ID>  # 删除容器
```

## 13. 清理镜像

删除不再需要的镜像：

```bash
podman rmi hello-world-web
```

## 总结

1. 创建并激活虚拟环境，安装 Flask。
2. 编写一个简单的 Flask Web 服务，显示 "Hello World"。
3. 创建 `Dockerfile` 和 `requirements.txt`，定义如何构建容器镜像。
4. 使用 Podman 构建并运行容器，访问 Web 服务。

通过这些步骤，你可以在 Podman 中构建和运行一个简单的 Flask Web 服务。
