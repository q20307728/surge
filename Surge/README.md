⚠️ 注意：
DOMAIN-SET 同时适用于 Surge for Mac v3.5.1 及更新的版本、Surge for iOS v4.2.2 及更新的版本，拥有比 RULE-SET 更优秀的匹配效率，建议优先使用。
RULE-SET 同时适用于 Surge for Mac v3.0 及更新的版本、Surge for iOS v3.4 及更新的版本。

1.直连域名列表 direct.txt

2.代理域名列表 proxy.txt

3.Apple 在中国大陆可直连的域名列表 apple.txt

4.iCloud 域名列表 icloud.txt  推荐直连

5.GFWList 域名列表 gfw.txt    


白名单模式（推荐）
⚠️ 注意：

白名单模式，意为「没有命中规则的网络流量，统统使用代理」，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户。

以下配置中，除了 DIRECT 和 REJECT 是默认存在于 Surge 中的 policy（路由策略/流量处理策略），其余均为自定义 policy，对应配置文件中 [Proxy] 或 [Proxy Group] 中的代理名称。如你直接使用下面的 [Rule] 规则，则需要在 [Proxy] 或 [Proxy Group] 中手动配置一个名为 PROXY 的 policy。

如你希望 Apple、iCloud 和 Google 列表中的域名使用代理，则把 policy 由 DIRECT 改为 PROXY，以此类推，举一反三。
DOMAIN-SET：

[Rule]
PROCESS-NAME,v2ray,DIRECT

PROCESS-NAME,xray,DIRECT

PROCESS-NAME,clash,DIRECT

PROCESS-NAME,naive,DIRECT

PROCESS-NAME,trojan,DIRECT

PROCESS-NAME,trojan-go,DIRECT

PROCESS-NAME,ss-local,DIRECT

PROCESS-NAME,privoxy,DIRECT

PROCESS-NAME,leaf,DIRECT

PROCESS-NAME,Thunder,DIRECT

PROCESS-NAME,DownloadService,DIRECT

PROCESS-NAME,qBittorrent,DIRECT

PROCESS-NAME,Transmission,DIRECT

PROCESS-NAME,fdm,DIRECT

PROCESS-NAME,aria2c,DIRECT

PROCESS-NAME,Folx,DIRECT

PROCESS-NAME,NetTransport,DIRECT

PROCESS-NAME,uTorrent,DIRECT

PROCESS-NAME,WebTorrent,DIRECT

DOMAIN-SET,https://cdn.jsdelivr.net/gh/Loyalsoldier/surge-rules@release/private.txt,DIRECT

DOMAIN-SET,https://cdn.jsdelivr.net/gh/Loyalsoldier/surge-rules@release/reject.txt,REJECT

RULE-SET,SYSTEM,DIRECT

DOMAIN-SET,https://cdn.jsdelivr.net/gh/Loyalsoldier/surge-rules@release/icloud.txt,DIRECT

DOMAIN-SET,https://cdn.jsdelivr.net/gh/Loyalsoldier/surge-rules@release/apple.txt,DIRECT

DOMAIN-SET,https://cdn.jsdelivr.net/gh/Loyalsoldier/surge-rules@release/google.txt,DIRECT

DOMAIN-SET,https://cdn.jsdelivr.net/gh/Loyalsoldier/surge-rules@release/proxy.txt,PROXY,force-remote-dns

DOMAIN-SET,https://cdn.jsdelivr.net/gh/Loyalsoldier/surge-rules@release/direct.txt,DIRECT

RULE-SET,https://cdn.jsdelivr.net/gh/Loyalsoldier/surge-rules@release/telegramcidr.txt,PROXY

RULE-SET,https://cdn.jsdelivr.net/gh/Loyalsoldier/surge-rules@release/cncidr.txt,DIRECT

RULE-SET,LAN,DIRECT

FINAL,PROXY,dns-failed
