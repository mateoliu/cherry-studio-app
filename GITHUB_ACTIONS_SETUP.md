# GitHub Actions 自动构建 Android APK 详细指南

## 📋 概述

本指南将帮助你设置 GitHub Actions 来自动构建 Cherry Studio App 的 Android APK，无需本地编译环境。

## 🚀 快速开始

### 步骤 1: 注册 Expo 账号

1. 访问 [https://expo.dev/](https://expo.dev/)
2. 点击 "Sign up" 注册新账号
3. 验证邮箱并完成注册

### 步骤 2: 获取 Expo Access Token

1. 登录 Expo 账号后，点击右上角头像
2. 选择 "Account settings"
3. 在左侧菜单选择 "Access tokens"
4. 点击 "Create token"
5. 输入 token 名称（如：github-actions）
6. 复制生成的 token（只显示一次，请妥善保存）

### 步骤 3: 配置 GitHub Secrets

1. 进入你的 GitHub 仓库页面
2. 点击 "Settings" 标签
3. 在左侧菜单选择 "Secrets and variables" → "Actions"
4. 点击 "New repository secret"
5. 添加以下配置：
   - **Name**: `EXPO_TOKEN`
   - **Secret**: 粘贴刚才复制的 Expo Access Token
6. 点击 "Add secret"

### 步骤 4: 修改应用配置

编辑 `app.config.ts` 文件，修改包名：

```typescript
android: {
  adaptiveIcon: {
    foregroundImage: './src/assets/images/adaptive-icon.png',
    backgroundColor: '#F65D5D'
  },
  package: 'com.yourname.cherrystudio'  // 改成你的唯一包名
}
```

### 步骤 5: 运行构建

#### 方法 1: 手动触发构建

1. 进入 GitHub 仓库的 "Actions" 页面
2. 选择 "Build Android (Simple)" workflow
3. 点击 "Run workflow" 按钮
4. 选择构建配置：
   - `preview`: 构建 APK 文件（推荐）
   - `production`: 构建 AAB 文件（用于 Google Play）
5. 点击绿色的 "Run workflow" 按钮

#### 方法 2: 自动触发构建

推送代码到 `main` 或 `master` 分支会自动触发构建。

## 📱 下载 APK

### 从 Expo 网站下载

1. 访问 [https://expo.dev/](https://expo.dev/)
2. 登录你的账号
3. 进入项目页面：`https://expo.dev/accounts/[你的用户名]/projects/cherry-studio/builds`
4. 找到状态为 "Finished" 的构建
5. 点击构建项，然后点击 "Download" 下载 APK

### 构建状态说明

- **In Queue**: 构建在队列中等待
- **In Progress**: 正在构建中
- **Finished**: 构建完成，可以下载
- **Failed**: 构建失败，需要检查日志

## 🔧 故障排除

### 常见问题

1. **构建失败 - Token 错误**
   - 检查 GitHub Secrets 中的 `EXPO_TOKEN` 是否正确设置
   - 确保 token 没有过期

2. **构建失败 - 包名冲突**
   - 修改 `app.config.ts` 中的 `android.package` 为唯一值
   - 格式：`com.yourname.appname`

3. **依赖安装失败**
   - 检查 `package.json` 中的依赖是否正确
   - 确保 `yarn.lock` 文件存在

4. **数据库生成失败**
   - 检查 `drizzle.config.ts` 配置
   - 确保数据库相关文件存在

### 本地测试（可选）

如果需要本地测试构建：

```bash
# 安装 EAS CLI
npm install -g @expo/eas-cli

# 登录 Expo
eas login

# 本地构建测试
eas build --platform android --profile preview --local
```

## 📊 构建配置说明

### Preview 配置
- 输出：APK 文件
- 用途：测试和分发
- 签名：Expo 管理的签名

### Production 配置
- 输出：AAB 文件
- 用途：Google Play Store 发布
- 签名：需要配置发布签名

## 🎯 高级配置

### 自定义构建脚本

如果需要更复杂的构建流程，可以修改 `.github/workflows/build-android.yml` 文件。

### 添加构建通知

可以在 workflow 中添加 Slack 或邮件通知：

```yaml
- name: Notify on success
  if: success()
  run: echo "Build completed successfully!"
```

## 📝 注意事项

1. **免费额度**: Expo 免费账号每月有构建次数限制
2. **构建时间**: 通常需要 10-20 分钟完成构建
3. **文件大小**: APK 文件通常在 50-100MB
4. **版本管理**: 每次构建会自动递增版本号

## 🆘 获取帮助

如果遇到问题：

1. 查看 GitHub Actions 的构建日志
2. 检查 Expo 网站的构建详情
3. 参考 [Expo 官方文档](https://docs.expo.dev/build/introduction/)
4. 在项目 Issues 中提问

---

🎉 现在你可以在云端自动构建 Android APK 了！