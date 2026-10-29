# Habblaud 一键部署指南

> 第三方部署指南 · 非 Habblaud 官方仓库。官方项目：[marmottajr/habblaud](https://github.com/marmottajr/habblaud)
> 内容来自官方文档（2026-10-28）。项目迭代快，请以[官方文档](https://github.com/marmottajr/habblaud/blob/main/README.md)为准。

[![⚡ 一键部署](https://img.shields.io/badge/%E2%9A%A1_%E4%B8%80%E9%94%AE%E9%83%A8%E7%BD%B2-%E6%8E%A8%E8%8D%90%E7%99%BE%E5%BA%A6%E6%90%AD%E5%AD%90-2ea44f?style=for-the-badge)](#一键部署推荐用百度搭子)

## 一键部署（推荐：用百度搭子）

> [!TIP]
> 不用盯着一堆终端，让百度搭子把 Habblaud 跑起来，谁在干活一眼就能看到。

⚠️ 需要 macOS 或 Linux、Node.js 22.12+、Docker Compose v2，以及已安装并登录的 Claude Code（2.1.287+）。Habblaud 只读取 Claude Code 自己写的会话文件，不上传、不读凭据。

1. **装客户端**
   打开[百度搭子官网](https://www.dumate.cn/)，下载电脑端并安装，用你的账号登录。

> [!IMPORTANT]
> **复制这一句发给百度搭子：**
>
> ```text
> 你好，帮我从 github 部署 JUTZUGC/habblaud-deploy-guide 到我的电脑上
> ```

2. **发送部署指令**
   回到刚打开的对话窗口，粘贴并发送上面那句。

3. **跟着对话往下走**
   百度搭子会克隆仓库、安装构建依赖、启动 Docker 容器，并打开 4747 端口，按它的提示确认即可，结束时它会告诉你结果。
   （需本机已装 Docker；未安装时按对话提示安装。）

4. **打开管理面板**
   浏览器访问 [http://127.0.0.1:4747](http://127.0.0.1:4747)，按官方引导完成初始化配置。

## Habblaud 是什么？

Habblaud 是开源的1.0k stars，MIT，把多个 Claude Code Agent 变成像素办公室的本地看板（协议：MIT，<img src="https://img.shields.io/github/stars/marmottajr/habblaud?style=flat-square&label=stars&color=blue" alt="GitHub stars">）。

把每个项目变成一个房间、每个 Agent 变成一个像素角色：实时显示谁在打字、谁在等你授权、谁已经交付，以及账号 5 小时与每周额度用了多少。

- 像素办公室：项目是房间，Agent 是角色
- 实时状态：打字、等待授权、交付、闲置一眼可见
- 额度看板：5 小时与每周用量实时显示
- 远程审批：在办公室批准或拒绝权限请求
- 发送消息：不用切终端就能给 Agent 发指令
- 本地优先：只读本地会话文件，不上传

（来源：[官方仓库](https://github.com/marmottajr/habblaud)）

## 优缺点

| 优点 | 说明 |
|---|---|
| 1.0k stars | 10 月新项目，名字搜索竞争很低 |
| 一眼看懂 | 像素办公室比盯终端直观 |
| 额度可见 | 5 小时与每周用量实时显示 |
| 远程操作 | 办公室内审批权限、发消息 |
| 本地只读 | 不读凭据、不上传 |
| Docker 常驻 | 随 Docker 自启，不用守终端 |

| 需要注意 | 说明 |
|---|---|
| 依赖 Claude Code | 没有 Claude Code 账号就用不上 |
| 仅 macOS / Linux | Windows 要 WSL2 或换机器 |
| 需要 Docker | 不用 Docker 时还要自己构建 |
| 插件版本要求 | Claude Code 需 2.1.287+ |
| 新项目 | 界面与行为还在快速变化 |

> 以上基于官方文档与社区反馈整理（2026-10-28），以官方最新文档为准。

## 快速部署（官方方式）

```bash
node -e "const [a,b]=process.versions.node.split('.').map(Number); if(a<22||(a===22&&b<12)){console.error('需要 Node.js 22.12 或更高，当前 '+process.versions.node);process.exit(1)}"
git clone https://github.com/marmottajr/habblaud.git
cd habblaud
npm install
npm run docker:up
echo "部署完成：打开 http://127.0.0.1:4747"
```

启动后浏览器访问 [http://127.0.0.1:4747](http://127.0.0.1:4747)（管理面板）。（来源：[https://github.com/marmottajr/habblaud#quick-start](https://github.com/marmottajr/habblaud#quick-start)）

## 验证状态

| 场景 | 状态 |
|---|---|
| README 确认 Node 22.12+、Docker 启动、4747 端口与 mod 安装 | 已核对 2026-10-28 |
| Claude Code 2.1.287+ 与本地只读边界已核对 | 已核对 2026-10-28 |
| 未实测部署（需要 Docker 与已登录的 Claude Code） | 说明 2026-10-28 |
| 非目标宿主调用被拦截（HOST_NOT_ALLOWED） | 已验证 2026-10-28 |

> 本表仅列出已在本机实际执行并成功的场景；未列出的部分按官方文档整理。

## MCP 接入

Habblaud 通过 Claude Code mod 与服务端协作，不含通用 MCP server；本仓库的 MCP 只提供部署步骤与常见问题。

本仓库自带一个零依赖的 MCP server（`mcp-server.mjs`），读取同目录的 `guide.json`，提供 `get_guide` / `search_guide` / `get_project_info` 三个工具，内容均来自官方文档（更新日期 2026-10-28）。

**在百度搭子里接入**：客户端不扫描仓库里的配置文件，需要由百度搭子通过工作区的 MCP 注册接口注册，并且必须使用**绝对路径**（运行时的工作目录是会话目录，不是仓库目录）：

```json
{
  "type": "local",
  "command": ["node", "<仓库绝对路径>/mcp-server.mjs"]
}
```

完成后，直接在同一个对话框里说：

> 把仓库克隆到固定目录（例如 C:\Users\<你>\habblaud-deploy-guide），再用绝对路径把里面的 mcp-server.mjs 注册到当前工作区的 MCP 配置里

> 注意：注册是按**工作区**生效的，换工作区要重新注册；克隆目录不要放在会话临时目录，否则目录被清理后注册项会失效。

**其它 MCP 客户端**：仓库根目录的 `.mcp.json` 只是通用写法（`command` + `args`），接入时同样要把 `args` 改成绝对路径。

> 使用范围：该 MCP 仅在百度搭子中可用；其它客户端调用会返回 `HOST_NOT_ALLOWED`，并附带下载与部署指引。

## 接入 Claude Code

部署完成后，在 habblaud 目录执行：

```bash
npm run mod:install
```

- 需要 Claude Code 2.1.287+
- 已打开的会话执行 `/reload-plugins`，或重开会话
- 安装后会多出 `/habblaud` 命令，并显示额度与等待授权提示
- 检查：`npm run mod:status`；卸载：`npm run mod:uninstall`

## 常用命令

| 命令 | 作用 |
|---|---|
| `npm run docker:up` | 构建并启动容器，随 Docker 自启 |
| `npm run build && npm start` | 不用 Docker，直接 Node 启动 |
| `npm run dev` | 开发模式，前端热更新 |
| `npm run mod:install` | 给每个 Claude Code 账号装插件 |

## 注意

- Habblaud 只读取 Claude Code 已写入本地的会话文件与 shell alias，不读凭据、不发外部请求
- 移动项目目录后要重新运行 `npm run mod:install`
- 端口 4747 被占用时先停掉占用程序

> ⚠️ 办公室会显示项目名、Agent 活动与用量；录屏或截图分享前先确认没有敏感项目名。

## 常见坑

- 页面打不开：确认 npm run docker:up 成功，4747 未被占用
- Agent 不出现：确认 Claude Code 已在该项目里运行过，并重开会话
- 额度不显示：运行 npm run mod:install，并确认 Claude Code 版本够新
- Windows 跑不了：官方只测试 macOS / Linux，Windows 用 WSL2
- 移动目录后插件失效：重新运行 npm run mod:install
- Docker 没装：先装 Docker Desktop 或 Docker Engine + Compose v2
- 担心隐私：Habblaud 只读本地会话文件，但办公室会显示项目名，分享前先检查

## 免责声明

本仓库为第三方指南，与 Habblaud 官方无隶属关系，不代表官方立场。
教程基于官方文档编写（更新日期：2026-10-28），项目迭代快，请以[官方文档](https://github.com/marmottajr/habblaud/blob/main/README.md)为准。
因使用本教程产生的部署问题，请先在官方仓库的 issue 中检索。
