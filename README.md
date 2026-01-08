# 使用说明


### 图片上传方式

#### 使用PicGo客户端
1. **下载安装** [PicGo](https://github.com/Molunerfinn/PicGo)
2. **配置GitHub图床**：
   - 仓库名：`用户名/仓库名`（如 `Mohan710/image-bed`）
   - 分支：`main` 或 `master`
   - Token：
      - 生成访问令牌（Token）
      - 点击头像 → Settings → Developer settings → Personal access tokens → Tokens (classic)
      - 点击 "Generate new token (classic)"
      - 勾选权限：repo（完全控制仓库）
      - 保存生成的token（只会显示一次）
   - 存储路径：`images/`（可选，用于分类）
   - 自定义域名：`https://cdn.jsdelivr.net/gh/用户名/仓库名@main`

3. **上传图片**：
   - 拖拽图片到PicGo窗口
   - 自动上传并复制链接到剪贴板

### 构建CDN链接
**基本格式**：
```
https://cdn.jsdelivr.net/gh/用户名/仓库名@分支/文件路径
```

**示例**：
- 原始GitHub链接：
  ```
  https://raw.githubusercontent.com/zhangsan/image-bed/main/images/photo.jpg
  ```
- 对应的jsDelivr CDN链接：
  ```
  https://cdn.jsdelivr.net/gh/zhangsan/image-bed@main/images/photo.jpg
  ```

**高级用法**：
1. **指定版本/提交号**（推荐，避免缓存问题）：
   ```
   https://cdn.jsdelivr.net/gh/zhangsan/image-bed@e7d5c5/images/photo.jpg
   ```

2. **自动压缩图片**（添加参数）：
   ```
   https://cdn.jsdelivr.net/gh/zhangsan/image-bed@main/images/photo.jpg?format=webp
   ```


## 注意事项和限制

### **jsDelivr限制**：
1. **文件大小限制**：单文件 ≤ 50MB
2. **仓库大小**：受GitHub限制（建议 ≤ 1GB）
3. **CDN缓存**：更新后有延迟（可通过版本号解决）
4. **流量限制**：理论上无限制，但超大流量可能被限

### **最佳实践**：
1. **使用版本号**：避免使用 `@latest`，改用具体提交hash
2. **图片优化**：上传前压缩图片（推荐[TinyPNG](https://tinypng.com)）
3. **备份重要图片**：GitHub是代码托管平台，不是专业图床
4. **隐私图片**：不要上传敏感/私人照片

### **监控图片访问**：
可以通过GitHub Insights查看仓库流量统计，了解图片访问情况。


## 故障排除

1. **图片无法访问**：
   - 检查仓库是否为Public
   - 确认图片路径正确
   - 检查jsDelivr状态：https://status.jsdelivr.com

2. **更新后不生效**：
   - jsDelivr有缓存，等待几分钟
   - 在URL后添加 `?v=时间戳` 强制刷新

3. **上传权限问题**：
   - 确认GitHub Token有效且权限足够
   - Token需有 `repo` 权限

## 高级技巧


### **使用GitHub Actions自动压缩图片**
创建 `.github/workflows/compress-images.yml`：
```yaml
name: Compress Images
on: [push]
jobs:
  compress:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: calibreapp/image-actions@main
        with:
          githubToken: ${{ secrets.GITHUB_TOKEN }}
```
