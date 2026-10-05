# Riset: Fix 90W Fast Charging POCO F6 (FlameOS AOSP-based)

Date: 2026-10-05
Device: POCO F6 (peridot) — FlameOS (AOSP-based), root KernelSU
ROM Perbandingan: ASCP v6.3-peridot-UNOFFICIAL-20261001-0757 (AOSP-based) vs ROM terpasang (FlameOS)

## Temuan Perbedaan Utama (Penyebab 90W bisa nggak berfungsi di ASCP/AOSP-based)

### 1. Xiaomi MiCharge HAL (vendor.xiaomi.hardware.micharge) — HILANG di ASCP
- Current ROM (FlameOS): 
  - /vendor/lib64/vendor.xiaomi.hardware.micharge-V2-ndk.so ada
  - /vendor/etc/init/vendor.xiaomi.hardware.micharge-service.rc ada
  - /vendor/etc/vintf/manifest/vendor.xiaomi.hardware.micharge.xml ada
- ASCP v6.3: TIDAK ditemukan file2 terkait micharge (lib/bin/rc/xml) di vendor/odm
- Implikasi: charging policy khusus Xiaomi (PD/90W detection, input current limit tuning) via HAL ini tidak ada di ASCP → bisa turun ke detection standar AOSP → 90W tidak ter-trigger/max.

### 2. Charger Config (BAA/Xiaomi Battery AI/Charger Config) — HILANG di ASCP
- Current: /odm/etc/charger/BAA_config_common.json, BAA_config_peridot.json ADA
- ASCP: tidak ada di odm (diextract) → missing battery/charger strategy configs (LowSoh-FvDown, FreqChg-FvDown dsb.) yang bisa mempengaruhi fastcharge behavior.

### 3. Firmware/charger blobs — perbedaan
- Current punya firmware charger: /odm/firmware/211_Charge_RTP.bin, 74_ChargeWire_RTP.bin, 75_ChargeWireless_RTP.bin
- Belum dicek full ASCP, tapi ada indikasi blob charging Xiaomi ada di ROM stock/HyperOS-derived tapi bisa beda di pure AOSP build.

### 4. qcom-battery/sysfs nodes & policy
- Battery model current: N16T_5000mah_90w (power_supply/battery/model_name) — hardware sama.
- Perbedaan utama: missing Xiaomi HAL + configs → adapter detection/input_current_limit bisa tidak di-set optimal untuk 90W PD.

## Kesimpulan Sementara
90W fastcharge bisa bermasalah di ROM AOSP-based murni karena **miCharge HAL (micharge)** + **BAA charger configs** dibuang/tdk diporting. FlameOS masih membawa sebagian dari stack charging Xiaomi (atau adaptasi) sehingga 90W tetap bisa kerja tergantung implementasi.

## Langkah Verifikasi/Test (TODO)
- [ ] Cek adapter_id + input_current_limit + current_max saat colok charger 90W asli (PD)
- [ ] Bandingkan dmesg | grep -i charg saat colok di kondisi berbeda
- [ ] Coba restore micharge HAL + BAA configs dari stock/vendor ke ROM AOSP (jika ingin fix) — perlu hati2 (stabilitas)
- [ ] Dokumentasikan hasil tes dengan charger 90W terverifikasi

## Referensi File
- Current micharge: /vendor/lib64/vendor.xiaomi.hardware.micharge-V2-ndk.so, /vendor/etc/init/vendor.xiaomi.hardware.micharge-service.rc, /vendor/etc/vintf/manifest/vendor.xiaomi.hardware.micharge.xml
- Current BAA: /odm/etc/charger/BAA_config_*.json
