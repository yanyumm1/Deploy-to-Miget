不需要那么麻烦了。直接用build-docker-image.yml这个工作流生成镜像去平台通过镜像地址去搭建就好了。

镜像地址不需要填tag
tag版本号填第二个框



# 🚀 使用 GitHub Actions 自动部署到 Miget

本仓库通过 **GitHub Actions** 自动将代码部署到 **Miget** 平台。

只适用于EU区，US区，跑不成功。
---

## 📝 注册账号
注册账号过程略过。

---

## 1️⃣ 生成 `MIGET_TOKEN_KEY`

1. 登录 **Miget 控制台**  
   👉 https://app.miget.com

2. 按照官方文档创建 App  
   👉 https://docs.miget.com/quick-start/dockerfile  
   创建完成后进入你的 **App**

3. 打开路径：  
   **Settings → Git Tokens**

4. 创建或复制 **Git Token**
   - Token 只会 **完整显示一次**
   - 请务必 **妥善保存**

---

## 2️⃣ 在 GitHub 仓库中设置 Secret

进入你的 GitHub 仓库：

**Settings → Secrets and variables → Actions → New repository secret**

填写以下内容：

- **Name**：`MIGET_TOKEN_KEY`
- **Value**：你在 Miget 控制台生成的 **Git Token**

保存即可。

---

## 3️⃣ GitHub Actions 中的 Miget Remote 配置

GitHub Actions 使用以下步骤将代码推送到 Miget：

```yaml
- name: Add Miget remote
  env:
    MIGET_TOKEN_KEY: ${{ secrets.MIGET_TOKEN_KEY }}
  run: |
    git remote add miget \
      https://<Git Username>:${MIGET_TOKEN_KEY}@git.eu-east-1.miget.io/<Miget 自动创建的 Git 仓库路径>
```

### 参数说明

| 项目 | 含义 |
|---|---|
| `migetr0v` | Miget 提供的 **Git Username** |
| `MIGET_TOKEN_KEY` | Miget 生成的 **Git Token** |
| `git.eu-east-1.miget.io` | Miget Git Server（Region） |
| `migetr0v/yrusin-ivrne` | Miget 自动创建的 **Git 仓库路径** |

---

### 如何确定 Git Remote 地址

在 **生成 Git Token** 的页面上方有一个 **Deployment** 区域，  
右下角会显示完整的 **操作流程**。

👉 **将该流程全部复制给 AI 即可**

在 GitHub Actions 的工作流中，重点关注 `run: |` 里的这一行：

```bash
git remote add miget \
https://migetr0v:${MIGET_TOKEN_KEY}@git.eu-east-1.miget.io/migetr0v/yrusin-ivrne

```

### ⚠️ 注意事项

- `migetr0v` 和 `yrusin-ivrne` **必须与 Miget 控制台显示的一致**
- 该地址 **不是 App 的访问 URL**
- 这是 **Git 推送地址（Git Remote）**

---

## 4️⃣ 部署流程说明

1. 推送代码到 GitHub 的 `main` 分支  
2. GitHub Actions 自动触发  
3. Actions 将代码 `git push` 到 Miget  
4. Miget 自动构建并部署应用  
5. 访问 Miget 提供的应用地址 🎉

---

## 5️⃣ 常见问题

### Q: 出现 `repository not found`

可能原因：

- Git Remote 地址填写错误  
- App 未启用 **Git Deploy**  
- 使用了错误的 **Git Username**

---

### Q: GitHub Actions 执行失败

排查方向：

- 检查仓库 Secrets 是否正确设置 `MIGET_TOKEN_KEY`
- 检查 `main` 分支是否存在最新提交

---

## 🙏 感谢

- 老王 nodejs-argo  
  https://github.com/eooce/nodejs-argo
