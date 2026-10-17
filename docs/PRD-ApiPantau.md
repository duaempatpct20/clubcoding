# PRD — ApiPantau

**Peringatan Dini Kebakaran Hutan & Lahan Berbasis Warga**

| | |
|---|---|
| Versi | 1.0 (Draft) |
| Tanggal | 17 Oktober 2026 |
| Status | Draft — menunggu review tim |
| Platform | Mobile (Flutter: Android & iOS), Admin (Web) |
| Konteks | Lomba aplikasi — tema *"Realizing impactful innovation through creative and sustainable technology"* |

---

## 1. Ringkasan Eksekutif

Karhutla (kebakaran hutan dan lahan) di Indonesia berulang setiap musim kemarau, dengan lahan gambut yang terbakar di bawah permukaan hingga berminggu-minggu dan menghasilkan kabut asap yang mengganggu kesehatan jutaan warga.

Sistem pemantauan yang ada (ANTARES, SIPONGI, NASA FIRMS) bersifat **top-down**: mengandalkan titik panas dari satelit yang memiliki latensi beberapa jam, sering *false positive*, dan tidak memberi kanal bagi warga untuk melapor maupun menerima peringatan di tingkat RT/desa.

**ApiPantau** menutup celah tersebut dengan tiga pilar:

1. **Lapor** — warga melaporkan asap/api dalam 10 detik (foto + lokasi otomatis).
2. **Verifikasi** — laporan warga dicocokkan dengan titik panas satelit (FIRMS) dan divalidasi verifikator komunitas; status kejadian ditentukan secara berlapis.
3. **Peringatkan** — pengguna dalam radius tertentu dari kejadian terkonfirmasi menerima notifikasi push, termasuk di area tanpa sinyal (antrean luring).

**Posisi produk:** ApiPantau *bukan* pengganti sistem resmi atau layanan darurat, melainkan **jaringan sensor manusia + satelit** yang membuat deteksi lebih cepat dan peringatan lebih dekat ke rumah warga.

---

## 2. Latar Belakang & Masalah

### 2.1 Konteks

- Karhutla terjadi berulang setiap tahun, umumnya puncaknya Agustus–Oktober, dengan kebakaran gambut yang sulit dipadamkan.
- Titik panas satelit (VIIRS/MODIS) memiliki **latensi umumnya ±3 jam** (hingga 24 jam untuk beberapa produk) — api sudah menyebar sebelum ada laporan.
- Titik panas satelit menghasilkan **false positive** signifikan (kilang gas, kilat, sawit terbakar kecil, awan panas).
- Warga sering menjadi pihak pertama yang melihat asap, tetapi **tidak punya kanal pelaporan yang terstruktur** dan hampir tidak pernah menerima peringatan radius.
- Data dan sistem yang ada (SIPONGI, ANTARES) ditujukan untuk petugas/pemerintah, bukan untuk warga.

### 2.2 Masalah yang Dipecahkan

| # | Masalah | dampak jika dibiarkan |
|---|---|---|
| P1 | Deteksi lambat (berjam-jam) | Api menjalar sebelum ditangani |
| P2 | False positive satelit | Sumber daya pemadaman terbuang, kewaspadaan menurun |
| P3 | Warga tak punya kanal lapor | Informasi lapangan hilang |
| P4 | Warga tak menerima peringatan radius | Rumah/komunitas terkejut saat api dekat |
| P5 | Area kebakaran minim sinyal | Laporan & data tidak tersimpan |

### 2.3 Peluang

- Kepedulian publik tinggi saat musim karhutla → momentum adopsi.
- Data satelit terbuka gratis (NASA FIRMS), cuaca terbuka (BMKG/Open-Meteo).
- Telepon pintar murah + kamera sudah dimiliki hampir semua warga desa.

---

## 3. Tujuan & Target Keberhasilan

### 3.1 Tujuan Produk

- **G1 — Kecepatan:** Mengurangi waktu dari "ada asap" menjadi "warga sekitar tahu" menjadi **< 2 menit** untuk laporan warga (vs berjam-jam untuk data satelit).
- **G2 — Keakuratan:** Menyediakan status kejadian berlapis agar peringatan punya tingkat kepercayaan tinggi (target presisi ≥ 70%).
- **G3 — Jangkauan:** Meningkatkan kesiapsiagaan warga di radius 2–10 km dari kejadian aktif.
- **G4 — Bukti dampak:** Menyediakan data waktu-deteksi dan jumlah laporan yang bisa ditunjukkan ke juri lomba, pemerintah daerah, dan komunitas.

### 3.2 KPI (Key Results)

| KPI | Target MVP (3 bulan pertama) |
|---|---|
| Median waktu laporan → notifikasi terkirim | < 2 menit |
| Median waktu laporan → verifikasi pertama | < 15 menit (jam 07.00–21.00) |
| Presisi laporan terkonfirmasi | ≥ 70% |
| Pengguna aktif bulanan (MAU) | 1.000 |
| Laporan terverifikasi per bulan saat musim kemarau | 300 |
| Retensi D30 pengguna pelapor | ≥ 20% |
| Laporan berhasil dikirim dari mode luring | ≥ 90% (setelah kembali daring) |

---

## 4. Lingkup Produk

### 4.1 Di dalam lingkup MVP

- Aplikasi mobile Flutter (Android prioritas utama, iOS menyusul).
- Peta interaktif: titik panas satelit + laporan warga + status kejadian.
- Alur laporan warga 1-tap (foto + lokasi + catatan) dengan dukungan luring.
- Autentikasi via nomor HP (OTP) atau email.
- Peran: **Warga**, **Verifikator**, **Admin**.
- Notifikasi push radius berdasarkan kejadian terkonfirmasi.
- Integrasi titik panas NASA FIRMS (otomatis, berjadwal).
- Dashboard admin ringkas (statistik laporan, kejadian, waktu verifikasi).

### 4.2 Di luar lingkup MVP (ditunda / tidak dijanjikan)

- ❌ Bukan alat pemadaman atau pengganti layanan darurat 112/113.
- ❌ Tidak memprediksi kebakaran jangka panjang (model FWI) di MVP.
- ❌ Tidak menjadi sistem resmi pelaporan pemerintah (integrasi SIPONGI/ANTARES = pasca-MVP, perlu MoU).
- ❌ Tidak ada web-app publik penuh (hanya dashboard admin).
- ❌ Tidak ada monetisasi/iklan.
- ❌ Tidak ada fitur *chat* sosial, komentar, atau umpan balik publik terbuka (risiko hoaks).

### 4.3 Prinsip Produk

1. **Luring-pertama (offline-first)** untuk pembuatan laporan — area kebakaran justru minim sinyal.
2. **Sederhana di tekanan**: satu tombol utama, dapat dioperasikan sambil panik.
3. **Jujur soal ketidakpastian**: setiap peringatan menampilkan sumber & tingkat keyakinan (warga / satelit / keduanya).
4. **Data terbuka untuk kebaikan**: agregat anonim dapat dibagikan ke peneliti/pemerintah.

---

## 5. Pengguna & Persona

| Persona | Peran | Kebutuhan utama | Kendala |
|---|---|---|---|
| **Rani** — warga desa | Warga | Lapor cepat, tahu apakah bahaya dekat rumahnya | Sinyal lemah, HP kelas menengah, kuota terbatas |
| **Pak Hari** — relawan RT/patrol | Verifikator | Antrean laporan masuk, konfirmasi cepat, koordinasi | Banyak laporan masuk serentak |
| **Dinas BPBD/Admin** | Admin | Gambaran umum, statistik, ekspor data | Butuh ringkasan, bukan detail per piksel |
| **Juri lomba / mitra** | Eksternal | Melihat dampak & inovasi | Butuh demo data yang meyakinkan |

---

## 6. User Story & Acceptance Criteria

### US-01 — Daftar & izinkan lokasi
**Sebagai** warga, **saya ingin** mendaftar dengan nomor HP **sehingga** laporan saya dapat diverifikasi.

- [ ] AC: OTP 6 digit terkirim ≤ 60 detik; verifikasi ≤ 3 percobaan.
- [ ] AC: Saat pertama membuka peta, aplikasi meminta izin lokasi dengan penjelasan manfaat (bukan izin buta).
- [ ] AC: Menolak izin lokasi tetap memungkinkan laporan (lokasi diambil manual di peta).

### US-02 — Melaporkan asap/api (inti)
**Sebagai** warga, **saya ingin** melaporkan asap dengan sekali tekan **sehingga** petugas & tetangga cepat tahu.

- [ ] AC: Dari layar utama, laporan terkirim dalam ≤ 3 langkah: (1) tombol **Lapor**, (2) foto, (3) kirim.
- [ ] AC: Lokasi terisi otomatis dari GPS; pengguna dapat menggeser pin sebelum kirim.
- [ ] AC: Pengisian catatan bersifat opsional dengan kata sugesti ("asap tebal", "sudah ada api").
- [ ] AC: Gambar dikompres (maks. ~200 KB, sisi terpanjang ≤ 1.280 px) sebelum unggah.
- [ ] AC: Konfirmasi "Laporan terkirim" muncul langsung (optimistic), status awal `menunggu_verifikasi`.
- [ ] AC: **Offline**: laporan masuk antrean lokal, otomatis terkirim saat daring kembali, ditandai "terkirim (luring)".

### US-03 — Melihat peta & kejadian di sekitar
**Sebagai** warga, **saya ingin** melihat apakah ada kejadian di sekitar saya **sehingga** saya dapat mengambil tindakan.

- [ ] AC: Peta memuat layer: titik panas satelit, laporan warga, poligon/area kejadian (untuk MVP boleh titik berat + radius).
- [ ] AC: Ketuk marker → panel ringkas: sumber (warga/satelit), waktu, status, tingkat keyakinan.
- [ ] AC: Peta tampil < 3 detik pada koneksi 4G standar dengan cache tile dasar.
- [ ] AC: Pengguna dapat menyetel radius peringatan (2/5/10 km).

### US-04 — Menerima peringatan radius
**Sebagai** warga, **saya ingin** menerima notifikasi saat ada kejadian terkonfirmasi dekat saya **sehingga** saya tidak terkejut.

- [ ] AC: Notifikasi terkirim jika kejadian berstatus `terkonfirmasi` berada dalam radius preferensi pengguna.
- [ ] AC: Isi notifikasi: jenis (asap/api), jarak (≈), waktu, sumber, tombol "Lihat di peta".
- [ ] AC: Tidak ada notifikasi duplikat untuk kejadian yang sama dalam 6 jam (kecuali eskalasi status).
- [ ] AC: Pengguna dapat mematikan jenis notifikasi tertentu (tetapi tidak peringatan status `aktif` di radius < 2 km).

### US-05 — Memverifikasi laporan (Verifikator)
**Sebagai** verifikator, **saya ingin** meninjau laporan masuk **sehingga** peringatan hanya keluar untuk yang benar.

- [ ] AC: Antrean laporan terurut waktu & jarak dari titik kejadian aktif.
- [ ] AC: Aksi: `konfirmasi`, `tolak`, `butuh_info` (minta foto tambahan).
- [ ] AC: Verifikasi mencatat *audit trail*: siapa, kapan, keputusan, catatan.
- [ ] AC: Saat sebuah laporan dikonfirmasi dan berada < 500 m dari hotspot satelit, status naik menjadi `terkonfirmasi_ganda` (keyakinan tinggi) dan **peringatan radius terpicu**.

### US-06 — Admin melihat statistik dampak
**Sebagai** admin, **saya ingin** dashboard ringkas **sehingga** dampak produk dapat dibuktikan.

- [ ] AC: Metrik: jumlah laporan, laporan terkonfirmasi, median waktu verifikasi, kejadian aktif, estimasi pengguna terpapar peringatan.
- [ ] AC: Filter per wilayah & rentang tanggal; ekspor CSV.
- [ ] AC: Grafik tren musim karhutla (mingguan).

---

## 7. Prioritas Fitur (MoSCoW)

### Must (P0 — MVP)
| ID | Fitur |
|---|---|
| F-01 | Peta interaktif dengan layer satelit (FIRMS) + laporan warga |
| F-02 | Alur laporan 1-tap (foto + lokasi + catatan) |
| F-03 | Antrean laporan luring & sinkronisasi otomatis |
| F-04 | Autentikasi OTP/email + peran (Warga/Verifikator/Admin) |
| F-05 | Antrean verifikasi + audit trail |
| F-06 | Notifikasi push radius (FCM) |
| F-07 | Ingest otomatis titik panas FIRMS (jadwal) |
| F-08 | Status kejadian (state machine) & panel detail |
| F-09 | Dashboard admin ringkas |
| F-10 | Pengaturan profil: radius peringatan, preferensi notifikasi |

### Should (P1 — jika waktu cukup)
| ID | Fitur |
|---|---|
| F-11 | AI validasi foto (asap/api vs awan/lainnya) sebagai penanda awal |
| F-12 | Indeks risiko harian dari data cuaca (hujan, kelembapan, angin) |
| F-13 | Kualitas udara/kabut (AQI) di sekitar pengguna |
| F-14 | Pengelompokan otomatis laporan + hotspot menjadi satu "kejadian" |
| F-15 | Halaman statistik pengguna (kontribusi, poin sederhana) |
| F-16 | Mode hemat kuota (peta satelit only saat diminta, kompresi agresif) |

### Could (P2 — pasca lomba)
| ID | Fitur |
|---|---|
| F-17 | Bot WhatsApp untuk laporan tanpa aplikasi |
| F-18 | Integrasi API SIPONGI/ANTARES & koordinasi BPBD |
| F-19 | Peta sumber air terdekat (kombinasi ide "AirTerdekat") |
| F-20 | Gamifikasi komunitas (badge, papan peringkat desa) |
| F-21 | Pelaporan pemulihan pasca-kebakaran |
| F-22 | Web app publik (read-only) |

---

## 8. Kebutuhan Fungsional (Detail)

### 8.1 Alur Status Laporan

```
[menunggu_verifikasi] ──verifikator konfirmasi──▶ [terkonfirmasi] ──selesai──▶ [ditangani]
         │                                              │
         ├──tolak──▶ [ditolak]                          └──<500m dari hotspot──▶ [terkonfirmasi_ganda]
         └──butuh_info──▶ [menunggu_verifikasi]
```

### 8.2 Alur Status Kejadian (Incident)

```
[terpantau] ──min. 1 terkonfirmasi──▶ [aktif] ──verifikator──▶ [terkendali] ──24 jam tanpa aktivitas──▶ [selesai]
                                    └──eskalasi (luas/keyakinan naik)──▶ notifikasi ulang radius
```

### 8.3 Aturan Notifikasi

| Pemicu | Penerima | Syarat |
|---|---|---|
| Kejadian masuk `aktif` | Pengguna dalam radius (default 5 km) | Keyakinan ≥ "terkonfirmasi" |
| Eskalasi status kejadian | Pengguna dalam radius | Anti-spam 6 jam |
| Laporan baru di radius pengguna | Verifikator & admin | — |
| Kedatangan hotspot satelit baru | Hanya verifikator/admin | Bukan push ke warga (hindari spam/false positive) |

Aturan teknis:
- Notifikasi dikirim via **FCM HTTP v1** dari backend (Edge Function), bukan dari aplikasi.
- Setiap pengiriman dicatat ke tabel `notification_log` (idempotensi + audit).
- Preferensi pengguna disimpan per jenis notifikasi.

### 8.4 Ingest Titik Panas Satelit

- Sumber utama: **NASA FIRMS** (API area/buffer dengan `MAP_KEY` gratis).
- Jadwal: setiap 30 menit saat musim rawan (Mei–Oktober), setiap 3 jam di luar itu (dapat dikurangi untuk hemat kuota).
- Deduplikasi: kunci unik = `source + satellite + acq_date + acq_time + lat/lon (rounded 4 decimal)`.
- Field yang disimpan: `source, satellite, bright_ti4/ti5, confidence, frp, acq_date, acq_time, geom`.
- Produk: VIIRS (prioritas, cakupan lebih baik), MODIS (pelengkap).

### 8.5 AI Validasi Foto (P1)

- Input: foto laporan → klasifikasi: `asap`, `api`, `awan/normal`, `tidak relevan`.
- Output digunakan **hanya sebagai penanda** (boost/flag), **tidak** menggantikan verifikator.
- Implementasi: model *image classification* ringan (on-device TFLite) untuk mode luring + API cloud (mis. Gemini/Vision) untuk skoring server-side saat daring.
- Metrik: akurasi ≥ 80% pada uji 100 foto campuran (aset dari laporan nyata + stok).

---

## 9. Kebutuhan Non-Fungsional

| Kategori | Kebutuhan |
|---|---|
| **Performa** | API p95 < 500 ms (tanpa tile peta); peta tampil < 3 s; laporan terkirim (daring) < 5 dtk |
| **Luring** | Pembuatan laporan & pembacaan cache peta terakhir berfungsi tanpa sinyal; sinkronisasi otomatis saat daring |
| **Skalabilitas** | Mendukung 10.000 pengguna terdaftar, 1.000 MAU, 50 laporan/menit saat puncak musim |
| **Ketersediaan** | 99,5% bulanan untuk API; peta satelit bergantung provider eksternal (punya fallback tile) |
| **Ukuran aplikasi** | APK ≤ 40 MB; dukungan Android 8.0+ (target perangkat kelas menengah) |
| **Hemat kuota** | Kompresi gambar otomatis; tile peta di-cache; ingest satelit dijadwalkan (bukan polling dari klien) |
| **Keamanan** | RLS di seluruh tabel; rate limit per user (mis. 10 laporan/jam); token FCM tersimpan terenkripsi; tanpa penyimpanan kredensial eksternal di klien |
| **Privasi** | Sesuai UU No. 27/2022 (PDP): lokasi laporan = data publik kejadian, lokasi rumah pengguna tidak ditampilkan; EXIF foto dibersihkan setelah ekstraksi |
| **Aksesibilitas** | Kontras memadai, target teks ≥ 14 sp, tombol utama ≥ 48 dp, mode gelap |
| **Bahasa** | Bahasa Indonesia (default), siap untuk i18n |
| **Observabilitas** | Logging terstruktur, crashlytics, metrik notifikasi terkirim/gagal |

---

## 10. Arsitektur Sistem

```mermaid
flowchart LR
  subgraph Klien
    APP[Flutter App\nAndroid/iOS]
    ADMIN[Dashboard Admin\nWeb]
  end

  subgraph Backend["Backend: Supabase (rekomendasi)"]
    AUTH[Auth\nOTP/Email]
    REST[PostgREST API]
    DB[(PostgreSQL + PostGIS)]
    STOR[Storage\nfoto laporan)]
    RT[Realtime\nfeed laporan]
    EF[Edge Functions\nDeno/TS]
    CRON[pg_cron / Scheduler]
  end

  subgraph Eksternal
    FIRMS[NASA FIRMS API]
    WX[BMKG / Open-Meteo]
    FCM[Firebase Cloud Messaging]
    AI[AI Vision API P1]
    TILE[Tile: OSM / Esri]
  end

  APP --> AUTH & REST & STOR & RT
  ADMIN --> REST
  CRON --> EF
  EF --> FIRMS & WX & FCM & AI
  EF --> DB
  REST --> DB
  STOR --> DB
  APP -. push .- FCM
  APP -. tile .- TILE
```

### 10.1 Stack Teknis

**Klien mobile (Flutter)**
- State management: **Riverpod** (rekomendasi) atau Bloc.
- Peta: **flutter_map** + tile OSM (dasar) & Esri World Imagery (satelit).
- Lokasi: `geolocator`; Peta & marker: `latlong2`.
- Luring: `drift` (SQLite) untuk antrean laporan & cache.
- Push: `firebase_messaging`.
- Supabase: `supabase_flutter`.
- Foto: `image_picker` + `flutter_image_compress`.

**Backend — lihat §11 untuk rekomendasi lengkap.**

---

## 11. Rekomendasi Backend

### Keputusan: **Supabase** (utama)

| Kriteria (bobot) | Supabase | Firebase | Custom (NestJS+Postgres) |
|---|---|---|---|
| Query geospasial (radius, clustering) — 30% | ★★★★★ PostGIS asli: `ST_DWithin`, `ST_ClusterDBSCAN` | ★★☆☆☆ perlu trik geohash, limit 1 dokumen | ★★★★★ |
| Kecepatan MVP — 20% | ★★★★☆ | ★★★★★ | ★★☆☆☆ |
| Model data relasional (laporan↔kejadian↔hotspot↔user) — 20% | ★★★★★ SQL + RLS | ★★★☆☆ butuh denormalisasi & fan-out | ★★★★★ |
| Push notification — 15% | ★★★★☆ via Edge Function → FCM v1 (perlu setup) | ★★★★★ native | ★★★☆☆ |
| Biaya (skala lomba → 10k user) — 10% | ★★★★★ free tier murah hati | ★★★★☆ (Firestore bisa melonjak) | ★★☆☆☆ (VPS + ops) |
| Kurva belajar tim — 5% | ★★★★☆ | ★★★★★ (materi ID lebih banyak) | ★★★☆☆ |
| **Skor tertimbang** | **≈ 4,55** | **≈ 3,75** | **≈ 4,15** |

**Alasan utama memilih Supabase:**
1. **PostGIS** — fitur inti produk adalah pertanyaan geospasial ("kejadian dalam radius 5 km dari saya", "kelompokkan 40 titik jadi 1 kejadian"). Ini mahal dan rumit di Firestore.
2. **Relasional & SQL** — laporan, hotspot, kejadian, verifikasi, notifikasi semuanya saling terkait; dashboard statistik jadi satu query.
3. **Satu layanan** mencakup Auth, Storage, Realtime, Edge Functions, dan Database.
4. **Row Level Security** cocok untuk model peran (Warga/Verifikator/Admin) tanpa menulis lapisan otorisasi kustom.

**Stack final yang direkomendasikan:**

| Lapisan | Pilihan | Alasan |
|---|---|---|
| Mobile | Flutter + Riverpod + flutter_map | Sesuai keputusan tim |
| Backend/BaaS | **Supabase** (Postgres+PostGIS, Auth, Storage, Realtime, Edge Functions, pg_cron) | Geospasial & relasional |
| Push | **Firebase Messaging (FCM HTTP v1)** dari Supabase Edge Function | Standard, gratis, andal — boleh hybrid Supabase+FCM |
| Tile peta | OpenStreetMap (dasar) + Esri World Imagery (satelit) | Tanpa biaya API key di MVP |
| Satelit | NASA FIRMS (MAP_KEY) | Data resmi, gratis |
| Cuaca (P1) | Open-Meteo (mulai) → BMKG dataonline (P1, butu daftar token) | Open-Meteo tanpa kunci, cepat integrasi |
| AI foto (P1) | TFLite on-device + API cloud (Gemini/Vision) | Hemat kuota & biaya |

**Kapan memilih alternatif lain:**
- **Firebase** — jika tim sudah sangat familiar dengannya dan target hanya 1 kota demo; solusi geospasial disederhanakan (geohash + radius kasar) dan clustering dilakukan di klien.
- **Custom (NestJS/Go + Postgres)** — jika butuh logika bisnis sangat kompleks atau kontrol penuh; **tidak disarankan untuk lomba** karena makan waktu 1–2 minggu ekstra untuk hal non-diferensiasi.

---

## 12. Model Data (ringkas)

```sql
-- Profil & peran
profiles(id uuid PK, phone/text, name text, role text -- 'warga'|'verifikator'|'admin'
       , home_geom geography(Point,4326) null
       , alert_radius_km int default 5, fcm_token text, created_at timestamptz)

-- Laporan warga
reports(id uuid PK, user_id uuid FK profiles
      , geom geography(Point,4326), description text
      , photos text[]                     -- path di Storage
      , status text -- menunggu_verifikasi|terkonfirmasi|terkonfirmasi_ganda|ditolak|ditangani
      , source text default 'warga'
      , ai_label text null, ai_score numeric null
      , verified_by uuid null, verified_at timestamptz null, verify_note text
      , offline_created_at timestamptz null, created_at timestamptz default now())
-- INDEX: GIST(geom), btree(created_at), btree(status)

-- Titik panas satelit
hotspots(id uuid PK, source text, satellite text, bright_ti4 numeric, bright_ti5 numeric
       , confidence text, frp numeric, acq_date date, acq_time time
       , geom geography(Point,4326) NOT NULL, dedup_key text UNIQUE, created_at timestamptz)
-- INDEX: GIST(geom), btree(acq_date)

-- Kejadian (agregasi)
incidents(id uuid PK, centroid geography(Point,4326), radius_m numeric
        , status text -- terpantau|aktif|terkendali|selesai
        , confidence text, first_seen timestamptz, last_seen timestamptz
        , contained_at timestamptz null, summary text)

-- Relasi kejadian ↔ bukti
incident_evidence(incident_id uuid FK, report_id uuid null, hotspot_id uuid null, linked_at timestamptz)

-- Audit verifikasi & log notifikasi
verifications(id uuid PK, report_id uuid FK, actor_id uuid, action text, note text, created_at timestamptz)
notification_log(id uuid PK, incident_id uuid, user_id uuid, fcm_message_id text,
                 status text, created_at timestamptz, UNIQUE(incident_id, user_id))
```

**Kebutuhan geospasial kunci (PostGIS):**
- Radius: `ST_DWithin(reports.geom, $user_point, $radius_m)`
- Clustering laporan+hotspot: `ST_ClusterDBSCAN(geom, eps := 750, minpoints := 2)`
- bbox peta: `geom && ST_MakeEnvelope($minx,$miny,$maxx,$maxy, 4326)`

---

## 13. Kontrak API (ringkas)

| Metode | Endpoint | Keterangan |
|---|---|---|
| GET | `/rest/v1/reports?bbox=...&status=eq.terkonfirmasi` | Laporan dalam viewport peta |
| POST | `/rest/v1/reports` | Buat laporan (upsert foto terpisah) |
| POST | `/rest/v1/storage/object/report-photos/{id}` | Unggah foto terkompres |
| PATCH | `/rest/v1/reports/{id}` | Verifikator: ubah status (RLS: role verifikator) |
| GET | `/rest/v1/hotspots?bbox=...&acq_date=gte.{7d}` | Titik panas satelit |
| GET | `/rest/v1/rpc/incidents_near?lat=..&lng=..&radius_km=..` | Kejadian terdekat (ST_DWithin) |
| GET | `/rest/v1/rpc/dashboard_stats?from=..&to=..` | Statistik admin |
| POST | `/functions/v1/ingest-firms` | Cron: tarik FIRMS, dedup, clustering, notifikasi |
| POST | `/functions/v1/notify-incident` | FCM HTTP v1 ke radius |
| POST | `/functions/v1/classify-photo` | (P1) skoring AI foto |

---

## 14. Milestone (asumsi 5 minggu menuju lomba)

| Minggu | Milestone | Keluaran |
|---|---|---|
| M1 | Fondasi | Repo, Supabase project + skema & RLS, auth OTP, peta dasar dengan tile |
| M2 | Laporan | Alur laporan + foto + antrean luring, daftar laporan saya |
| M3 | Peringatan | Ingest FIRMS berjadwal, panel kejadian, notifikasi radius (FCM), audit log |
| M4 | Verifikasi & Admin | Antrean verifikator, dashboard admin, statistik dampak |
| M5 | Penguatan | Uji coba lapangan 1 RT/desa, perbaikan bug, data demo, deck presentasi |

**Definition of Done (DoD) MVP:** seluruh AC US-01..US-06 lulus uji manual; APK terpasang di 2 perangkat berbeda; demo end-to-end (lapor → verifikasi → notifikasi) berjalan < 60 detik tanpa crash.

---

## 15. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|---|---|---|
| Latensi satelit 3–24 jam membuat "peringatan dini" dipandang menyesatkan | Tinggi | Komunikasikan jujur: satelit = konfirmasi, **warga = deteksi cepat**; jadikan ini narasi produk |
| False positive satelit memicu kepanikan | Tinggi | Push radius hanya untuk status `terkonfirmasi`; hotspot mentah hanya tampil di peta bertanda |
| Adopsi warga rendah | Tinggi | Kemitraan dengan RT/RW, sekolah, komunitas relawan; kampanye musim kemarau |
| Laporan palsu/jahat | Sedang | Rate limit, verifikator, reputasi pengguna (P2), penandaan lokasi tidak wajar |
| Area tanpa sinyal | Sedang | Antrean luring + auto-retry + indikator status; hindari fitur yang wajib daring |
| Kuota API/key berubah (FIRMS, FCM) | Sedang | Kunci disimpan di backend saja; fallback ke feed CSV harian |
| Biaya Supabase/Firebase melonjak | Rendah | Batasi ingest sesuai jadwal, kompresi gambar, monitor usage |
| Persepsi "bukan sistem resmi" | Sedang | Tampilkan sumber & status data transparan; roadmap integrasi BPBD |

---

## 16. Peta Dampak (untuk presentasi lomba)

| Dimensi lomba | Pemenuhan di ApiPantau |
|---|---|
| **Impactful** | Memangkas waktu deteksi dari jam → menit; menyelamatkan harta & kesehatan (kabut); KPI terukur (§3.2) |
| **Creative** | Menggabungkan satelit + laporan warga + AI verifikasi dalam satu *confidence layer*; notifikasi radius geospasial |
| **Sustainable technology** | Memakai infrastruktur sederhana hemat energi (ingest terjadwal, kompresi, mode luring); mendukung SDGs 13 (Iklim), 15 (Ekosistem Daratan), 3 (Kesehatan), 11 (Kota Berkelanjutan), 17 (Kemitraan) |

---

## 17. Pertanyaan Terbuka (perlu keputusan tim)

1. Platform prioritas: **Android dulu** atau Android+iOS bersamaan? *(rekomendasi: Android dulu)*
2. Siapa yang menjadi **verifikator MVP** (relawan komunitas? anggota tim saja untuk demo)?
3. Nama produk final — "ApiPantau" masih placeholder.
4. Perlu **kemitraan pilot** 1 desa/RT untuk uji coba nyata? *(sangat meningkatkan nilai lomba)*
5. Apakah lomba mensyaratkan open source? *(jika ya, lisensi MIT + dokumentasi)*

---

*PRD ini hidup — perbarui setiap kali ada keputusan baru. Versi berikutnya: v1.1 setelah FGD internal.*
