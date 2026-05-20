# Pixeldrain Bypass Pro Ultra v7

Userscript bypass untuk Pixeldrain — download tanpa bandwidth cap, speed multiplication via parallel connections, multi-proxy failover, dan auto-bypass semua proteksi.

## Fitur Utama

- **Speed Multiplication** — 32 parallel Range connections × 1 MB/s = hingga 32 MB/s (bypass server throttle)
- **16 Proxy Mirrors** — Zero bandwidth usage di akun Pixeldrain kamu
- **Circuit Breaker** — Auto-isolate mirror mati, auto-recover setelah cooldown
- **Pre-flight Check** — Deteksi captcha/block SEBELUM download dimulai
- **View-Pad** — Cegah captcha trigger (maintain download/view ratio)
- **Resumable Downloads** — Chunk progress tersimpan di GM storage, lanjut kapan saja
- **Premium Auth (Layer A)** — API key support untuk unlimited tanpa cap
- **Adaptive Chunking** — Chunk size otomatis sesuai file size & mirror availability
- **Export** — curl, wget, aria2c, axel, IDM, PowerShell, HTTPie + batch .sh/.bat
- **External Downloaders** — JDownloader & Aria2 RPC integration
- **QR Code** — Generate QR untuk download di device lain
- **Video Unlock** — Bypass "logged-in only" gate pada video (client-side patch)
- **SPA-aware** — Toolbar inject ulang otomatis saat navigasi

## Install

1. Install [Tampermonkey](https://www.tampermonkey.net/) atau [Violentmonkey](https://violentmonkey.github.io/)
2. Buat script baru, paste isi `pixeldrain-bypas.js`
3. Buka halaman Pixeldrain — toolbar bypass muncul otomatis

## Download Strategies (urutan auto-failover)

| # | Strategy | Layer | Keterangan |
|---|----------|-------|------------|
| 1 | `premium_direct` | A | API key, unlimited, no cap |
| 2 | `speed_multiplied` | B | N×1MB/s parallel Range across mirrors |
| 3 | `multi_proxy_ranged` | B | Chunked parallel across mirrors |
| 4 | `single_host_ranged` | B | Multi-connection single mirror |
| 5 | `gm_stream_fsa` | B | GM xhr → File System Access stream |
| 6 | `native_fetch` | C | Browser fetch no-referrer (CORS open) |
| 7 | `gm_blob` | B | In-RAM blob fallback |
| 8 | `gm_download` | B | Browser download manager |

## Bypass Layers

```
Layer A — Premium API key → pixeldrain.com direct, unlimited
Layer B — Proxy mirrors + GM Referer spoof → 0 bandwidth on your IP
Layer C — Direct CORS fetch → works always, costs 6GB cap
Layer D — viewer_data patch → unlock video/download gates
Layer E — View-pad → prevent captcha by maintaining view ratio
```

## Pixeldrain Protection (yang di-bypass)

| Protection | Bypass Method |
|---|---|
| 1 MB/s per-connection throttle | Parallel connections (speed multiplication) |
| 6 GB/24h bandwidth cap | Proxy mirrors (traffic never hits your IP) |
| reCAPTCHA v2 | Pre-flight detection + view-pad prevention |
| Hotlink ratio detection | View-pad maintains download/view ratio |
| "Logged-in only" video gate | `viewer_data` Object.defineProperty patch |
| Rate limit (3000 req/window) | Distributed across 16 mirrors |

## Settings

Klik tombol **⚙** di toolbar atau menu Tampermonkey → Settings. Konfigurasi:

- API key (premium)
- Max connections & chunk size
- Custom proxy hosts
- Circuit breaker threshold
- External downloader URLs
- Auto-download on page load
- Debug logging

## Requirements

- Browser modern (Chrome/Firefox/Edge)
- Tampermonkey atau Violentmonkey
- File System Access API (untuk speed multiplication & streaming) — Chrome/Edge only

## Credits

Based on work by MegaLime0, hhoneeyy, Nurarihyon, hdyzen, nazdridoy.

## License

MIT
