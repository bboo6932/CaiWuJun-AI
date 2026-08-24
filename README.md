# 蔡武君 AI

一个 macOS 桌面 AI 伙伴原型。它使用低多边形小男孩形象作为桌面角色，支持文本输入、麦克风语音输入，并通过 OpenAI Responses API 生成真实 AI 回复。

同一 Swift 包还包含独立的 `Falcon AI` 股票研究主控驾驶舱。两者共用可靠的本地 Codex/对话基础设施，但人格、记忆目录和职责完全分离。

## Falcon AI 驾驶舱

- 应用：`/Users/bobo/Applications/Falcon AI.app`
- 数据：只读取 Master Brain 的 `current_brain_snapshot.json`
- 记忆：`~/Library/Application Support/FalconAI/conversations.json`
- 安全：PAPER_ONLY；无券商、无真实下单；fixture 数据不能晋级候选
- 界面：数据健康门、13 节点时间线、研究优先级、正式候选、Google 外部反证状态、Codex 主控对话

重新构建：

```bash
swift run CaiWuJunCoreCheck
./scripts/build-falcon-app.sh
```

## 已实现

- 可双击打开的 macOS `.app`
- 蔡武君角色主界面
- 根据时间自动显示开场白
- 文本输入与发送
- 麦克风按钮：点一次开始语音输入，再点一次暂停并发送
- 多个独立对话窗口
- 每个窗口有独立本地上下文记忆
- 删除窗口时会删除对应上下文记忆
- 教学模式：适合学生提问、讲题、拆概念
- AI 回复后会用系统中文语音朗读
- 朗读时有嘴巴开合动画
- 每 3 秒自动眨眼
- 回复内容触发开心、惊讶、生气、伤心等表情状态
- OpenAI API Key 设置入口
- AI 后端可选：豆包火山方舟、OpenAI、自定义/本地 OpenAI 兼容接口
- 默认 AI 后端：本机 Codex CLI，不需要 API Key
- 蔡武君人格提示词：喜欢游戏、编程、拍照，活泼但有点内向，偶尔中二
- 没有 API Key 时明确提示需要配置，不使用假 AI 回复
- 已生成一张表情素材表：`Assets/CaiWuJunExpressionSheet.png`

## 打开应用

已经生成好的应用在：

```text
build/蔡武君 AI.app
```

在 Finder 里双击它即可打开。

## 配置 AI 核心

默认情况下，蔡武君会使用本机 Codex CLI：

```text
/opt/homebrew/bin/codex
```

这个模式使用你电脑上已经登录的 Codex，不需要 API Key，也不需要点设置按钮。

如果要切换后端，打开应用后点击顶部 `AI` 按钮，选择 AI 后端并填入 API Key，然后保存。

默认后端是豆包火山方舟：

```text
https://ark.cn-beijing.volces.com/api/v3
doubao-seed-2-0-lite-260215
```

也可以切换到 OpenAI：

```text
https://api.openai.com/v1
gpt-5-mini
```

还可以填自定义或本地 OpenAI 兼容接口，例如 LM Studio 常见地址：

```text
http://127.0.0.1:1234/v1
```

也可以在启动应用前设置环境变量：

```bash
export ARK_API_KEY="你的豆包火山方舟 key"
# 或
export OPENAI_API_KEY="你的 OpenAI key"
```

说明：电脑里的 `Doubao.app` 和 `ChatGPT.app` 是桌面客户端，不是本地模型 API 服务。蔡武君不能稳定地“调用它们的窗口”来获得回答，所以默认用本机 Codex CLI，其他后端则使用官方/兼容 API 接口。

## 语音输入

点击麦克风按钮开始听你说话。第一次使用时 macOS 会请求麦克风和语音识别权限。

再次点击麦克风按钮会停止语音输入，并把识别出的文字发送给蔡武君。

## 本地上下文记忆

点击顶部的窗口列表按钮可以查看所有对话窗口。点击新窗口按钮会创建一个独立窗口记忆，并显示“正在重启，尝试重启...”的启动提示。

每个窗口的聊天内容会保存在本机：

```text
~/Library/Application Support/CaiWuJunAI/conversations.json
```

选择某个窗口时，蔡武君只读取那个窗口的上下文。删除窗口时，对应记忆也会被删除。

## 教学模式

点击顶部的毕业帽按钮可以开启或关闭教学模式。开启后，蔡武君会更像导师一样解释概念、拆步骤、举例和给练习。

## 重新构建

```bash
swift run CaiWuJunCoreCheck
./scripts/build-app.sh
```

构建完成后，新的应用仍会输出到：

```text
build/蔡武君 AI.app
```
