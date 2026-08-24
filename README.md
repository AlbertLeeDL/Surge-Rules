# Surge-Rules

个人使用的 Surge 规则集，覆盖 PT、加密货币交易所、TradingView、境外券商、境外银行和 T-Mobile。

## 规则集

| 文件 | 建议策略 | 用途 |
| --- | --- | --- |
| `rules/PrivateTracker.list` | `DIRECT` | PT 网站与 Tracker 域名 |
| `rules/Binance.list` | `加密货币` | Binance 网页、App 与 API |
| `rules/OKX.list` | `加密货币` | OKX 网页、App 与 API |
| `rules/Futu.list` | `金融-香港` | 富途及富途香港 |
| `rules/IBKR.list` | `金融-美国` | Interactive Brokers |
| `rules/TradingView.list` | `金融-美国` | TradingView 网页、App、API 与行情连接 |
| `rules/BOCHK.list` | `金融-香港` | 中国银行（香港） |
| `rules/USBanks.list` | `金融-美国` | Capital One、American Express、Bank of America |
| `rules/TMobileAccount.list` | `美国手机卡` | T-Mobile 账户网页与 App |
| `rules/TMobileSystem.list` | `DIRECT` | T-Mobile Wi-Fi Calling 与运营商系统流量 |

## Surge 配置

将以下规则放在通用 Proxy、China 和 FINAL 规则之前。必须把 `TMobileSystem` 放在 `TMobileAccount` 前面，确保运营商系统流量保持直连。

```ini
# T-Mobile 系统流量
RULE-SET,https://raw.githubusercontent.com/AlbertLeeDL/Surge-Rules/main/rules/TMobileSystem.list,DIRECT

# PT
RULE-SET,https://raw.githubusercontent.com/AlbertLeeDL/Surge-Rules/main/rules/PrivateTracker.list,DIRECT

# 加密货币交易所
RULE-SET,https://raw.githubusercontent.com/AlbertLeeDL/Surge-Rules/main/rules/Binance.list,加密货币
RULE-SET,https://raw.githubusercontent.com/AlbertLeeDL/Surge-Rules/main/rules/OKX.list,加密货币

# 香港金融
RULE-SET,https://raw.githubusercontent.com/AlbertLeeDL/Surge-Rules/main/rules/Futu.list,金融-香港
RULE-SET,https://raw.githubusercontent.com/AlbertLeeDL/Surge-Rules/main/rules/BOCHK.list,金融-香港

# 美国金融
RULE-SET,https://raw.githubusercontent.com/AlbertLeeDL/Surge-Rules/main/rules/IBKR.list,金融-美国
RULE-SET,https://raw.githubusercontent.com/AlbertLeeDL/Surge-Rules/main/rules/TradingView.list,金融-美国
RULE-SET,https://raw.githubusercontent.com/AlbertLeeDL/Surge-Rules/main/rules/USBanks.list,金融-美国

# T-Mobile 账户网站与 App
RULE-SET,https://raw.githubusercontent.com/AlbertLeeDL/Surge-Rules/main/rules/TMobileAccount.list,美国手机卡
```

## 注意事项

- 金融和交易所策略应使用手动选择的稳定节点或 `DIRECT`，不要使用 smart、load-balance 等自动换出口的策略。
- `PrivateTracker.list` 只匹配站点和 Tracker 域名，无法覆盖直接连接 IP 的 Peer 流量。qBittorrent 仍需在容器或网络层保持直连。
- `TMobileSystem.list` 应始终使用 `DIRECT`，不要和 T-Mobile 账户网站规则合并。
- 富途规则只包含域名，没有加入可能误匹配其他腾讯云服务的大范围 IP 段。
- 本仓库不包含机场订阅、账户标识、API Key、Cookie、PT Passkey 或其他秘密信息。
