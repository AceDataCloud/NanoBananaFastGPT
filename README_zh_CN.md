# Nano Banana 图像 · Ace Data Cloud for FastGPT

这是 **Ace Data Cloud** 维护的独立 Nano Banana 集成，使用你自己的 Ace 应用 API Token；不是 Nano Banana 或 FastGPT 官方插件。

[English guide](README.md) · [当前价格](https://platform.acedata.cloud/models?utm_source=opensource&utm_medium=referral&utm_campaign=evergreen&utm_content=nanobananafastgpt_readme_pricing)

本插件把生成操作与单项／批量任务查询分开。生成只提交一次；之后只查询原 taskId。返回 pending 或工作流显示成功，都不代表媒体文件已完成。

## 从零开始

[FastGPT 官方插件市场](https://cloud.fastgpt.io/config/tool/marketplace) · [本插件官方收录审核](https://github.com/labring/fastgpt-community-plugins/pull/87)。当前候选尚未上架，审核链接不是安装按钮。

新用户先注册并登录 Ace；Applications 中若没有 General application，先点 **Initialize General Application**。这里创建的是 Ace API Token，不是 FastGPT 登录凭据。

1. 使用 FastGPT 4.15 或更新版本。当前尚未官方上架；批准后按服务名搜索并核对作者 **Ace Data Cloud**。现有安装包与校验文件在 [GitHub 正式版本](https://github.com/AceDataCloud/NanoBananaFastGPT/releases/tag/v0.2.2)。系统管理员可在插件管理上传；**团队管理员或所有者**在系统已开启“团队上传插件”时，也可从 **工具 → 添加插件 → 上传插件** 上传 .pkg，并在解析后确认安装。该开关也控制团队 Marketplace 安装，普通成员不能安装。团队与系统插件密钥独立。

   依据为 [FastGPT 当前团队安装文档](https://github.com/labring/FastGPT/blob/main/document/content/plugin/team-installation.mdx)，读取日期2026-10-19。具体Cloud工作区能否使用，需核当前版本、套餐/功能开关和现有角色；缺按钮不等于需要另注册账号，应先问现有管理员。团队/本地上传不代表官方市场公开收录。

   [官方维护者新反馈](https://github.com/labring/FastGPT/pull/8017#issuecomment-6257574098)要求第三方提供商先单独申请合作。GitHub包公开不代表该准入已批准。
2. 登录 [Ace Data Cloud 应用管理](https://platform.acedata.cloud/console/applications?utm_source=opensource&utm_medium=referral&utm_campaign=evergreen&utm_content=nanobananafastgpt_readme_api_key)，打开 **General Application**，确认 Nano Banana 权限、余额和当期价格。点击下图 **1** 复制密钥，或选择 **2 Manage Keys → Create** 新建 FastGPT 专用密钥。若开启 Allowed APIs，同时允许 /nano-banana/images 与 /nano-banana/tasks。在 FastGPT 插件配置的 **Ace Data Cloud API key** 只填令牌本身，不加 Bearer 或引号。

![Ace Data Cloud 的真实应用密钥界面，密钥已遮挡](assets/get-api-key-en.png)

创建专用 Token 时点 **Manage Keys → Create**，名称可填 `FastGPT Nano Banana`，按需设置有效期与 Allowed APIs，再创建并复制。允许上文的创建和任务查询接口。

![Ace Console 创建专用凭据真实界面](assets/create-api-key-en.png)

此图复用 [Suno Dify 源教程](https://github.com/AceDataCloud/SunoDify/blob/8d3d03570e73324220f111f7fe343da0792a8071/README.md) 的真实 Ace Console 截图（2026-10-08/09）。图中 `Dify tutorial` 是示例名称；请改用自己的 FastGPT 名称，图中没有真实密钥。

3. 在 **Studio → Create Agent → Workflow** 新建工作流。从 **System Tools** 添加 **Nano Banana by Ace Data Cloud / Generate Nano Banana image** 和 **Retrieve Nano Banana task**，激活两个工具并选择已配置的 **System secret**。连接 **Process starts → Generate Nano Banana image → Retrieve Nano Banana task → Basic / Assigned Reply**。生成工具按下表填写，其余留空：

| 字段 | 首次运行值 |
|---|---|
| Model | nano-banana-2-lite |
| Resolution | 1K |
| Aspect Ratio | 1:1 |
| Count | 1 |

**Prompt**：A single teal ceramic cube on a plain cream background, studio product photograph, no text.

4. 查询工具的 **Task ID** 选择 **Variable Reference → Generate Nano Banana image → Task ID**。在 **Assigned Reply** 中用变量选择器插入查询工具的 status、success、taskId、mediaUrls 和 costCredits。选择 **Save Only**，再用 **Run Preview** 执行一次。若结果为 pending，保存 taskId；之后新建 **Process starts → Retrieve Nano Banana task → Basic / Assigned Reply** 工作流，只填同一个 Task ID 查询。重跑生成工作流会再次提交并计费。
5. status=succeeded 且 success=true 后打开 mediaUrls。用 taskId 在 Ace Data Cloud 请求记录核对调用与 Credits；实际套餐换算与价格以当期为准。

以下 FastGPT 截图展示插件安装，以及**通过 FastGPT 只读查询此前已生成的任务**；并非新发起的 FastGPT 生成。

![FastGPT 插件状态 Normal，系统密钥已配置](assets/fastgpt-installed-configured.png)

![FastGPT 查询此前生成任务的工作流](assets/fastgpt-lookup-workflow.png)

![FastGPT Run Preview 返回此前生成的媒体](assets/fastgpt-lookup-result.png)

编辑时选择 **Edit Nano Banana image**，填入一张经授权且可访问的 HTTPS 参考图片 URL。编辑会单独提交并可能计费；把返回的 Task ID 接到同一个 **Retrieve Nano Banana task** 查询工具。

多个已有任务可用 **Retrieve Nano Banana tasks** 输入 1–20 个 Task ID，一次只读查询。逐项检查状态；只有全部成功时批次才算成功。此工具不会提交新生成。

如果有请求的任务未返回，或返回的任务 ID 无法对应，批量结果会保持待完成。请核对已有任务 ID，不要重新提交生成。

## 无密钥示例

[examples/generate.json](examples/generate.json) 不含密钥。把真实密钥写入本地 .secrets.local.json，内容为 {"apiKey":"YOUR_OWN_KEY"}，然后运行：

~~~sh
pnpm install
pnpm exec fastgpt-plugin debug . --run --tool generate --input-file examples/generate.json --secrets-file .secrets.local.json
pnpm exec fastgpt-plugin build --entry . --output ./dist
pnpm exec fastgpt-plugin check --entry . --output ./dist
pnpm exec fastgpt-plugin pack --entry . --dist ./dist --output ./out
~~~

若 pending，另建本地 examples/retrieve.local.json，内容为 {"taskId":"返回的任务 ID"}；只运行：

~~~sh
pnpm exec fastgpt-plugin debug . --run --tool retrieveTask --input-file examples/retrieve.local.json --secrets-file .secrets.local.json
~~~

本地密钥文件已由 Git 忽略。401/403 检查密钥、权限与余额；400 核对参数；429 降低并发；超时、5xx 或任务失败先查原任务和请求记录，不要自动重试付费生成。客服只需 taskId 或 traceId，不需要密钥。

插件只向 api.acedata.cloud 发送所选输入和令牌。[隐私说明](PRIVACY.md) · [源码与问题反馈](https://github.com/AceDataCloud/NanoBananaFastGPT/issues)。

## 截图范围

FastGPT 截图为 2026-10-09 的自建 4.15.8 实例，显示旧名称和0.1.0插件。它们不证明本次0.2.2新名称、普通市场安装或当前版本真实调用。
