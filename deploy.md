# 部署到 GitHub Pages(境外托管,免备案)

目标:在 ICP 备案完成前,用 GitHub Pages 把 `lanling.vip` 这个域名先撑起来,满足苹果开发者账号申请的"官网"要求。

## 前置准备
- GitHub 账号(免费)
- 已购域名 `lanling.vip`(阿里云控制台可管理 DNS)

## 步骤一:把代码推到 GitHub

### 方式 A:网页拖拽(零命令行)
1. 登录 https://github.com → 右上 `+` → `New repository`
2. 仓库名建议:`lanling-website`,选 **Public**,不勾 README
3. 创建后点 `uploading an existing file`
4. 把 `/Users/huangjiajun/Desktop/lanjing-website/` 目录下所有文件拖进去
5. 底部 `Commit changes`

### 方式 B:命令行
```bash
cd /Users/huangjiajun/Desktop/lanjing-website
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/<你的用户名>/lanling-website.git
git push -u origin main
```

## 步骤二:启用 GitHub Pages
1. 仓库 → `Settings` → 左侧 `Pages`
2. `Source` 选 **Deploy from a branch**
3. `Branch` 选 `main`,文件夹 `/ (root)` → `Save`
4. 等 1–2 分钟,刷新看到 `Your site is live at https://<你的用户名>.github.io/lanling-website/`
5. 打开 URL 确认能访问。**这个 URL 就可以填给苹果开发者注册系统当作"官网"了。**

## 步骤三:绑定自定义域名 lanling.vip(可选,推荐)
苹果其实只看域名能否打开,所以是否绑定 `lanling.vip` 都可以。但绑定后更专业,且后续直接复用。

注意:`.vip` 域名解析到境外 IP **不需要备案**,但如果以后想把站点搬回阿里云大陆服务器,**那时**才需要备案。

### 在 GitHub 这边
1. 仓库 → `Settings` → `Pages` → `Custom domain` 填 `lanling.vip` → `Save`
2. 仓库根目录会自动生成一个 `CNAME` 文件,内容是 `lanling.vip`
3. 勾选 `Enforce HTTPS`(等 DNS 生效后才能勾)

### 在阿里云 DNS 这边
阿里云控制台 → 云解析 DNS → `lanling.vip` → 解析设置,添加 4 条 A 记录(指向 GitHub Pages):

| 记录类型 | 主机记录 | 记录值 |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | `<你的用户名>.github.io` |

DNS 通常 10 分钟–1 小时生效,可以用 https://www.whatsmydns.net 查传播状态。

## 步骤四:正式上线前的检查清单

把项目里所有占位文字替换成真实内容,主要是:

| 当前占位 | 替换成 |
|---|---|
| `蓝领智工` | 你的真实公司名 |
| `办公地址:待补充` | 真实办公地址 |
| `ICP 备案号:待办理` | 备案通过后填入,如 `沪ICP备2026000000号` |
| `公安备案号:待办理` | 公安备案通过后填入 |

`ai@lanling.vip` 已经填好,无需替换。

VSCode / 文本编辑器都有"在文件中查找替换"功能,一次性改完。

## 之后:备案通过怎么迁回国内

ICP 备案下来后,如果你想用阿里云大陆服务器(国内访问更快):
1. 阿里云买轻量应用服务器,装 Nginx
2. 把这个目录传到服务器 `/var/www/html/`(scp 或 FTP)
3. 阿里云 DNS 把 `lanling.vip` 的 A 记录改成服务器 IP(替换上面的 GitHub Pages IP)
4. GitHub Pages 的 Custom domain 设置可以清空(或者保留,作为容灾备份)
5. 网站底部加上 ICP 备案号链接

或者也可以继续用 GitHub Pages,但备案通过后 `.vip` 域名绑定 GitHub Pages 在国内的访问速度还是会有点慢,长期建议迁回大陆。
