# MJS Weather

**Single-file weather page (PWA) for rain-cell arrival and convection at a point.**
It extracts the ECMWF IFS HRES 9 km point forecast for any coordinate, scans the 36 km surroundings, moves rain cells to the site with several motion vectors, and combines this with Himawari-9 infrared (JMA tiles) to follow cold cloud. Everything runs in your browser. There is no server and no account.

> **Experimental.** The rules and thresholds are heuristic and were tested mostly at one site in East Java (November 2026). It is not an official forecast or warning. Do not use it for safety-critical decisions.

Live page (after enabling GitHub Pages): <https://rakawi182.github.io/MJS-Weather/>

[Bahasa Indonesia ↓](#bahasa-indonesia)

---

## What it does

Seven tabs: **Overview · Surface · Convection · Satellite · Surroundings · Charts · Notes**.

- **Point extraction.** IFS HRES hourly values for the chosen coordinate, other models for comparison (GFS, ICON, JMA, UKMO, AIFS) and the ECMWF ensemble, all through Open-Meteo.
- **Surroundings scan.** 3 rings × 8 directions (radius 36 km, about the IFS grid spacing), steering wind from 850/700/500 hPa, CAPE, 10 m wind divergence, temperature and water-vapour maps.
- **Rain-cell arrival.** Cells with ≥ 0.5 mm/h are moved in a straight line with the steering wind, the pattern shift, the Corfidi vector and the IR motion. Output: closest-pass time (ETA), a 300-member cone with probabilities for the next 30 and 60 min, ETA labels and 30/60/90 min time lines on the polar map. New cell development is **not** modelled.
- **Satellite.** Himawari-9 B13 (infrared), B08 (water vapour) and B03 (visible) from JMA tiles: parallax-corrected cold tops, local cloud flow, projected cold-cloud outlines, growth/decay map, cold-core tracker, IR loop.
- **Radar-style view.** Model rain frames (−1 h … +3 h) fitted to the cold-cloud index of the latest IR scan; a fit that lands on the search limit (±18 km) is not used.
- **Interpretation.** "Weather now" classification, interpretive flags and an event timeline.
- **Observation log.** Enter Rain, Thunder, Dry and Sky observations (UTC); they are joined to the CSV row when you export.
- **CSV log.** One row per run (`ifs_point_log.csv`, about 635 columns) for later analysis.

Every version change is documented in the **Notes** tab of the app and summarised in [CHANGELOG.md](CHANGELOG.md).

## Install (PWA)

- **Android (Chrome):** menu ⋮ → *Install app* (or *Add to Home screen*).
- **iPhone/iPad (Safari):** Share → *Add to Home Screen*.
- **Desktop (Chrome/Edge):** install icon in the address bar.

The service worker caches only the page, the manifest and the icons, so the page opens offline. Weather, satellite and geocoding data always come from the network, so without a connection the page opens but cannot fetch data.

## Publish on GitHub Pages

1. Put all files of this folder in the **root** of the repository (no sub-folders needed).
2. Repository → *Settings* → *Pages* → *Build and deployment* → *Source*: **Deploy from a branch** → branch `main`, folder `/ (root)` → *Save*.
3. After a minute the page is at `https://rakawi182.github.io/MJS-Weather/`.

All paths in the manifest and the service worker are relative, so they work under `/MJS-Weather/`. When you change icons or the manifest, change `VERSION` in `sw.js` so installed copies refresh them. The page itself is fetched network-first and updates on the next load.

## Other coordinates

- **IFS and everything derived from it** (scan, steering wind, ETA, cone, polar map): any coordinate in the world.
- **Satellite features** need the site inside the Himawari-9 disk (sub-satellite point 140.7° E; roughly 80° E to 160° W and about 60° N to 60° S, an approximate range, not checked by the code).
- **Tested only around −7.5° / 112.6°.** The IR tile offset, the grey-to-temperature calibration, the scan-time offset, the parallax assumptions (surface 299 K, 6.5 K/km) and the thresholds were tuned or measured there and have not been verified elsewhere.

## Data, privacy and storage

- Coordinates you enter are sent to **Open-Meteo** (forecast, ensemble, geocoding) and, for the place name, to **BigDataCloud**'s reverse-geocoding client API. Satellite tiles are requested from **JMA** by tile number and IMERG tiles from **NASA GIBS**.
- No analytics, no tracking, no server of its own.
- The browser stores the observation log (`mjs_obs_v1`), calibration caches and the IR history in `localStorage`. **Storage is per origin**: the copy opened from a file and the copy on GitHub Pages do not share it. The observation log has an export (CSV) but no import, and it is not tied to a location, so export and clear it before you switch coordinates in the same browser.

## Files

| File | Purpose |
|---|---|
| `index.html` | The application (MJS Weather C79 plus PWA hooks in the `<head>` and a short script that registers the service worker) |
| `manifest.webmanifest` | Web app manifest (name, colours, icons) |
| `sw.js` | Service worker: caches the app shell only |
| `icon.svg`, `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`, `apple-touch-icon.png`, `favicon.png` | Icons |
| `CHANGELOG.md` | Recent version history |

## Credits and terms

Data belong to their providers: [Open-Meteo](https://open-meteo.com) (ECMWF IFS and other models; attribution required, the free API is for non-commercial use), [JMA](https://www.jma.go.jp) (Himawari-9 imagery), [NASA GIBS](https://earthdata.nasa.gov/gibs) (IMERG), BigDataCloud (reverse geocoding). Check each provider's terms before you reuse the data or publish results.

## License

No license has been chosen yet; until one is added, all rights are reserved by the author.

---

# Bahasa Indonesia

**Halaman cuaca satu berkas (PWA) untuk kedatangan sel hujan dan konveksi di satu titik.**
Mengambil prakiraan titik ECMWF IFS HRES 9 km untuk koordinat mana pun, memindai lingkungan 36 km, menggeser sel hujan ke lokasi dengan beberapa vektor gerak, dan menggabungkannya dengan inframerah Himawari-9 (ubin JMA) untuk mengikuti awan dingin. Semua berjalan di peramban, tanpa server dan tanpa akun.

> **Eksperimental.** Aturan dan ambang bersifat heuristik dan sebagian besar diuji di satu situs di Jawa Timur (November 2026). Ini bukan prakiraan atau peringatan resmi. Jangan dipakai untuk keputusan yang menyangkut keselamatan.

**Fitur:** ekstraksi titik IFS dan model lain; pemindaian lingkungan 3 cincin × 8 arah (R = 36 km) dengan angin steering, CAPE, divergensi, suhu, dan uap air; ETA kedatangan sel (kerucut 300 anggota, peluang 30 dan 60 menit, label ETA dan garis waktu 30/60/90 menit di peta polar); tab Satellite (parallax, aliran awan lokal, proyeksi awan dingin, peta tumbuh/luruh, pelacak inti dingin); tampilan gaya radar dengan fit ke IR (fit yang menyentuh batas ±18 km tidak dipakai); panel interpretasi; log pengamatan (Rain, Thunder, Dry, Sky) yang digabung ke CSV saat diekspor.

**Pasang sebagai aplikasi:** Android (Chrome): menu ⋮ → *Install app*. iPhone (Safari): Bagikan → *Add to Home Screen*. Service worker hanya menyimpan halaman, manifest, dan ikon. Data cuaca, satelit, dan geokode selalu dari jaringan, jadi tanpa koneksi halaman terbuka tetapi tidak bisa mengambil data.

**Publikasi di GitHub Pages:** letakkan semua berkas di root repo → *Settings* → *Pages* → *Deploy from a branch* → `main` / `(root)` → *Save*. Alamatnya `https://rakawi182.github.io/MJS-Weather/`. Kalau ikon atau manifest diganti, ubah `VERSION` di `sw.js`.

**Koordinat lain:** bagian IFS dan turunannya berjalan di koordinat mana pun. Fitur satelit butuh lokasi di dalam piringan Himawari-9 (perkiraan: bujur 80° E sampai 160° W, lintang sekitar 60° U sampai 60° S). Kalibrasi hanya diuji di sekitar −7,5° / 112,6°.

**Data dan penyimpanan:** koordinat dikirim ke Open-Meteo dan (untuk nama tempat) BigDataCloud. Log pengamatan disimpan di `localStorage`, terpisah per alamat (berkas lokal dan GitHub Pages tidak berbagi), bisa diekspor CSV tetapi tidak bisa diimpor, dan tidak terikat lokasi, jadi ekspor dan kosongkan sebelum berganti koordinat.

**Lisensi:** belum ditentukan; sampai ada lisensi, semua hak dipegang penulis.
