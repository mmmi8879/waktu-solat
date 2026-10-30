# Waktu Solat (Android)

Azan & waktu solat JAKIM (offline), arah kiblat, doa, surah, kisah sahabat, rekod solat dan
**peringatan zikir 3× sehari**. Projek Android Studio (Gradle), `applicationId com.amirul.waktusolat`,
versi 1.7, `minSdk 33`, `targetSdk 36`.

## Buka dalam Android Studio

1. **File → New → Project from Version Control** → tampal URL repo ini → Clone.
   (atau buka folder ini terus dengan **File → Open**)
2. Tunggu *Gradle sync*. Android Studio akan memuat turun Gradle 8.13 dan memasang *SDK Platform 36* jika diminta.
3. Pilih peranti/emulator (Android 13+) → **Run ▶**.

Jika sync merungut tentang versi Android Gradle Plugin, guna **Tools → AGP Upgrade Assistant**.

## Hasilkan AAB untuk Play Store

**Dalam Android Studio:** *Build → Generate Signed App Bundle / APK → Android App Bundle* →
cipta/pilih kunci muat naik → **release** → Finish. Fail: `app/release/app-release.aab`.

**Dari terminal** (selepas `keystore.properties` dibuat daripada `keystore.properties.example`):

```
gradle :app:bundleRelease        # atau ./gradlew jika wrapper sudah dijana
```

Hasil: `app/build/outputs/bundle/release/app-release.aab`.

**GitHub Actions:** tambah secret `UPLOAD_KEYSTORE_BASE64`, `KEY_ALIAS`, `KEYSTORE_PASSWORD`, kemudian
*Actions → Build AAB → Run workflow*. Panduan penuh dan senarai semak Play Console: [`docs/PLAYSTORE.md`](docs/PLAYSTORE.md).

## Struktur

| Laluan | Kandungan |
|---|---|
| `app/src/main/java/com/amirul/waktusolat/` | Kod Java: `MainActivity`, `AlarmScheduler`, `AzanService`, `ZikirScheduler`, `ZikirReceiver`, `ZikirData` … |
| `app/src/main/assets/` | App web (HTML/JS/CSS), data solat, audio surah, gambar masjid |
| `app/src/main/res/` | Ikon, tema, audio azan (`res/raw`) |
| `data/zones/`, `tools/` | Data JAKIM mentah dan skrip kemas kini data tahunan |
| `docs/FEATURES.md` | Penerangan ciri penuh (dokumen lama; arahan bina di dalamnya sudah digantikan oleh fail ini) |

## Nota penting

- **Data waktu solat dalam app sehingga 31 Dis 2026.** Muat turun data 2027 (`tools/fetch_zones.py`, kemudian
  jana semula `embed.js`) dan keluarkan kemas kini sebelum tahun baharu.
- `tools/build_assets.py` menimpa `app/src/main/assets` daripada `../ios-pwa`. Perubahan zikir dalam
  `index.html`, `app.js` dan `app.css` hanya ada dalam repo ini — salin dahulu ke `ios-pwa` sebelum menjalankannya.
- Jangan commit `*.jks` atau `keystore.properties`.
