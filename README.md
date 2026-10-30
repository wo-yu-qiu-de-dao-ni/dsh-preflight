# dsh-preflight（前置技能 v1.3.0）

把"前置技能"的交付质量三段规则注入 DSH agent 的 system prompt：
**收单拆义务定验收标准 → 四层输出纪律 → 交付前自检**。所有产出文档类任务自动按规则执行。

零依赖、零联网、零上传。指令文本随插件分发，本机本地生效。

## 安装（CLI 用户）

```sh
dsh plugin --profile web add ./dsh-preflight-1.3.0.tgz
dsh --profile web        # 重启/启动
```

验证：

```sh
dsh --profile web --dump-config
# 应出现 # == dsh-preflight 层，条目 id: preflight, config.level: full
```

卸载：`dsh plugin --profile web remove dsh-preflight`

## 安装（桌面版用户）

桌面版没有本地包安装入口，两条路：

1. **发布到 npm 后**在桌面版插件窗口填包名 `dsh-preflight` 安装（需要发布权限）；
2. **等价替代**：直接把安装说明里的"极简版"贴进工作区 `AGENTS.md`（DSH 官方 agent-instructions 插件会自动加载，效果与本插件一致，只是 DSH 负责发现）。

## 配置

```yaml
# 想省 token 可改成极简版（覆盖到 profile 的 cordis.patch.yml，需整行重写）：
- id: preflight
  name: dsh-preflight
  config:
    level: mini
```

## 与 AGENTS.md 的关系

本插件注入的是**全局规则**（所有会话生效）；`AGENTS.md` 是**工作区规则**（按 cwd 发现，DSH 官方机制）。两者可并存，内容相同则重复——二选一即可。

## 反馈

规则内容以 `../前置技能_v1.3.0.md` 为准（本插件内置文本是其命令式转写）。规则有改动时：更新 `lib/index.js` 里的 PREFLIGHT_* 文本 → 升版本号 → 重新 `npm pack`。
