# 🌐 Casper Free Proxies

> **Free HTTP, SOCKS4 & SOCKS5 proxies — automatically validated and updated every 30 minutes.**  
> No API keys. No rate limits. Just raw, tested proxies.

[![Updated](https://img.shields.io/badge/Updated-Every%2030%20min-brightgreen?style=flat-square)](https://github.com/Casper-Tech-ke/casper-free-proxies)
[![HTTP](https://img.shields.io/badge/HTTP-Live-blue?style=flat-square)](https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/http.json)
[![SOCKS4](https://img.shields.io/badge/SOCKS4-Live-orange?style=flat-square)](https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/socks4.json)
[![SOCKS5](https://img.shields.io/badge/SOCKS5-Live-purple?style=flat-square)](https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/socks5.json)
[![Made in Kenya](https://img.shields.io/badge/Made%20in-Kenya%20🇰🇪-red?style=flat-square)](https://xcasper.space)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

---

## 📋 Table of Contents

- [How it Works](#-how-it-works)
- [Download Proxies](#-download-proxies)
- [File Formats](#-file-formats)
- [REST API](#-rest-api)
- [Usage Examples](#-usage-examples)
- [About](#-about)

---

## ⚙️ How it Works

```
┌─────────────────────────────────────────────────────────────────┐
│                     CASPER PROXY ENGINE                         │
│                                                                 │
│  1. COLLECT  →  Gather raw proxies from multiple sources        │
│  2. TEST     →  Validate each proxy (response time + liveness)  │
│  3. FILTER   →  Keep only working proxies                       │
│  4. PUBLISH  →  Push clean JSON files to this GitHub repo       │
│                                                                 │
│  ⏱  Runs automatically every 30 minutes                        │
└─────────────────────────────────────────────────────────────────┘
```

Every proxy in this repo has been **tested and confirmed working** at the time of the last update. Dead proxies are automatically removed on each cycle.

---

## 📥 Download Proxies

All files are updated automatically. Use the **raw** GitHub URLs directly in your code:

### HTTP Proxies
```
https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/http.json
```

### SOCKS4 Proxies
```
https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/socks4.json
```

### SOCKS5 Proxies
```
https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/socks5.json
```

### All Combined
```
https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/proxies.json
```

### 50 Random Proxies
```
https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/random.json
```

### Metadata (with speed + country)
```
https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/metadata.json
```

### Last Updated Timestamp
```
https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/timestamp.json
```

---

## 📄 File Formats

### `http.json` / `socks4.json` / `socks5.json`
```json
{
  "updated": "2026-04-04T10:30:00.000Z",
  "count": 142,
  "proxies": [
    "41.65.67.165:1976",
    "103.30.31.59:20326",
    "167.103.34.108:8800"
  ]
}
```

### `metadata.json`
```json
{
  "41.65.67.165:1976": {
    "type": "http",
    "response_time": 1.34,
    "country": "KE",
    "anonymity": "elite"
  },
  "103.30.31.59:20326": {
    "type": "socks5",
    "response_time": 2.81,
    "country": "PH",
    "anonymity": "anonymous"
  }
}
```

### `timestamp.json`
```json
{
  "utc": "2026-04-04T10:30:00.000Z"
}
```

---

## 🔌 REST API

A live HTTP API is available at:

```
https://proxies.xcasper.space
```

| Endpoint        | Description                              |
|----------------|------------------------------------------|
| `GET /`        | API info, stats and endpoint list        |
| `GET /proxies` | All working proxies (combined)           |
| `GET /http`    | HTTP proxies only                        |
| `GET /socks4`  | SOCKS4 proxies only                      |
| `GET /socks5`  | SOCKS5 proxies only                      |
| `GET /random`  | 50 random proxies from the current pool  |
| `GET /metadata`| Full metadata: speed, country, type      |
| `GET /stats`   | Counts by proxy type                     |
| `GET /timestamp`| Last update time (UTC)                  |

### Example Response (`GET /stats`)
```json
{
  "provider": "CASPER TECH KENYA",
  "last_updated": "2026-04-04T10:30:00.000Z",
  "http": 324,
  "socks4": 66,
  "socks5": 0,
  "total": 390
}
```

---

## 💡 Usage Examples

### JavaScript / Node.js
```js
const fetch = require('node-fetch');
const { HttpsProxyAgent } = require('https-proxy-agent');

// Fetch a random proxy
const res = await fetch('https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/random.json');
const { proxies } = await res.json();
const proxy = proxies[Math.floor(Math.random() * proxies.length)];

// Use it
const agent = new HttpsProxyAgent(`http://${proxy}`);
const data = await fetch('https://api.ipify.org?format=json', { agent });
console.log(await data.json());
```

### Python
```python
import requests
import random

# Fetch proxies
r = requests.get('https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/http.json')
proxies_list = r.json()['proxies']

# Pick one randomly
proxy = random.choice(proxies_list)

# Use it
proxies = {'http': f'http://{proxy}', 'https': f'http://{proxy}'}
response = requests.get('https://api.ipify.org?format=json', proxies=proxies, timeout=8)
print(response.json())
```

### Python (with metadata — pick fastest)
```python
import requests

r = requests.get('https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/metadata.json')
meta = r.json()

# Sort by response time, pick top 5 HTTP proxies
http_proxies = [
    (k, v) for k, v in meta.items()
    if v['type'] == 'http' and v['response_time'] is not None
]
http_proxies.sort(key=lambda x: x[1]['response_time'])
fastest = http_proxies[:5]

for proxy, info in fastest:
    print(f"{proxy} — {info['response_time']}s — {info['country']}")
```

### cURL
```bash
# Get all HTTP proxies
curl https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/http.json

# Test a proxy directly
curl --proxy http://41.65.67.165:1976 https://api.ipify.org
```

### PHP
```php
$url = 'https://raw.githubusercontent.com/Casper-Tech-ke/casper-free-proxies/main/files/http.json';
$data = json_decode(file_get_contents($url), true);
$proxies = $data['proxies'];
$proxy = $proxies[array_rand($proxies)];

$ch = curl_init('https://api.ipify.org?format=json');
curl_setopt($ch, CURLOPT_PROXY, "http://$proxy");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_TIMEOUT, 8);
$result = curl_exec($ch);
echo $result;
```

---

## 📦 About

**Built and maintained by:**  
[TRABY CASPER](https://xcasper.space) — Casper Tech Kenya 🇰🇪

**Part of the Casper Tech ecosystem:**
- 🌐 [apis.xcasper.space](https://apis.xcasper.space) — 166+ Free REST APIs
- 🤖 GitHub: [@Casper-Tech-ke](https://github.com/Casper-Tech-ke)

**License:** MIT — use freely, no attribution required.

---

> *"No API Keys. No Rate Limits. Just working proxies."*
