本仓库通过 **GitHub Actions** 自动将代码部署到 **Miget** 平台。

---

## 1️⃣ 生成 MIGET_TOKEN_KEY

1. 登录 Miget 控制台:  
   [https://app.miget.com](https://app.miget.com)

2. 进入你的 App：  


3. 打开：

Settings → Git / Deploy Credentials

4. 创建或复制 **Git Token**  
- Token 只会完整显示一次  
- 请妥善保存  

---

## 2️⃣ 在 GitHub 仓库中设置 Secret

进入你的 GitHub 仓库：

Settings → Secrets and variables → Actions → New repository secret

填写：

- **Name**: `MIGET_TOKEN_KEY`  
- **Value**: `你在 Miget 控制台生成的 Git Token`  

保存即可。

---

## 3️⃣ GitHub Actions 中的 Miget Remote 配置

GitHub Actions 使用如下步骤将代码推送到 Miget：

```yaml
- name: Add Miget remote
  env:
    MIGET_TOKEN_KEY: ${{ secrets.MIGET_TOKEN_KEY }}
  run: |
    git remote add miget \
      https://<Git Username>:${MIGET_TOKEN_KEY}@git.eu-east-1.miget.io/<Miget 自动创建的 Git 仓库路径>

参数说明

项目	含义
migetr0v	Miget 提供的 Git Username
MIGET_TOKEN_KEY	Miget 生成的 Git Token
git.eu-east-1.miget.io	Miget Git Server（Region）
migetr0v/yrusin-ivrne	Miget 自动创建的 Git 仓库路径

再生成git-token上方有个Deployment，右下角有操作流程全部复制给Ai，工作流中的run: |
          git remote add miget \
            https://migetr0v:${MIGET_TOKEN_KEY}@git.eu-east-1.miget.io/migetr0v/yrusin-ivrne这一行就知道怎么填写了。
⚠️ 注意：
	•	migetr0v 和 yrusin-ivrne 必须与 Miget 控制台显示的一致
	•	该地址不是 App 访问 URL，而是 Git 推送地址

4️⃣ 部署流程说明
	1.	推送代码到 GitHub main 分支
	2.	GitHub Actions 自动触发
	3.	Actions 将代码 git push 到 Miget
	4.	Miget 构建并部署应用
	5.	访问应用：



5️⃣ 常见问题

Q: 出现 repository not found
	•	Git Remote 地址填写错误
	•	App 未启用 Git Deploy
	•	使用了错误的 Git Username

Q: GitHub Actions 失败
	•	检查仓库 Secrets 是否正确设置 MIGET_TOKEN_KEY
	•	检查 main 分支是否有最新提交

这份 README.md **完全覆盖 GitHub Actions 部署 Miget 所需的全部步骤**，可以直接放仓库用。  

如果你愿意，我可以顺手帮你加一段 **Action