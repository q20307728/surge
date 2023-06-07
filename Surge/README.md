⚠️ 注意：
DOMAIN-SET 同时适用于 Surge for Mac v3.5.1 及更新的版本、Surge for iOS v4.2.2 及更新的版本，拥有比 RULE-SET 更优秀的匹配效率，建议优先使用。
RULE-SET 同时适用于 Surge for Mac v3.0 及更新的版本、Surge for iOS v3.4 及更新的版本。
1.域名规则
DOMAIN：严格匹配某域名。

DOMAIN-SUFFIX：匹配某域名及其子域名，如 DOMAIN-SUFFIX,apple.com 可以匹配 apple.com 和 www.apple.com，但是不会匹配 anapple.com。

DOMAIN-KEYWORD：简单的字符串搜索，只要域名包含子串就会匹配。

DOMAIN-SET：专为大量域名集列表文件设计，支持上万条记录的快速查询。文件中每行为一个域名，如果某行以 . 开头则表示匹配所有子域名和该域名本身。可用于广告过滤。

IP 地址规则
当连接的目标主机的 IP 地址符合时，匹配该规则。包含 IP-CIDR，IP-CIDR6，GEOIP 三种类型。

当目标主机名是一个域名或主机名时，IP 类型规则会触发本地 DNS 解析。根据解析得到的 IP 地址进行判断。当解析失败是： * 如果最终的 FINAL 规则带有 dns-failed 标记，那么将直接匹配 FINAL 规则。 * 如果 FINAL 规则不带有 dns-failed 标记，该请求将直接失败。

IP 类型规则有一个专有的参数 no-resolve，如果一个 IP 规则带有该参数，那么 1. 如果目标主机名是一个域名，那么将跳过该规则，不触发 DNS 解析。 2. 如果目标主机名是 IP 地址，按规则进行判断。 3. 如果目标主机名是一个域名，且先前出现的 IP 规则已经触发了 DNS 解析获得了 IP 地址，那么使用该 IP 地址进行判断。

由于 DNS 查询有时间开销，所以在配置规则时，最优的方式是尽量先不触发 DNS 解析，将所有会触发 DNS 解析的规则放在底部。这样应使用代理策略的请求就完全避免了在本地进行 DNS 解析。

但是也不用刻意的去完全避免解析，因为一旦决定使用 DIRECT 策略，那么最终还是要进行解析。Surge 有完备的 DNS 缓存系统，不必在意短时间内的重复解析。

不过需要注意，如果某目标主机在本地 DNS 不可被解析，那么一定需要加入对应的规则，在触发 DNS 前就决定策略终止匹配。或者对 FINAL 规则加上 dns-failed 标记并使用代理策略

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
