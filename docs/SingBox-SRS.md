# sing-box SRS 规则集

本仓库由 CI 自动把 `Filters/AWAvenue-Ads-Rule-Singbox*.json`（sing-box rule-set 源文件）编译为二进制 `.srs` 规则集，并跟随上游 `TG-Twilight/AWAvenue-Ads-Rule` 自动更新。

## 构建产物

| 规则集 | 二进制订阅 | 源文件 |
| --- | --- | --- |
| 全量 | `Filters/AWAvenue-Ads-Rule-Singbox.srs` | `Filters/AWAvenue-Ads-Rule-Singbox.json` |
| 不含隐私 | `Filters/AWAvenue-Ads-Rule-Singbox-No.Privacy.srs` | `Filters/AWAvenue-Ads-Rule-Singbox-No.Privacy.json` |
| 不含不良 | `Filters/AWAvenue-Ads-Rule-Singbox-No.Unwelcome.srs` | `Filters/AWAvenue-Ads-Rule-Singbox-No.Unwelcome.json` |
| 仅广告 | `Filters/AWAvenue-Ads-Rule-Singbox-Only.Ads.srs` | `Filters/AWAvenue-Ads-Rule-Singbox-Only.Ads.json` |

订阅地址（Raw）：

```
https://raw.githubusercontent.com/zuohl/AWAvenue-Ads-Rule/main/Filters/AWAvenue-Ads-Rule-Singbox.srs
```

jsDelivr 镜像：

```
https://gcore.jsdelivr.net/gh/zuohl/AWAvenue-Ads-Rule@main/Filters/AWAvenue-Ads-Rule-Singbox.srs
```

## 配置示例

```json
{
  "route": {
    "rule_set": [
      {
        "type": "remote",
        "tag": "awavenue-ads",
        "format": "binary",
        "url": "https://raw.githubusercontent.com/zuohl/AWAvenue-Ads-Rule/main/Filters/AWAvenue-Ads-Rule-Singbox.srs",
        "download_detour": "direct",
        "update_interval": "1d"
      }
    ],
    "rules": [
      {
        "rule_set": ["awavenue-ads"],
        "outbound": "block"
      }
    ]
  }
}
```

## 自动更新机制

- `build-srs.yml`：`Filters/*Singbox*.json` 发生变更（push、手动触发、或被同步流程调用）时，用 `sing-box rule-set compile` 重新编译 `.srs` 并提交。
- `sync-upstream.yml`：每 6 小时轮询一次上游 `TG-Twilight/AWAvenue-Ads-Rule` 的 `main` 分支头提交（记录在 `.upstream-version`），有变更就合并拉取，随后触发 SRS 构建。也支持手动触发，或通过 `repository_dispatch`（`types: upstream-pushed`）由外部通知即时触发。

## 本地构建

```bash
sing-box rule-set compile --output Filters/AWAvenue-Ads-Rule-Singbox.srs Filters/AWAvenue-Ads-Rule-Singbox.json
```
