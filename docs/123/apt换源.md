
# 更换 Ubuntu 系统的 APT 源为阿里云源

通过更换 APT 源为阿里云源，可以提高 Ubuntu 系统的软件安装和更新速度，特别是在中国地区。

## 1. 备份原有的源列表

在开始修改前，建议先备份当前的 APT 源列表，以便需要时可以恢复。

执行以下命令备份源列表：

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

## 2. 编辑 APT 源列表

使用文本编辑器打开 `sources.list` 文件：

```bash
sudo nano /etc/apt/sources.list
```

## 3. 替换为阿里云的源

删除文件中的原有源地址，替换为阿里云提供的源。根据你的 Ubuntu 版本，修改文件内容为以下示例：

### 对于 Ubuntu 20.04 (Focal Fossa)：

```bash
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
```

### 对于 Ubuntu 18.04 (Bionic Beaver)：

```bash
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
```

### 对于其他版本的 Ubuntu：

你可以访问 [阿里云镜像站](http://mirrors.aliyun.com/) 找到对应版本的源地址。

## 4. 更新软件包列表

修改完 `sources.list` 后，保存并退出编辑器。然后，更新 APT 的软件包列表：

```bash
sudo apt update
```

## 5. 升级系统

可以选择升级系统中的所有包：

```bash
sudo apt upgrade
```

## 6. 恢复原始源（如果需要）

如果你想恢复原来的源列表，可以使用备份的 `sources.list` 文件：

```bash
sudo cp /etc/apt/sources.list.bak /etc/apt/sources.list
```

然后再次运行 `sudo apt update` 更新软件包列表。

## 总结

- 备份原有的源列表。
- 修改 `/etc/apt/sources.list` 文件为阿里云源。
- 运行 `sudo apt update` 更新软件包列表。
- 可选择 `sudo apt upgrade` 升级所有包。

通过以上步骤，你就成功将 Ubuntu 系统的 APT 源更换为阿里云源。
