



以下是一份 **cURL 命令入门手册**，包含基础语法和常见使用场景的代码示例：

---

## cURL 入门手册

### 1. 什么是 cURL？
cURL 是一个用于传输数据的命令行工具，支持 HTTP、HTTPS、FTP、SFTP 等多种协议。常用于测试 API、下载文件、调试网络请求等。

---

### 2. 安装 cURL
• **Linux**（Debian/Ubuntu）:  
  ```bash
  sudo apt-get install curl
  ```
• **Linux**（Red Hat/CentOS）:  
  ```bash
  sudo yum install curl
  ```
• **macOS**: 预装，或通过 Homebrew 更新：`brew install curl`
• **Windows**: 下载 [官方二进制包](https://curl.se/windows/) 或通过 Chocolatey 安装。

---

### 3. 基础语法
```bash
curl [options] [URL]
```

---

### 4. 常用场景与示例

#### 场景 1: 发送 GET 请求
```bash
# 获取网页内容
curl https://example.com

# 将结果保存到文件
curl -o output.html https://example.com

# 显示响应头信息
curl -I https://example.com
```

#### 场景 2: 发送 POST 请求
```bash
# 发送表单数据（Content-Type: application/x-www-form-urlencoded）
curl -X POST -d "name=John&age=30" https://api.example.com/users

# 发送 JSON 数据
curl -X POST -H "Content-Type: application/json" -d '{"name":"John", "age":30}' https://api.example.com/users

# 从文件读取 POST 数据
curl -X POST -d "@data.json" https://api.example.com/users
```

#### 场景 3: 上传文件
```bash
# 上传单个文件（表单形式）
curl -F "file=@/path/to/file.jpg" https://api.example.com/upload

# 上传多个文件
curl -F "file1=@file1.jpg" -F "file2=@file2.jpg" https://api.example.com/upload
```

#### 场景 4: 设置 HTTP 头
```bash
# 自定义 User-Agent
curl -H "User-Agent: MyCustomAgent" https://example.com

# 设置多个 Header
curl -H "Content-Type: application/json" -H "Authorization: Bearer abc123" https://api.example.com/data
```

#### 场景 5: 处理 Cookie
```bash
# 保存响应中的 Cookie 到文件
curl -c cookies.txt https://example.com/login

# 使用 Cookie 发送后续请求
curl -b cookies.txt https://example.com/dashboard
```

#### 场景 6: 认证与权限
```bash
# Basic 认证
curl -u username:password https://api.example.com/secure

# Bearer Token 认证
curl -H "Authorization: Bearer YOUR_TOKEN" https://api.example.com/data
```

#### 场景 7: 高级功能
```bash
# 限制下载速度（100KB/s）
curl --limit-rate 100K -O https://example.com/largefile.zip

# 跟随重定向（自动跳转）
curl -L https://example.com/redirect

# 仅显示 HTTP 状态码
curl -o /dev/null -s -w "%{http_code}\n" https://example.com

# 调试模式（显示详细日志）
curl -v https://example.com
```

---

### 5. 常见问题
#### 问题 1: 如何忽略 SSL 证书验证？
```bash
curl -k https://insecure-website.com  # 不推荐生产环境使用！
```

#### 问题 2: 如何断点续传？
```bash
# 继续未完成的下载
curl -C - -O https://example.com/largefile.zip
```

---

### 6. 常用参数速查表
| 参数 | 说明 |
|------|------|
| `-X [METHOD]` | 指定 HTTP 方法（如 GET/POST） |
| `-H "Header: Value"` | 添加 HTTP 头 |
| `-d "data"` | 发送 POST 数据 |
| `-o filename` | 输出到文件 |
| `-O` | 使用远程文件名保存 |
| `-L` | 跟随重定向 |
| `-v` | 显示详细日志 |

---

### 7. 更多学习资源
• 官方文档: https://curl.se/docs/
• `man curl`（命令行手册）
希望这份手册能帮助你快速掌握 cURL！