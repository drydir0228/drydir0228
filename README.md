{
  "log": {
    "level": "debug",
    "timestamp": true
  },
  "experimental": {
    "clash_api": {
      "external_controller": "127.0.0.1:9090",
      "external_ui": "ui",
      "secret": "",
      "external_ui_download_url": "https://mirror.ghproxy.com/https://github.com/MetaCubeX/Yacd-meta/archive/gh-pages.zip",
      "external_ui_download_detour": "direct",
      "default_mode": "rule"
    },
    "cache_file": {
      "enabled": true,
      "store_fakeip": false
    }
  },
  "dns": {
    "servers": [
      {
        "tag": "proxyDns",
        "address": "tls://8.8.8.8",
        "detour": "proxy"
      },
      {
        "tag": "localDns",
        "address": "https://223.5.5.5/dns-query",
        "detour": "direct"
      },
      {
        "tag": "block",
        "address": "rcode://success"
      }
    ],
    "rules": [
      {
        "domain": [
          "ghproxy.com",
          "cdn.jsdelivr.net",
          "testingcf.jsdelivr.net"
        ],
        "server": "localDns"
      },
      {
        "rule_set": "geosite-category-ads-all",
        "server": "block"
      },
      {
        "outbound": "any",
        "server": "localDns",
        "disable_cache": true
      },
      {
        "rule_set": "geosite-cn",
        "server": "localDns"
      },
      {
        "clash_mode": "direct",
        "server": "localDns"
      },
      {
        "clash_mode": "global",
        "server": "proxyDns"
      },
      {
        "rule_set": "geosite-geolocation-!cn",
        "server": "proxyDns"
      }
    ],
    "final": "localDns",
    "strategy": "ipv4_only"
  },
  "inbounds": [
    {
      "type": "tun",
      "inet4_address": "172.19.0.1/30",
      "mtu": 9000,
      "auto_route": true,
      "strict_route": true,
      "sniff": true,
      "endpoint_independent_nat": false,
      "stack": "system",
      "platform": {
        "http_proxy": {
          "enabled": true,
          "server": "127.0.0.1",
          "server_port": 2080
        }
      }
    },
    {
      "type": "mixed",
      "listen": "127.0.0.1",
      "listen_port": 2080,
      "sniff": true,
      "users": []
    }
{
  "dns": {
    "independent_cache": true,
    "rules": [],
    "servers": [
      {
        "address": "https://8.8.8.8/dns-query",
        "address_resolver": "dns-direct",
        "strategy": "ipv4_only",
        "tag": "dns-remote"
      },
      {
        "address": "dot.pub",
        "address_resolver": "dns-local",
        "detour": "direct",
        "strategy": "ipv4_only",
        "tag": "dns-direct"
      },
      {
        "address": "local",
        "detour": "direct",
        "tag": "dns-local"
      },
      {
        "address": "rcode://success",
        "tag": "dns-block"
      }
    ]
  },
  "experimental": {
    "cache_file": {
      "enabled": true,
      "path": "../cache/clash.db",
      "store_fakeip": true
    },
    "clash_api": {
      "external_controller": "127.0.0.1:9090",
      "external_ui": "../files/yacd"
    }
  },
  "inbounds": [
    {
      "listen": "127.0.0.1",
      "listen_port": 6450,
      "override_address": "8.8.8.8",
      "override_port": 53,
      "tag": "dns-in",
      "type": "direct"
    },
    {
      "domain_strategy": "",
      "endpoint_independent_nat": true,
      "inet4_address": [
        "172.19.0.1/28"
      ],
      "mtu": 9000,
      "sniff": true,
      "sniff_override_destination": false,
      "stack": "system",
      "tag": "tun-in",
      "type": "tun"
    },
    {
      "domain_strategy": "",
      "listen": "127.0.0.1",
      "listen_port": 2080,
      "sniff": true,
      "sniff_override_destination": false,
      "tag": "mixed-in",
      "type": "mixed"
    }
  ],
  "log": {
    "level": "panic"
  },
  "outbounds": [
    {
      "password": "",
      "server": "183.240.98.84",
      "server_port": 443,
      "username": "",
      "type": "http",
      "domain_strategy": "",
      "headers": {
        "Host": "sptest.baidu.com",
        "X-T5-Auth": "482857715"
      },
      "tag": "proxy"
    },
    {
      "tag": "direct",
      "type": "direct"
    },
    {
      "tag": "bypass",
      "type": "direct"
    },
    {
      "tag": "block",
      "type": "block"
    },
    {
      "tag": "dns-out",
      "type": "dns"
    }
  ],
  "route": {
    "auto_detect_interface": true,
    "rule_set": [
      {
        "format": "binary",
        "path": "/storage/emulated/0/Android/data/moe.nb4a/files/ruleSets/geosite-cn.srs",
        "tag": "geosite:cn",
        "type": "local"
      },
      {
        "format": "binary",
        "path": "/storage/emulated/0/Android/data/moe.nb4a/files/ruleSets/geoip-cn.srs",
        "tag": "geoip:cn",
        "type": "local"
      }
    ],
    "rules": [
      {
        "outbound": "dns-out",
        "port": [
          53
        ]
      },
      {
        "inbound": [
          "dns-in"
        ],
        "outbound": "dns-out"
      },
      {
        "outbound": "proxy",
        "rule_set": [
          "geosite:cn"
        ]
      },
      {
        "outbound": "proxy",
        "rule_set": [
          "geoip:cn"
        ],
        "rule_set_ipcidr_match_source": true
      },
      {
        "ip_cidr": [
          "224.0.0.0/3",
          "ff00::/8"
        ],
        "outbound": "block",
        "source_ip_cidr": [
          "224.0.0.0/3",
          "ff00::/8"
        ]
      }
    ]
  }
}
