# 🌍 出境签证材料查询网站 — 部署指南

## 文件说明

| 文件 | 作用 |
|------|------|
| `index.html` | 主站 — 签证查询、材料追踪、机票酒店、行程总览 |
| `admin.html` | 管理后台 — 增删改签证数据（需要密码登录） |
| `firebase.json` | Firebase 部署配置 |
| `firestore.rules` | 数据库读写权限规则 |

## 一、准备工作（约 10 分钟）

### 1.1 注册 Firebase
打开 [console.firebase.google.com](https://console.firebase.google.com)，用你的 Google 账号登录。

### 1.2 创建项目
1. 点击 **「添加项目」** / **「Create a project」**
2. 项目名称填：`travel-visa-planner`（或自定）
3. Google Analytics：**关闭**（不需要）
4. 点击 **「创建项目」**

### 1.3 启用 Firestore 数据库
1. 左侧菜单 → **「Firestore Database」**
2. 点击 **「创建数据库」**
3. 位置选择：`asia-east1`（台湾，国内访问快）或 `nam5`（美国）
4. 安全规则选 **「测试模式」**（我们会上传自己的规则）

### 1.4 获取 Firebase 配置
1. 左侧菜单 → **「项目设置」**（齿轮图标）
2. 在 **「您的应用」** 底部，点击 **「Web」** 图标（`</>`）
3. 应用昵称填：`travel-planner`
4. **复制**弹出的 `firebaseConfig` 对象（包含 apiKey, projectId 等）
5. 将这个配置**替换**下面两个文件中的 `firebaseConfig`：
   - `index.html`（搜索 `firebaseConfig`）
   - `admin.html`（搜索 `firebaseConfig`）

## 二、上传签证数据

### 方法：通过管理后台导入（推荐）
1. 部署完成后，打开 `https://你的域名/admin.html`
2. 用默认密码 `admin123` 登录
3. 点击 **「📥 导入默认数据」** → 一键导入 34 个国家
4. 之后可在后台随时编辑

## 三、部署上线

### 3.1 安装 Firebase CLI
在电脑上打开终端（Terminal / PowerShell），运行：
```bash
npm install -g firebase-tools
```

### 3.2 登录 Firebase
```bash
firebase login
```
浏览器会自动弹出，选择你的 Google 账号登录。

### 3.3 初始化 & 部署
```bash
cd C:\Users\12183\Desktop\travel-planner-site

# 选择刚才创建的项目
firebase use --add

# 部署！
firebase deploy
```

部署成功后，终端会显示你的网址：
```
Hosting URL: https://travel-visa-planner.web.app
```

## 四、使用

### 普通用户
- 访问 `https://你的域名` → 查看签证信息、追踪材料进度
- 个人行程数据保存在**浏览器本地**（换设备需导出/导入备份）

### 管理员
- 访问 `https://你的域名/admin.html`
- 登录密码：`admin123`（登录后可修改）
- 可以：添加/编辑/删除签证数据
- **修改后所有用户实时看到更新**

## 五、分享给朋友

把域名（如 `https://travel-visa-planner.web.app`）发给朋友即可！

管理后台也可以分享给信得过的朋友，他们也能编辑签证数据。

## 六、常见问题

**Q: 为什么不直接内置 34 国数据，还要导入？**
A: 为了方便后续维护。数据存在 Firestore 数据库中，修改后所有人立刻看到，不需要重新部署。

**Q: 免费额度够用吗？**
A: Firebase 免费额度每月 10GB 流量、1GB 存储、5 万次读取/天。个人和小团队用完全够。

**Q: 如何更新签证数据？**
A: 打开 `admin.html` → 登录 → 编辑对应国家 → 保存。所有用户实时同步。

**Q: 忘记管理密码怎么办？**
A: 去 Firebase Console → Firestore Database → `config` 集合 → `admin` 文档 → 查看或修改 `password` 字段。

**Q: 安全吗？**
A: 签证数据是公开信息，允许任何人读取。管理后台用密码保护，Firestore 规则允许写入（通过 admin.html 密码验证）。如需更高安全性，可改用 Firebase Auth。
