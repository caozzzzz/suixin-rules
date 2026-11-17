# 随心规则 · 多客户端代理配置与规则

随心规则提供 Surge、Shadowrocket、Clash、Clash Mi、Clash Verge 五份配置模板，共享本仓库的远程规则。按客户端选择配置，填入自己的节点或订阅后使用。

**仓库中的示例节点和订阅不能实际连接。真实密钥、订阅 Token 和节点凭据只在本地填写。**

- [配置下载](#配置下载)
- [首次导入](#首次导入)
- [五份配置的默认设置与差异](#五份配置的默认设置与差异)
- [TUN 开启和关闭的区别](#tun-开启和关闭的区别)
- [规则与自动更新](#规则与自动更新)
- [文件结构与维护](#文件结构与维护)

## 配置下载

下列链接均为可供客户端下载的 Raw 地址。

| 客户端 | 配置 | 节点来源 |
| --- | --- | --- |
| Surge | [surge.conf](https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/surge.conf) | 在配置中填写 Surge 格式订阅或节点 |
| Shadowrocket | [shadowrocket.conf](https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/shadowrocket.conf) | 在 App 首页单独添加订阅或节点 |
| Clash | [clash.yaml](https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/clash.yaml) | 在配置中填写 Clash 格式订阅或节点 |
| Clash Mi | [clashmi.yaml](https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/clashmi.yaml) | 同 Clash；默认开启 TUN |
| Clash Verge | [clashverge.yaml](https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/clashverge.yaml) | 同 Clash；默认关闭 TUN |

## 首次导入

### Surge

1. 下载并导入 [surge.conf](https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/surge.conf)。
2. 将 Airport 的 `policy-path=` 示例地址换成自己的 **Surge 格式订阅**。
3. 如需使用 Self，在本地的 `[Proxy]` 添加节点定义，再用节点名称替换 Self 中的 `REJECT`。可在本地 `[Host]` 增加自建节点的固定解析，必要时再加一条节点域名直连规则。
4. 在规则模式下启动，手动选择 Airport 或 Self，再检查 AI 组。

### Shadowrocket

1. 在首页添加订阅或节点，并手动选中要给 Airport 使用的节点。
2. 在「配置」页下载并启用 [shadowrocket.conf](https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/shadowrocket.conf)，把「全局路由」设为「配置」。首次连接按提示授权 VPN。
3. Airport 跟随首页选中的 `PROXY`。需要独立的 Self 出口时，在本地 `[Proxy]` 定义节点，并用节点名称替换 Self 中的 `REJECT`。

### Clash / Clash Mi / Clash Verge

1. 导入对应 YAML，把 `proxy-providers.Airport.url` 换成自己的 **Clash 格式订阅**。
2. 如果使用自建节点，把节点写入顶层 `proxies`，再将名称加入 Self 的 `proxies` 并替换 `REJECT`。节点协议是否可用取决于所选内核。
3. 使用规则模式，检查主代理和 AI 的选择。关闭 TUN 时，可使用系统代理或手动指定本地代理入口。

**没有自建节点时，把 AI 从 Self 改成 Airport 或主代理。** 模板中的 Self 使用 `REJECT` 占位，未配置便不会自动走代理或直连；Airport 的示例订阅也无法实际联网。

## 五份配置的默认设置与差异

五份都使用手动选择：`Proxy🪁 → Airport / Self`，不再按地区分组或自动切换。AI 优先 Self；Netflix、Telegram、Twitter、TikTok、Spotify、YouTube、YouTube Music 跟随主代理，也可单独选择 Airport 或 Self。Apple-AI 在 AI 和普通 Apple 之前匹配；普通 Apple 的首选为直连。

规则顺序为内网 → GitHub → 业务 → 国内 → 兜底。国内规则直连，`GEOIP,CN` 允许解析域名再判断 IP；未匹配流量交给「兜底」，首选主代理。五份均保留 AdBlock 策略组供手动启用，目前未引用广告规则集。

| 项目 | Surge | Shadowrocket | Clash | Clash Mi | Clash Verge |
| --- | --- | --- | --- | --- | --- |
| Airport | Surge 订阅 | 首页已选节点 | Clash 订阅 | 同 Clash | 同 Clash |
| TUN / VPN | 由 App 和平台管理 | 由 App 管理 | 无 TUN 预设 | 默认开启 | 默认关闭 |
| DNS | 国内 DoH + 普通 DNS；接管进入 Surge 范围的传统 53 端口请求 | 国内 DoH + 普通 DNS；系统 DNS 回退，保留对两个 Google DNS 地址的接管 | Fake-IP + 国内 DoH | 同 Clash，启用 TUN 时接管 UDP/TCP 53 | 同 Mi，启用 TUN 后生效 |
| 域名嗅探 | 不用 Mihomo 的配置项 | 不用 Mihomo 的配置项 | 未配置 | HTTP / TLS / QUIC | 同 Mi |
| QUIC | 仅阻断代理方向 | 仅阻断代理方向 | 未设置同等阻断 | 同 Clash | 同 Clash |
| 本地代理入口 | 共享端口 6152 / 6153，Wi-Fi 和热点共享关闭 | 未显式指定监听端口 | 混合端口 7890，局域网访问关闭 | 同 Clash | 同 Clash |

五份均关闭 IPv6。三份 YAML 的 DoH 不跟随代理规则，减少节点域名解析对代理的依赖；Fake-IP 排除本地域名、强制门户及指定的时间同步域名等。Mi 与 Verge 的有效配置仅 `tun.enable` 不同。实际运行状态仍取决于客户端及其覆写设置。

## TUN 开启和关闭的区别

| 状态 | 接管方式 | 使用时的区别 |
| --- | --- | --- |
| 开启 | 通过虚拟网卡与路由将流量交给内核 | 可覆盖很多不遵循系统代理的软件，例如部分游戏、命令行工具；TCP / UDP 代理能力仍取决于内核与节点 |
| 关闭 | 通过系统代理或应用手动指定的代理入口 | 忽略代理设置的软件可能直接联网；普通 HTTP 系统代理无法全面接管 UDP，SOCKS UDP 还需应用与节点支持 |

**开启 TUN 不等于所有连接走代理。** 进入内核的流量仍按规则决定 DIRECT、代理或 REJECT；关闭 TUN 也可以继续使用代理，但未进入代理入口的连接不受这些规则控制。

桌面 TUN 通常需要管理员权限或服务模式，移动端通常需要 VPN 权限。它可能与其他 VPN、虚拟网卡冲突，不保证提速。53 端口 DNS 接管不会自动把应用自行使用的 DoH/DoT 转成内置 DNS 查询。

Mi / Verge 可通过 `tun.enable` 切换；Clash 文件没有完整 TUN 预设。Surge、Shadowrocket 及移动客户端的实际 VPN 接管状态需在 App 中确认。

参考：[Mihomo TUN 文档](https://wiki.metacubex.one/config/inbound/tun/)、[MetaCubeX 对代理入口与 TUN 的说明](https://github.com/MetaCubeX/metacubexd)。本文默认值来自仓库文件的静态检查，未进行各平台实机连通性测试。

## 规则与自动更新

规则集位于 `rules/`，策略名称由各主配置指定。不同客户端引用的规则与匹配顺序有差异，详见上方对比。

| 文件 | 用途 |
| --- | --- |
| [ai.list](rules/ai.list) | AI 合集，包含 ChatGPT、Gemini 等 |
| [apple-ai.list](rules/apple-ai.list) | Apple Intelligence / Private Relay |
| [apple.list](rules/apple.list) | 普通 Apple 服务 |
| [netflix.list](rules/netflix.list) | Netflix |
| [telegram.list](rules/telegram.list) | Telegram |
| [twitter.list](rules/twitter.list) | Twitter / X |
| [tiktok.list](rules/tiktok.list) | TikTok |
| [spotify.list](rules/spotify.list) | Spotify |
| [youtube.list](rules/youtube.list) | YouTube |
| [youtube-music.list](rules/youtube-music.list) | YouTube Music |
| [china.list](rules/china.list) | 中国大陆规则，采用无 IPv6 的上游版本 |
| [adblock.list](rules/adblock.list) | 备用广告规则，主配置默认不引用 |
| [openai.list](rules/openai.list) | 旧 OpenAI 兼容文件，当前主配置不引用 |

GitHub Actions 每天北京时间 **06:20** 同步上游并检查格式，也可在 Actions 手动运行 **Sync Surge rules**。仓库更新规则文件与客户端拉取更新是两个步骤，客户端何时应用以自身更新机制为准。

<details>
<summary>展开：在已有 Surge 配置中引用本仓库规则</summary>

以下是片段，不是完整配置。先在自己的配置中定义 `Proxy🪁`、`Apple-AI`、`AI` 等策略组，再引用；完整顺序以 [surge.conf](surge.conf) 为准。

```ini
[Rule]
DOMAIN-SUFFIX,raw.githubusercontent.com,Proxy🪁
RULE-SET,https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/rules/apple-ai.list,Apple-AI,extended-matching
RULE-SET,https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/rules/ai.list,AI,extended-matching
RULE-SET,https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/rules/apple.list,Apple
RULE-SET,https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/rules/netflix.list,Netflix
RULE-SET,https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/rules/china.list,DIRECT
GEOIP,CN,DIRECT
FINAL,Final,dns-failed
```

</details>

## 文件结构与维护

```text
.
├── README.md
├── surge.conf / shadowrocket.conf          # Surge、Shadowrocket 入口
├── clash.yaml / clashmi.yaml / clashverge.yaml
├── rules/                                 # 共享规则集
├── icons/                                 # 成品图标及旧 URL 兼容文件
│   └── source/                            # 18 枚图标源图片
├── examples/
│   ├── subscription.list                  # Surge 示例订阅
│   └── clash-subscription.yaml            # Clash 示例订阅
├── scripts/
│   ├── generate-icons.py                  # 生成图标
│   ├── requirements-icons.txt             # 固定图标依赖 Pillow==11.1.0
│   └── validate_rules.py                  # 检查规则格式
└── .github/workflows/                      # 自动同步与图标生成
```

主配置保留在根目录；规则、图标、示例和维护工具分别归档。

### 策略组图标

Surge 配置引用本仓库托管的 Essential 图标，成品统一为 256×256 透明 PNG，符号放在 208×208 内容区域内。图标由 ImageGen 设计并从透明图集中拆分为源图片；保留旧版带版本号文件，兼容已有下载地址。例如：

```ini
icon-url=https://raw.githubusercontent.com/caozzzzz/suixin-rules/main/icons/ai.png?v=essential-20261002
```

### 本地维护命令

普通导入配置无需安装 Python 或图标依赖。以下命令仅用于维护者，在仓库根目录运行：

```shell
# 检查规则格式
python scripts/validate_rules.py

# 安装固定依赖并重新生成图标
python -m pip install -r scripts/requirements-icons.txt
python scripts/generate-icons.py
```

`scripts/requirements-icons.txt` 供图标生成使用。图标源图片、生成脚本或依赖变化时自动运行 **Generate policy icons**，也可手动触发；每日规则同步不重新生成图标。
