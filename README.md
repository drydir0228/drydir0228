

mixed-port: 7890
allow-lan: false
mode: rule
geodata-mode: true
log-level: info
ipv6: false
find-process-mode: strict
global-client-fingerprint: chrome
geodata-loader: memconservative
keep-alive-interval: 30
tcp-concurrent: true

dns:
  enable: true
  ipv6: false
  listen: 0.0.0.0:1053
  enhanced-mode: fake-ip
  fake-ip-range: 28.0.0.1/8
  default-nameserver:
    - https://223.6.6.6/dns-query
    - https://1.1.1.1/dns-query
  nameserver:
    - https://223.6.6.6/dns-query
    - https://1.1.1.1/dns-query

geox-url:
  geoip: "https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.metadb"

proxy-providers:
  nanoport:
    type: http
    url: https://api.v1.mk/sub?target=clash&url=https%3A%2F%2Fsub.cfno1.eu.org%2Fclash%3Fhost%3Dworld.kh11a.link%26uuid%3D6a26eadd-4580-49ba-ab5c-e96bf90f3b99%26path%3D%2F%3Fed%3D2048%26protocol%3Dvless&insert=false&config=https%3A%2F%2Fraw.githubusercontent.com%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online_Mini_AdblockPlus.ini&emoji=true&list=false&xudp=false&udp=false&tfo=false&expand=true&scv=false&fdn=false&new_name=true
    interval: 432000
    path: ./provider201.yaml
    health-check:
      enable: false
      interval: 7200
      url: http://www.gstatic.com/generate_204

  

baidu: &baidu
  type: http
  port: 443
  headers:
    Host: 153.3.236.22:443
    X-T5-Auth: 683556433
    User-Agent: okhttp/3.11.0 Dalvik/2.1.0 (Linux; Build/RKQ1.200826.002) baiduboxapp/11.0.5.12 (Baidu; P1 11)
        
proxies:

  - name: 绉诲姩
    server: 183.240.98.84
    <<: *baidu

    
proxy-groups:
  - name: 馃寧 鍏ㄧ悆鑺傜偣
    type: select
    use:
      - nanoport
 
    proxies:
      - 馃殌 鐧惧害鐩磋繛

  - name: 馃殌 鐧惧害鐩磋繛
    type: select
    proxies:
      - 绉诲姩 
      - 馃幆 鍏ㄧ悆鐩磋繛
      
  - name: 馃幆 鍏ㄧ悆鐩磋繛
    type: select
    proxies:
      - DIRECT

  - name: 馃洃 鍏ㄧ悆鎷︽埅
    type: select
    proxies:
      - REJECT

  - name: 馃悷 婕忕綉涔嬮奔
    type: relay
    proxies:
      - 馃殌 鐧惧害鐩磋繛
      - 馃寧 鍏ㄧ悆鑺傜偣



rule-providers:
  reject:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/reject.txt"
    path: ./ruleset/reject.yaml
    interval: 86400


rules:
  # UDP 瑙勫垯 
  - NETWORK,UDP,馃洃 鍏ㄧ悆鎷︽埅
  # 骞垮憡杩囨护
  - geosite,category-ads-all,馃洃 鍏ㄧ悆鎷︽埅
  - RULE-SET,reject,馃洃 鍏ㄧ悆鎷︽埅
  # 鍒嗘祦瑙勫垯
  - DOMAIN-KEYWORD,google,馃悷 婕忕綉涔嬮奔
  - GEOSITE,CN,馃殌 鐧惧害鐩磋繛
  - GEOIP,CN,馃殌 鐧惧害鐩磋繛
  - MATCH,馃悷 婕忕綉涔嬮奔
