# My Custom Clash Rules

这是我个人的 Clash 自定义规则集，主要用于存放自建服务域名以及防止去广告规则（REJECT）误杀的特定应用域名（如 QQ音乐、英雄联盟等）。

## 🚀 接入方式

请将以下内容添加到你的 Clash 主配置文件的对应位置。

### 1. 添加 Rule Provider
在配置文件的 `rule-providers` 模块下添加：

```yaml
rule-providers:
  # ... 其他规则集 ...
  
  my_whitelist:
    type: http
    behavior: classical
    format: yaml
    # 注意：下面这个链接请替换为本仓库 my_whitelist.yaml 的真实 Raw 链接
    url: "https://raw.githubusercontent.com/你的GitHub用户名/clash-custom-rules/main/my_whitelist.yaml"
    path: ./ruleset/custom/my_whitelist.yaml
    interval: 86400
```

### 2. 路由规则引用
在配置文件的 `rules` 列表的**最顶部**（必须在 `REJECT` 规则之前），添加以下路由：

```yaml
rules:
  - "RULE-SET,my_whitelist,DIRECT"  # 命中自定义规则的流量直接放行（也可改为指定节点组）
  - "RULE-SET,reject,REJECT"        # 原有的广告拦截大闸
  # ... 其他规则 ...
```

## 🛠️ 如何维护
如果以后有新的 App 被误杀，或者有新的自建域名：
1. 直接在 GitHub 网页端编辑 `my_whitelist.yaml`。
2. 按照 `  - DOMAIN-SUFFIX,example.com` 的格式追加。
3. 提交 Commit。
4. 在 Clash 客户端中点击更新 `my_whitelist` 规则集即可生效。
