# Protokol Harian

Dashboard peribadi: jadual solat, selawat 12K, azan picker, rutin harian — data live JAKIM (api.waktusolat.app).

## Ciri

- **Solat tracker** — Subuh / Zohor / Asar / Maghrib / Isyak ikut zon semasa (auto-detect atau manual)
- **Selawat 12K** — chunks dinamik ikut waktu solat (weekday vs weekend)
- **Zikir tareqat** — 2 sesi × 30 minit
- **Azan picker** — pilih reciter, main preview, auto-play ikut waktu solat, silent mode
- **Ramadhan countdown** — ke Ramadhan 1448H
- **Theme toggle** — dark / light mode

## Stack

Single-file HTML (~62KB) + Google Fonts (Fraunces / Karla / JetBrains Mono). Tiada build step, tiada framework.

## Jalankan

### Lokal (laptop)

```bash
# Buka terus dalam browser
xdg-open jadual-harian.html
```

### PWA (telefon / tablet)

1. Buka URL GitHub Pages dalam Chrome
2. Menu (⋮) → **"Add to Home Screen"**
3. App boleh dibuka macam native — support offline, auto-update

## PWA bits

- `manifest.json` — app name, icons, theme color, display standalone
- `sw.js` — service worker, cache shell + Google Fonts, network-first untuk JAKIM API
- `icon-192.png` + `icon-512.png` — geometric seal design (gold on dark, matching dashboard palette)

## Cache strategy

| Resource | Strategy |
|----------|----------|
| Shell (`jadual-harian.html`, manifest, icons) | Cache-first (offline) |
| Google Fonts (CSS + woff2) | Cache-first (offline) |
| JAKIM API (`waktusolat.app`) | Network-first, fallback cache |

Cache version: `jadual-v1`. Bump version bila tukar shell files untuk invalidate client cache.

## Deployment

GitHub Pages. Push ke `main` → auto-deploy ke `https://azizizaidi.github.io/jadual-harian/`.

## Sumber data

- Waktu solat: [api.waktusolat.app](https://api.waktusolat.app)
- Hijri: kiraan client-side
- Lokasi: `navigator.geolocation` + zone code (SGR03 = Klang, WLY01 = Putrajaya)