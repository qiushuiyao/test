# 部署指南

## 部署选项

### 选项1：GitHub Pages（推荐）
1. 将代码推送到 GitHub 仓库
2. 进入仓库 Settings → Pages
3. 选择分支（通常是 main）和根目录
4. 保存后访问 `https://[用户名].github.io/[仓库名]`

### 选项2：Vercel
1. 导入 GitHub 仓库到 Vercel
2. 框架选择 "Other"
3. 构建命令留空
4. 输出目录留空
5. 部署后获得 `https://[项目名].vercel.app`

### 选项3：Netlify
1. 拖拽 `index.html` 到 Netlify
2. 或连接 GitHub 仓库
3. 自动部署，获得 `https://[随机名].netlify.app`

### 选项4：传统服务器
```bash
# 复制文件到服务器
scp -r domain-icp-page/ user@server:/var/www/html/

# 或使用 rsync
rsync -avz domain-icp-page/ user@server:/var/www/html/
```

## Nginx 配置示例

```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /var/www/domain-icp-page;
    index index.html;

    # 启用 gzip 压缩
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    # 缓存设置
    location ~* \.(html)$ {
        expires -1;
        add_header Cache-Control "no-store, no-cache, must-revalidate, proxy-revalidate";
    }

    location ~* \.(css|js)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # 错误页面
    error_page 404 /index.html;
}
```

## Apache 配置示例

```apache
<VirtualHost *:80>
    ServerName your-domain.com
    DocumentRoot /var/www/domain-icp-page
    
    <Directory /var/www/domain-icp-page>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    
    # 启用压缩
    AddOutputFilterByType DEFLATE text/html text/plain text/css application/javascript
    
    # 设置缓存
    <FilesMatch "\.(html)$">
        Header set Cache-Control "no-store, no-cache, must-revalidate"
    </FilesMatch>
    
    <FilesMatch "\.(css|js)$">
        Header set Cache-Control "max-age=31536000, public, immutable"
    </FilesMatch>
</VirtualHost>
```

## 自定义域名

### 1. 购买域名
在域名注册商处购买域名。

### 2. 配置 DNS
```dns
# A 记录（IPv4）
@    A    服务器IP地址
www  A    服务器IP地址

# 或 CNAME（用于 GitHub Pages/Vercel/Netlify）
@    CNAME    [用户名].github.io
www  CNAME    [用户名].github.io
```

### 3. 等待生效
DNS 变更通常需要 10分钟到48小时生效。

## HTTPS 配置

### 使用 Let's Encrypt
```bash
# 安装 certbot
sudo apt install certbot python3-certbot-nginx

# 获取证书
sudo certbot --nginx -d your-domain.com -d www.your-domain.com

# 自动续期
sudo certbot renew --dry-run
```

### 手动配置
将证书文件放到服务器，然后配置 Nginx：
```nginx
server {
    listen 443 ssl http2;
    server_name your-domain.com;
    
    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/privkey.pem;
    
    # 其他配置...
}
```

## 监控和维护

### 1. 可用性检查
```bash
# 使用 curl 检查
curl -I https://your-domain.com

# 或使用监控服务
# - UptimeRobot
# - Pingdom
# - StatusCake
```

### 2. 日志查看
```bash
# Nginx 访问日志
tail -f /var/log/nginx/access.log

# 错误日志
tail -f /var/log/nginx/error.log
```

### 3. 定期备份
```bash
# 备份网站文件
tar -czf backup-$(date +%Y%m%d).tar.gz /var/www/domain-icp-page/

# 备份配置
cp /etc/nginx/sites-available/your-domain.com ~/backup/
```

## 故障排除

### 页面无法访问
1. 检查 DNS 是否生效：`nslookup your-domain.com`
2. 检查服务器是否运行：`systemctl status nginx`
3. 检查防火墙：`ufw status`
4. 检查端口：`netstat -tlnp | grep :80`

### 备案信息不显示
1. 检查浏览器控制台是否有错误
2. 检查网络请求是否被阻止
3. 确保文件权限正确：`chmod 644 index.html`

### 移动端显示异常
1. 使用浏览器开发者工具模拟移动设备
2. 检查 viewport meta 标签
3. 测试不同屏幕尺寸

## 性能优化

### 1. 图片优化
- 使用 WebP 格式
- 压缩图片大小
- 使用懒加载

### 2. 代码优化
- 压缩 HTML/CSS/JS
- 使用 CDN 加载库文件
- 减少 HTTP 请求

### 3. 服务器优化
- 启用 HTTP/2
- 配置合适的缓存策略
- 使用 CDN 服务

## 安全建议

1. **定期更新**：保持服务器和软件更新
2. **防火墙**：只开放必要端口
3. **备份**：定期备份网站和配置
4. **监控**：设置异常访问告警
5. **SSL**：始终使用 HTTPS

---

**部署完成检查清单**：
- [ ] 页面可访问
- [ ] HTTPS 正常工作
- [ ] 移动端显示正常
- [ ] 备案信息正确显示
- [ ] 监控已设置
- [ ] 备份计划已制定