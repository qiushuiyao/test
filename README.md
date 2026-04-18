# 域名备案页面 (Domain ICP Page)

一个简洁大气的静态备案页面，用于新域名备案后挂载，确保爬虫和备案机构能正常扫描到。

## 功能特点

- ✅ **简洁大气**：现代设计，响应式布局
- ✅ **备案信息展示**：清晰展示备案主体、备案号等信息
- ✅ **完全静态**：无需后端，直接部署
- ✅ **移动端适配**：完美支持手机和平板
- ✅ **SEO友好**：包含必要的meta标签和结构化数据
- ✅ **易于定制**：修改简单，一键替换信息

## 使用场景

1. **新域名备案后**：挂载此页面等待审核
2. **临时维护页面**：网站维护时显示
3. **合规展示**：展示备案信息，符合法规要求
4. **多域名管理**：为多个备案域名提供统一页面

## 快速开始

### 1. 直接使用
将 `index.html` 上传到你的网站根目录即可。

### 2. 自定义配置
修改 `index.html` 中的以下信息：

```html
<!-- 备案主体 -->
<div class="info-value">个人/企业名称</div>

<!-- 备案号 -->
<div class="info-value">京ICP备XXXXXX号</div>

<!-- 备案时间 -->
<div class="info-value">2026年4月</div>

<!-- 备案所在地 -->
<div class="info-value">北京市</div>
```

### 3. 部署选项

#### GitHub Pages
```bash
# 克隆仓库
git clone https://github.com/qiushuiyao/domain-icp-page.git

# 推送到 GitHub
# 在仓库设置中启用 GitHub Pages
```

#### 静态服务器
```bash
# 使用 Python 快速测试
python3 -m http.server 8000

# 或使用 nginx
# 将文件复制到 nginx 的 html 目录
```

## 文件结构

```
domain-icp-page/
├── index.html          # 主页面文件
├── README.md           # 项目说明
├── .gitignore          # Git忽略文件
└── deploy-guide.md     # 部署指南
```

## 定制选项

### 修改颜色主题
在 `index.html` 的 CSS 中修改颜色变量：

```css
:root {
    --primary-color: #2563eb;    /* 主色 */
    --secondary-color: #3b82f6;  /* 辅色 */
    --text-color: #1f2937;       /* 文字色 */
    --light-bg: #f8fafc;         /* 背景色 */
}
```

### 添加备案查询链接
在备案号处添加查询链接：

```html
<a href="https://beian.miit.gov.cn" target="_blank" style="color: inherit;">
    京ICP备XXXXXX号
</a>
```

## 注意事项

1. **备案信息必须真实**：确保填写真实的备案信息
2. **定期更新**：备案信息变更时及时更新页面
3. **保持在线**：确保页面可访问，供监管机构检查
4. **备份副本**：建议保留页面备份

## 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 贡献

欢迎提交 Issue 和 Pull Request 来改进这个项目。

## 联系

如有问题或建议，请通过 GitHub Issues 联系。

---

**最后更新**: 2026年4月19日
**维护者**: OpenClaw AI Assistant