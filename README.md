# Personal Proxy Configs & Rules

个人代理配置与规则仓库，包含 Surge、Shadowrocket、Clash、Clash Mi、Clash Verge 配置模板，以及共享规则和策略组图标。配置不提供可实际连接的订阅或节点凭据。

## 配置入口

| 客户端 | 配置下载（Raw） | 使用前需要准备 |
| --- | --- | --- |
| Surge | [surge.conf](https://raw.githubusercontent.com/caozzzzz/surge-rules/main/surge.conf) | Surge 格式订阅，或自己的节点 |
| Shadowrocket | [shadowrocket.conf](https://raw.githubusercontent.com/caozzzzz/surge-rules/main/shadowrocket.conf) | 在 App 首页单独添加节点或订阅 |
| Clash | [clash.yaml](https://raw.githubusercontent.com/caozzzzz/surge-rules/main/clash.yaml) | Clash 格式订阅、支持所用协议的内核 |
| Clash Mi | [clashmi.yaml](https://raw.githubusercontent.com/caozzzzz/surge-rules/main/clashmi.yaml) | 同上；文件中默认开启 TUN |
| Clash Verge | [clashverge.yaml](https://raw.githubusercontent.com/caozzzzz/surge-rules/main/clashverge.yaml) | 同上；文件中默认关闭 TUN |

## 文件结构

```text
.
├── README.md                       # 使用说明与默认设置
├── surge.conf                      # Surge 配置入口
├── shadowrocket.conf               # Shadowrocket 配置入口
├── clash.yaml                      # Clash 配置入口
├── clashmi.yaml                     # Clash Mi 配置入口
├── clashverge.yaml                  # Clash Verge 配置入口
├── rules/                          # 共享远程规则集
├── icons/                          # 客户端引用的成品图标与兼容文件
│   └── source/                     # 图标源图片
├── examples/
│   └── subscription.list           # Surge 示例节点订阅（无法连接）
├── scripts/                        # 仓库维护工具
│   ├── generate-icons.py           # 生成图标
│   ├── requirements-icons.txt      # 图标生成依赖（Pillow 固定版本）
│   └── validate_rules.py           # 检查规则格式
└── .github/workflows/              # 自动同步规则、生成图标
```

主配置保留在根目录，便于直接下载；规则、图标、示例、维护工具分别归档。现有配置、规则和图标的下载地址保持不变。`requirements-icons.txt` 已移至 `scripts/`，自动化中的安装命令及触发路径也同步更新。它是维护依赖，普通使用者无需安装；直接删除会使现有图标生成流程缺少依赖清单。

### 规则与资源索引

- `rules/ai.list`：AI 合集，包含 ChatGPT、Gemini 等（自动同步）
- `rules/openai.list`：旧 OpenAI 兼容文件，不再由主配置引用
- `rules/apple.list`：普通 Apple 服务（自动同步）
- `rules/apple-ai.list`：Apple Intelligence / Private Relay（自动同步）
- `rules/spotify.list`：Spotify（自动同步）
- `rules/youtube.list`：YouTube（自动同步）
- `rules/youtube-music.list`：YouTube Music（自动同步）
- `rules/netflix.list`：Netflix（自动同步）
- `rules/telegram.list`：Telegram（自动同步）
- `rules/twitter.list`：Twitter / X（自动同步）
- `rules/tiktok.list`：TikTok（自动同步）
- `rules/china.list`：中国大陆 IPv4 规则（自动同步）
- `rules/adblock.list`：广告规则（自动同步；Shadowrocket 和三份 Clash 配置引用，当前 Surge 主配置未引用）
- `icons/`：Essential 系列策略组图标（256×256、透明背景 PNG）
- `icons/source/`：18 枚独立图标的设计源图片
- `scripts/generate-icons.py`：统一尺寸、留白并生成图标，保留旧版 URL 兼容文件

## 自有图标

策略组图标由本仓库自行托管，不依赖第三方图标仓库。配置使用以下格式引用：

```ini
icon-url=https://raw.githubusercontent.com/caozzzzz/surge-rules/main/icons/ai.png?v=essential-20261002
```

Essential 系列采用无底板的独立符号，保留品牌和地区辨识度。Airport 使用客机剪影，Proxy 使用蓝紫交错流线表达流量转发。图标设计由 ImageGen 制作，再从透明图集中拆分为独立源图片。

在本地运行 `python -m pip install -r scripts/requirements-icons.txt` 安装固定版本依赖后运行 `python scripts/generate-icons.py`，可以从 `icons/source/` 重新生成整套图标。所有图标统一输出为 256×256 RGBA PNG，符号置于 208×208 的内容区域，保留透明留白。图标源图片、生成脚本或依赖发生变化时自动生成，也可手动运行 Generate policy icons；每日规则同步不再重新生成图标。旧版带版本号的图标文件仍保留并同步生成，以兼容已下载的配置。

## 首次导入

### Clash / Clash Mi / Clash Verge

三份配置与 `surge.conf` 一样存放在仓库根目录，共用 `rules/` 中的规则：

- [clash.yaml](https://raw.githubusercontent.com/caozzzzz/surge-rules/main/clash.yaml)：保留美国、香港、新加坡、台湾地区策略组。
- [clashmi.yaml](https://raw.githubusercontent.com/caozzzzz/surge-rules/main/clashmi.yaml)：移除地区策略组，默认开启 TUN。
- [clashverge.yaml](https://raw.githubusercontent.com/caozzzzz/surge-rules/main/clashverge.yaml)：移除地区策略组，默认关闭 TUN。

导入后在本地将 `YOUR_SNELL_PSK` 替换为自己的 Snell 密钥，并将 Airport 的 `url` 示例地址替换为 Clash 格式订阅。Snell v5 需要所用客户端内核支持；这些文件不包含可直接使用的密钥和订阅。不要把真实密钥或订阅 Token 上传到公开仓库。

#### 五份配置的默认设置与差异

下表对比 Surge、Shadowrocket、Clash、Clash Mi、Clash Verge 五份配置中明确写入的设置。客户端开关、覆写和合并配置可能改变最终运行值，应以运行配置为准。未写入某项不等于明确关闭；Surge / Shadowrocket 的 VPN 接管与 Clash 的 `tun.enable` 不能直接等同。

| 项目 | Surge（surge.conf） | Shadowrocket（shadowrocket.conf） | Clash（clash.yaml） | Clash Mi（clashmi.yaml） | Clash Verge（clashverge.yaml） |
| --- | --- | --- | --- | --- | --- |
| TUN / VPN 接管 | 由平台和 App 的 VPN / 增强模式控制；未设置 Clash 式开关 | 由 App 的 VPN 连接开关控制；全局路由需选“配置” | 未配置 `tun`，本文件不主动开启 | `tun.enable: true`，默认开启 | `tun.enable: false`，默认关闭 |
| 接管预设 | `include-all-networks`、`include-local-networks` 均为 false；设置内网绕过 | 设置 `skip-proxy` 和 `tun-excluded-routes`，旁路内网及保留地址 | 无 TUN 预设 | `stack: mixed`；自动路由、自动检测出口网卡均开启 | 与 Mi 相同，开启 TUN 后生效 |
| DNS 接管 | `hijack-dns`：8.8.8.8:53、8.8.4.4:53 | 与 Surge 相同 | 未配置 TUN DNS 接管 | `any:53`（UDP）及 `tcp://any:53` | 与 Mi 相同，TUN 关闭时不通过 TUN 接管 |
| 域名嗅探 | 未设置 Mihomo 式 `sniffer`，不据此推断 App 自身能力 | 未设置 Mihomo 式 `sniffer`，不据此推断 App 自身能力 | 未配置 `sniffer` | 明确开启 HTTP / TLS / QUIC 嗅探 | 与 Mi 相同 |
| DNS 配置 | 阿里/腾讯 DoH + 普通 DNS；读取系统 Hosts；配置 `always-real-ip` 排除 | 阿里/腾讯 DoH + 普通 DNS；失败回退系统 DNS；本地 Hosts 用于代理连接 | Fake-IP；阿里/腾讯 DoH + 默认 DNS；设置 Fake-IP 排除列表 | 与 Clash 相同，额外明确开启 `respect-rules`、`use-hosts`、`use-system-hosts`，设置节点域名解析 DNS | 与 Mi 相同 |
| 地区组 | 美国、新加坡 select；香港、台湾 fallback | 美国、新加坡 url-test；香港 load-balance；台湾 fallback | 美国、新加坡 select；香港、台湾 fallback | 删除四个地区组 | 与 Mi 相同 |
| 节点来源与主代理组 | Airport 的 Surge 格式订阅、四个地区组、Zus/Zjp 示例节点 | 首页独立添加节点/订阅；主组可选地区、全部节点及 PROXY，默认香港 | Snell5、四个地区组及 Airport 的 Clash 格式订阅节点 | Snell5 和 Airport 订阅节点 | 与 Mi 相同 |
| 业务组 | Netflix、Telegram、Twitter、TikTok、AI、Spotify、YouTube、YouTube-Music 保留相应地区候选 | 保留地区候选；AI/Netflix/Twitter/TikTok 默认美国，Spotify 默认新加坡，其余三项默认香港 | 与 Surge 的这些业务组候选一致 | 这些业务组只引用主代理组 `Proxy🪁` | 与 Mi 相同 |
| Apple-AI | 主代理组、美国组、DIRECT | 主代理组、美国组、DIRECT；明确默认主代理组 | 主代理组、美国组、DIRECT | 主代理组、DIRECT | 与 Mi 相同 |
| Apple 普通服务 | 引用 `rules/apple.list`；Apple 候选为 DIRECT、主代理组 | 未引用 `rules/apple.list`；部分 Apple 域名显式指定 Apple，默认 DIRECT | 引用 `rules/apple.list`；Apple 候选为 DIRECT、主代理组 | 与 Clash 相同 | 与 Clash 相同 |
| 广告规则 | 定义 AdBlock 组，但未引用广告规则集 | 引用 `rules/adblock.list`，AdBlock 默认 REJECT | 引用广告规则集；AdBlock 候选为 REJECT、DIRECT | 与 Clash 相同 | 与 Clash 相同 |
| IPv6 | 关闭，`ipv6-vif = disabled` | `ipv6`、`prefer-ipv6` 均为 false | 关闭 | 关闭 | 关闭 |
| QUIC | `block-quic = all-proxy` | `block-quic = all-proxy` | 未显式配置同等 QUIC 阻断项 | 与 Clash 相同；嗅探 QUIC 不等于阻断 QUIC | 与 Mi 相同 |
| 代理入口与共享 | HTTP 6152、SOCKS 6153；监听地址 0.0.0.0，Wi-Fi/热点共享开关关闭 | 文件未显式设置本地代理监听端口 | 混合端口 7890；`allow-lan: false` | 与 Clash 相同 | 与 Clash 相同 |
| 规则与兜底 | 内网、国内直连；Apple-AI 在 AI / Apple 前；最终交给 Final | 内网、国内直连；广告先于业务规则；最终交给“兜底策略” | 内网、国内直连；Apple-AI 在 AI / Apple 前；最终 MATCH,Final | 与 Clash 相同 | 与 Clash 相同 |

逐项解析对比后，`clashmi.yaml` 和 `clashverge.yaml` 的有效配置仅有 `tun.enable` 不同，文件头注释也不同。Clash 版本还存在上表列出的 DNS、嗅探和策略组差异，并非只差一个 TUN 开关。

以下共同设置仅指三份 Clash 系列 YAML，不包括 Surge / Shadowrocket：混合代理端口 `7890`、`allow-lan: false`（不向局域网开放代理）、`mode: rule`（规则分流）、日志 `info`、IPv6 关闭、`unified-delay` 和 `tcp-concurrent` 开启、保存策略选择和 Fake-IP 映射。DNS 均开启 Fake-IP，使用相同的默认 DNS、DoH 和 Fake-IP 排除列表。Snell5 节点模板、Airport 订阅模板、12 份规则集及规则匹配顺序相同；Airport 和规则集的更新间隔为 86400 秒，Airport 健康检查间隔为 600 秒。Final、Apple、AdBlock 的候选策略也相同。

Mi / Verge 的嗅探预设：HTTP 端口 80、8080–8880，TLS / QUIC 端口 443、8443；开启 `parse-pure-ip` 和 `override-destination`，排除 `Mijia Cloud` 与 `+.push.apple.com`。嗅探用于辅助识别连接域名，不是解密 HTTPS 内容。

#### TUN 开启和关闭的区别

| 状态 | 流量如何进入客户端 | 实际使用区别 |
| --- | --- | --- |
| 开启 TUN | 创建虚拟网卡；本配置启用自动路由，将被路由接管的流量交给内核处理 | 能覆盖很多不遵循系统代理的软件，包括部分游戏和命令行工具；可接管 TCP / UDP，实际代理能力仍取决于内核和节点 |
| 关闭 TUN | 本文件仍提供 `7890` HTTP / SOCKS 混合代理入口 | 通常需开启客户端的“系统代理”，或在软件中手动指定代理；忽略系统代理的软件可能直接联网。SOCKS 的 UDP 能力取决于应用和节点，普通 HTTP 系统代理不能全面接管 UDP |

**TUN 开启不等于所有连接都走代理。** 三份文件都使用规则模式：被接管的连接仍按规则走 DIRECT、代理或 REJECT；例如国内规则和内网规则仍直连。关闭 TUN 也不等于关闭代理，但未进入代理入口的连接不会受到这些分流规则控制。

桌面系统启用 TUN 通常需要管理员权限或客户端服务模式，移动端通常需要授予 VPN 权限；它会改变路由/DNS 的处理范围，可能与其他 VPN 或虚拟网卡软件冲突，不保证提速或降低延迟。DNS 接管也不是对所有 DNS 的强制拦截：本预设针对 53 端口，应用自行使用的 DoH/DoT 不会仅因这些设置自动变成内置 DNS 查询。

在支持并直接使用这些 YAML 的客户端里，可通过 `tun.enable` 切换 Mi / Verge 的 TUN；仅修改 `clash.yaml` 的一个开关不足以获得完整的 Mi / Verge TUN 预设。手机客户端是否启用系统 VPN 隧道，还取决于客户端自身实现和权限，不能只凭 YAML 是否含 `tun` 判断。

TUN 参数及接管限制参考 [Mihomo 官方 TUN 文档](https://wiki.metacubex.one/config/inbound/tun/)；系统代理入口与 TUN 覆盖范围参考 [MetaCubeX 官方 metacubexd 说明](https://github.com/MetaCubeX/metacubexd)。以上对比来自本仓库文件，未进行各平台实机连通性测试。

### Surge

`surge.conf` 预置了本仓库的可下载示例订阅 `examples/subscription.list`，四个地区组均有名称匹配的示例节点，示例节点不能实际连接。使用时只需将 Airport 行 `policy-path=` 后面的 URL 替换为你的 Surge 格式完整订阅 URL，再把订阅域名规则中的 `example.com` 改成真实订阅域名。Airport 的 `hidden=1` 可改为 `hidden=0` 以显示入口。Zus、Zjp 是独立示例节点，可修改或删除；删除时也要从 Proxy🪁 策略组移除引用。本地控制 API 默认注释关闭。不要将真实订阅 Token 提交到公开仓库。

#### Surge 默认行为

- 美国、新加坡地区组为手动选择；香港、台湾为 fallback，按优先级选择可用节点。地区筛选依赖订阅节点名称。
- Airport 订阅每 86400 秒更新；香港、台湾健康检查间隔为 600 秒。配置引用本仓库托管的策略组图标。
- DNS 使用国内普通 DNS 和 DoH，IPv6 关闭，Wi-Fi/热点代理共享关闭，本地控制 API 默认注释关闭。
- Apple 默认直连；Apple-AI 优先于 AI 和普通 Apple 规则。内网、国内规则直连，其余流量交给 Final 策略组。
- `block-quic = all-proxy` 阻止代理方向的 QUIC，直连不受此设置影响。
- 当前 Surge 主配置虽然定义了 AdBlock 策略组，但未引用 `rules/adblock.list`，因此不能仅凭出现该策略组就认为启用了该广告规则集。

在 Surge 中通过上方 Raw 地址导入配置，替换示例订阅或节点后，选择规则模式并启动连接。YAML 中的 `tun.enable` 是 Clash/Mihomo 配置项，不能用它判断 Surge 的 VPN 或增强模式状态；实际接管方式取决于平台及 App 设置。

### Shadowrocket

1. 在首页添加自己的订阅或节点；订阅与本仓库的规则配置分开管理，公开配置的 `[Proxy]` 段不包含节点凭据。
2. 在“配置”页通过 [shadowrocket.conf 的 Raw 地址](https://raw.githubusercontent.com/caozzzzz/surge-rules/main/shadowrocket.conf) 下载配置，并选择“使用配置”。
3. 首页“全局路由”选择“配置”，选择可用节点/策略组，然后启动连接；首次连接按系统提示授予 VPN 权限。
4. 节点名需包含香港/HK、新加坡/SG、美国/US、台湾/TW 等地区关键词；某个地区组没有节点时，检查命名或改选“全部节点”/`PROXY`。`PROXY` 指首页当前选中的节点。

#### Shadowrocket 默认行为

| 项目 | 当前配置 |
| --- | --- |
| 地区组 | 美国、新加坡为 `url-test` 自动测速选择；香港为 `load-balance`；台湾为 `fallback`；检查间隔 600 秒 |
| 主代理与兜底 | “代理选择”默认选香港组；“兜底策略”默认选“代理选择”，也可手动选 DIRECT |
| 应用策略 | AI、Netflix、Twitter、TikTok 默认美国；Spotify 默认新加坡；Telegram、YouTube、YouTube-Music 默认香港；Apple 默认 DIRECT，Apple-AI 默认“代理选择” |
| DNS | 阿里/腾讯 DoH 与普通 DNS，失败回退系统 DNS；接管发往 `8.8.8.8:53`、`8.8.4.4:53` 的 DNS；IPv6 关闭 |
| 局域网 | `skip-proxy` 和 `tun-excluded-routes` 配置内网及保留网段旁路 |
| 规则 | 内网与 GitHub 规则之后，先匹配广告、Apple-AI、AI 和其他业务；部分 Apple 域名显式交给 Apple；再匹配国内规则与国内 IP，最后兜底 |
| 广告 | 引用 `rules/adblock.list`，AdBlock 默认 REJECT，可手动改 DIRECT |
| QUIC / UDP | `block-quic = all-proxy`；节点不支持 UDP 策略时设为 REJECT |
| HTTPS 解密 | 未配置 MITM hostname，不需要为这份配置安装解密证书 |

Shadowrocket 没有照搬 Surge 的所有设置：香港组使用负载均衡，且当前没有引用 `rules/apple.list`，只对配置中列出的部分 Apple 域名显式指定 Apple 策略。它使用 iOS 的 VPN 隧道接管流量，连接开关及“全局路由”由 App 管理，不使用 Clash YAML 的 `tun.enable` 开关。以上是配置文件的静态说明，不代表已做各平台实机测试。

## Surge 引用

仓库所有者为 `caozzzzz`：

```ini
[Rule]
DOMAIN-SUFFIX,raw.githubusercontent.com,Proxy

RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/apple-ai.list,Apple-AI,extended-matching
RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/ai.list,AI,extended-matching
RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/apple.list,Apple
RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/netflix.list,Netflix
RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/telegram.list,Telegram,no-resolve
RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/twitter.list,Twitter
RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/tiktok.list,TikTok
RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/spotify.list,Spotify
RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/youtube-music.list,YouTube-Music
RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/youtube.list,YouTube
RULE-SET,https://raw.githubusercontent.com/caozzzzz/surge-rules/main/rules/china.list,DIRECT
GEOIP,CN,DIRECT
FINAL,Final,dns-failed
```

规则集文件内部不包含策略名称。策略由主配置中的 `RULE-SET` 行统一指定。

## 自动更新

GitHub Actions 每天北京时间 06:20 同步上游规则，并在提交前执行格式检查。也可以在仓库的 Actions 页面手动运行 `Sync Surge rules`。

## Surge 分流细节

以下仅说明 `surge.conf`。香港策略组使用 fallback，按节点列表优先级选择可用节点，故障时自动切换；Apple-AI 优先于 AI 和普通 Apple 规则匹配。

LAN 检查分为两层：开头使用 `RULE-SET,LAN,DIRECT,no-resolve`，避免为判断是否内网而提前解析所有域名；应用规则之后、国内规则之前保留 `RULE-SET,LAN,DIRECT`，继续识别域名解析出的内网地址。应用规则优先于后面的 LAN 检查；如果自定义内网域名可能命中应用规则，应在开头显式添加其直连规则。此调整不影响应用规则自身可能触发的 DNS 查询。

QUIC 使用 Surge 原生 `block-quic = all-proxy`：经代理转发的 QUIC 被阻止，让 YouTube 等客户端回退到 HTTPS/TCP；DIRECT 流量可使用 QUIC。不再使用按国家判断的 UDP/443 拦截规则，因此也不再一概拒绝其他相应 UDP/443 流量。该开关影响所有代理 QUIC，不仅限于 YouTube。
