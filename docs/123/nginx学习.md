# 笔记1


---

### 1. 安装 Nginx（如未安装）
```bash
sudo apt update
sudo apt install nginx
sudo systemctl enable --now nginx
```

---

### 2. 配置反向代理（基础示例）

#### 案例需求：
将访问 `your-domain.com` 的请求代理到本地的 `http://localhost:3000`（如 Node.js 应用）

#### 操作步骤：
1. **新建代理配置文件**：
   ```bash
   sudo nano /etc/nginx/sites-available/reverse-proxy.conf
   ```

2. **写入以下配置**：
   ```nginx
   server {
       listen 80;
       server_name your-domain.com;  # 改为你的域名或IP

       location / {
           proxy_pass http://localhost:3000;  # 目标服务器地址
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```

3. **创建符号链接启用配置**：
   ```bash
   sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/
   ```

4. **测试并重载配置**：
   ```bash
   sudo nginx -t          # 检查语法
   sudo systemctl reload nginx  # 应用配置
   ```

---

### 3. 进阶代理配置示例

#### 场景1：路径代理
将 `/api` 路径代理到不同后端：
```nginx
location /api/ {
    proxy_pass http://localhost:8080/;  # 注意结尾的斜杠
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
}
```

#### 场景2：多服务代理
```nginx
# 服务1：主站点
server {
    listen 80;
    server_name app.your-domain.com;
    location / {
        proxy_pass http://localhost:3000;
    }
}

# 服务2：管理后台
server {
    listen 80;
    server_name admin.your-domain.com;
    location / {
        proxy_pass http://localhost:4000;
    }
}
```

---

### 4. 安全增强配置

#### 添加SSL证书（Let's Encrypt）
1. 安装 Certbot：
   ```bash
   sudo apt install certbot python3-certbot-nginx
   ```

2. 获取证书：
   ```bash
   sudo certbot --nginx -d your-domain.com
   ```

3. 自动更新证书：
   ```bash
   sudo certbot renew --dry-run
   ```

#### 强制HTTPS跳转
在配置中添加：
```nginx
server {
    listen 80;
    server_name your-domain.com;
    return 301 https://$host$request_uri;
}
```

---

### 5. 常见问题排查

#### 错误1：502 Bad Gateway
• 检查后端服务是否运行：
  ```bash
  curl http://localhost:3000  # 替换你的端口
  ```
• 查看 Nginx 错误日志：
  ```bash
  tail -f /var/log/nginx/error.log
  ```

#### 错误2：代理后丢失客户端IP
确保配置中包含：
```nginx
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```

#### 防火墙放行
```bash
sudo ufw allow 'Nginx Full'  # 开放 80/443 端口
```

---

### 6. 完整代理模板
```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    # 静态文件缓存配置
    location /static/ {
        alias /var/www/static/;
        expires 30d;
    }

    # 动态请求代理
    location / {
        proxy_pass http://backend-server:8080;
        proxy_http_version 1.1;
        proxy_cache_bypass $http_upgrade;
        
        # 超时设置
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
        
        # 头信息传递
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # 禁止访问敏感文件
    location ~* \.(env|git|svn|htaccess)$ {
        deny all;
        return 403;
    }
}
```

---

### 操作验证
1. 测试代理是否生效：
   ```bash
   curl -H "Host: your-domain.com" http://localhost
   ```

2. 检查请求头信息：
   ```bash
   curl -I your-domain.com
   ```

通过以上配置，您可以实现：
• 请求负载均衡
• 路径级代理
• 多域名托管
• HTTPS 安全加密
• 静态资源缓存优化

建议根据实际业务需求调整超时时间和缓存策略。