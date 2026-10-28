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

文件名不代表所有同名客户端或内核都支持其中全部功能。三份 YAML 含 Snell v5 节点模板，使用前确认内核支持；不使用该节点时，需同时移除节点及策略组中的引用。

## 首次导入

### Surge

1. 使用上方 `surge.conf` 地址导入配置。
2. 将 Airport 行 `policy-path=` 后的示例地址替换为自己的 **Surge 格式订阅**。原地址指向本仓库的示例节点，仅供展示。
3. 将订阅域名规则中的 `example.com` 改成真实订阅域名。Zus、Zjp 是独立示例节点，可修改或删除；删除时也从 `Proxy🪁` 移除引用。
4. 选择规则模式、可用策略和节点，再启动连接。Airport 默认隐藏，需要显示时把 `hidden=1` 改成 `hidden=0`。

### Shadowrocket

1. 在 App 首页添加自己的订阅或节点。
2. 在“配置”页通过上方 `shadowrocket.conf` 地址下载，并选择“使用配置”。
3. 首页“全局路由”选择“配置”，检查各策略组，再启动连接；首次使用按系统提示授予 VPN 权限。
4. 地区组按节点名称筛选。节点名需含香港/HK、新加坡/SG、美国/US、台湾/TW 等关键词；空组可改选“全部节点”或 `PROXY`。`PROXY` 指首页当前选中的节点。

配置与节点订阅分开管理，公开文件的 `[Proxy]` 段不包含节点凭据。当前配置未设置 MITM hostname，无需为此安装 HTTPS 解密证书。

### Clash / Clash Mi / Clash Verge

1. 导入对应 YAML，将 Airport 的 `url` 示例地址换成自己的 **Clash 格式订阅**。
2. 使用 Snell5 时，将 `YOUR_SNELL_PSK` 换成真实密钥，并核对服务器与端口。
3. 使用规则模式，检查主代理组和业务组。Clash 版本保留地区组，Mi / Verge 版本通过主代理组统一选择节点。
4. 按需启用 TUN；关闭 TUN 时，开启系统代理或在应用中手动设置代理。客户端覆写可能改变 YAML 的值，以最终运行配置为准。

## 五份配置的默认设置与差异

以下描述仓库文件中明确写入的设置。**未配置不等于明确关闭**；Surge、Shadowrocket 的 VPN 接管也不能直接等同于 Clash 的 `tun.enable`。

### 接管、DNS 与网络

| 项目 | Surge | Shadowrocket | Clash | Clash Mi | Clash Verge |
| --- | --- | --- | --- | --- | --- |
| TUN / VPN | 由 App 与平台控制 | 由 App 的 VPN 连接开关控制 | 未配置 TUN | 默认开启 | 默认关闭 |
| 接管预设 | 全网络、局域网包含选项关闭；设置内网绕过 | 设置内网及保留网段旁路 | 无 TUN 预设 | mixed 栈、自动路由、自动检测出口 | 与 Mi 相同，启用 TUN 后生效 |
| DNS | 国内 DoH + 普通 DNS | 国内 DoH + 普通 DNS，失败回退系统 | Fake-IP + 国内 DoH | 同 Clash，增加 DNS 附加选项 | 与 Mi 相同 |
| DNS 接管目标 | Google DNS 的 53 端口 | 与 Surge 相同 | 未配置 TUN DNS 接管 | UDP / TCP 53 端口 | 同 Mi，TUN 关闭时不生效 |
| 域名嗅探 | 无 Mihomo 式 sniffer 配置 | 无 Mihomo 式 sniffer 配置 | 未配置 sniffer | 开启 HTTP / TLS / QUIC 嗅探 | 与 Mi 相同 |
| IPv6 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 |
| QUIC 阻断 | 代理方向阻断 | 代理方向阻断 | 未显式配置同等阻断项 | 与 Clash 相同 | 与 Clash 相同 |
| 代理入口 | HTTP 6152、SOCKS 6153；共享关闭 | 未显式设置监听端口 | 混合端口 7890；局域网访问关闭 | 与 Clash 相同 | 与 Clash 相同 |

### 节点与分流策略

| 项目 | Surge | Shadowrocket | Clash | Clash Mi | Clash Verge |
| --- | --- | --- | --- | --- | --- |
| 地区组 | 美国/新加坡手选；香港/台湾 fallback | 美国/新加坡测速；香港负载均衡；台湾 fallback | 与 Surge 相同 | 无地区组 | 无地区组 |
| 主代理候选 | 地区组、Airport、Zus/Zjp 示例节点 | 地区组、全部节点、PROXY；默认香港 | Snell5、地区组、订阅节点 | Snell5、订阅节点 | 与 Mi 相同 |
| 业务组 | 保留各业务的地区候选 | 保留地区候选，明确默认地区 | 与 Surge 的业务候选一致 | 统一引用主代理组 | 与 Mi 相同 |
| Apple-AI | 主代理、美国、DIRECT | 同 Surge，默认主代理 | 同 Surge | 主代理、DIRECT | 与 Mi 相同 |
| 普通 Apple | 引用 apple.list，Apple 组直连优先 | 仅部分 Apple 域名显式指定 Apple，默认直连 | 引用 apple.list，Apple 组直连优先 | 与 Clash 相同 | 与 Clash 相同 |
| 广告 | 有 AdBlock 组，未引用广告规则集 | 引用广告规则集，默认 REJECT | 引用广告规则集，REJECT 优先 | 与 Clash 相同 | 与 Clash 相同 |
| 兜底 | Final，可选主代理或 DIRECT | 兜底策略，默认主代理 | Final，可选主代理或 DIRECT | 与 Clash 相同 | 与 Clash 相同 |

**Mi 与 Verge 的有效配置仅 `tun.enable` 不同**，文件头注释也不同。Clash 与它们还存在地区组、嗅探和 DNS 设置上的差异。

<details>
<summary>展开：各客户端的具体参数与策略说明</summary>

### Surge

- Airport 每 86400 秒更新；香港、台湾组每 600 秒检查可用性，fallback 按节点优先级选择可用节点。
- `include-all-networks = false`、`include-local-networks = false`；设置 `skip-proxy`。HTTP/SOCKS 监听地址为 `0.0.0.0`，但 Wi-Fi、热点共享开关均关闭。本地控制 API 默认注释关闭，日志为 `notify`。
- DNS 使用阿里/腾讯 DoH 与普通 DNS，开启 `read-etc-hosts`；`always-real-ip` 为指定服务保留真实 IP。`hijack-dns` 指向 `8.8.8.8:53`、`8.8.4.4:53`。
- Apple-AI 在 AI、普通 Apple 规则之前。内网检查分两层：开头 `RULE-SET,LAN,DIRECT,no-resolve` 避免为内网判断提前解析所有域名；业务规则后再用 `RULE-SET,LAN,DIRECT` 识别解析为内网 IP 的地址。需优先直连的自定义内网域名可在开头显式添加。
- `block-quic = all-proxy` 影响代理方向的 QUIC，DIRECT 不受此设置影响；不支持 UDP 的节点策略设为 REJECT。

### Shadowrocket

- 地区组检查间隔为 600 秒。AI、Netflix、Twitter、TikTok 默认美国；Spotify 默认新加坡；Telegram、YouTube、YouTube-Music 默认香港。Apple-AI 默认主代理，Apple 默认 DIRECT。
- 使用 `skip-proxy`、`tun-excluded-routes` 旁路内网和保留地址。`hijack-dns` 的 Google DNS 目标与 Surge 相同；`use-local-host-item-for-proxy = true`。
- 内网、GitHub 规则后，先匹配广告、Apple-AI、AI 及其他业务，再匹配部分 Apple 域名、国内规则和国内 IP，最后兜底。未引用 `rules/apple.list`。
- `block-quic = all-proxy`；不支持 UDP 的节点策略设为 REJECT。没有配置 MITM hostname。

### 三份 Clash 系列 YAML

- 共同设置：`mixed-port: 7890`、`allow-lan: false`、`mode: rule`、日志 `info`、IPv6 关闭；开启 `unified-delay`、`tcp-concurrent`，保存策略选择和 Fake-IP 映射。
- DNS 默认服务器为 `223.5.5.5`、`119.29.29.29`，DoH 为 `https://223.5.5.5/dns-query`、`https://1.12.12.12/dns-query`；三份使用相同的 Fake-IP 排除列表。
- Snell5、Airport 模板、12 份规则集和规则顺序相同。Airport 与规则集每 86400 秒更新，Airport 健康检查间隔为 600 秒。
- Mi / Verge 额外明确开启 DNS 的 `respect-rules`、`use-hosts`、`use-system-hosts`，并用上述两项 DoH 设置 `proxy-server-nameserver`。
- Mi / Verge 的 TUN 预设为 `stack: mixed`、`auto-route: true`、`auto-detect-interface: true`，DNS 接管为 `any:53` 和 `tcp://any:53`。
- Mi / Verge 开启 `parse-pure-ip`、`override-destination`；嗅探 HTTP 80、8080–8880，TLS / QUIC 443、8443，排除 `Mijia Cloud`、`+.push.apple.com`。嗅探辅助识别域名，不是解密 HTTPS，也不等于阻断 QUIC。

</details>

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
| [adblock.list](rules/adblock.list) | 广告规则；Shadowrocket 和三份 YAML 引用 |
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
│   └── subscription.list                  # Surge 示例订阅
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
